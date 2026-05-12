# 第 4 讲总结：KV cache 与 BlockManager

## 本节目标

第 4 讲的目标是看懂 KV cache block 是怎么分配、复用、回收的，以及 prefix cache 为什么能命中。  
这一讲不把重点放在 attention 算子本身，而是放在 `BlockManager` 如何支撑调度器和运行时。

## 读过的代码

- `nanovllm/engine/block_manager.py`
- `nanovllm/engine/sequence.py`
- `nanovllm/engine/scheduler.py` 的少量辅助逻辑

## 主流程

`BlockManager` 主要负责四件事：

1. 判断一条 seq 能不能分配 block
2. 尽量复用已经缓存过的前缀 block
3. 为剩余部分分配新的 block
4. 在 seq 结束或被 preempt 时回收 block

核心配合关系是：

- `can_allocate(seq)`：先判断前缀缓存能命中多少 block，以及当前资源是否足够
- `allocate(seq, num_cached_blocks)`：把命中的旧 block 接上，再给后面的部分补新 block
- `hash_blocks(seq)`：在 prefill 过程中把已经处理完的 block 记录进缓存映射
- `deallocate(seq)`：把 seq 占着的 block 释放回去

## 关键概念

- `block_size`：一个 KV cache block 能容纳多少 token
- `seq.num_blocks`：这条 seq 当前总共需要多少个 block
- `seq.block_table`：这条 seq 使用了哪些 block
- `seq.num_cached_tokens`：已经进入 KV cache 的 token 数
- `hash_to_block_id`：prefix cache 的索引表
- `used_block_ids`：当前已经被引用占用的 block
- `free_block_ids`：当前还可用的 block

## 重要纠偏

- `can_allocate(seq)` 不是分配缓存，而是先判断前缀命中和资源是否够用
- `num_cached_blocks` 表示命中了多少个旧 block，不会在 `allocate()` 里自己变化
- 新分配数量不是单独传进去的，而是由 `seq.num_blocks - num_cached_blocks` 隐含决定
- `allocate(seq, num_cached_blocks)` 会同时做两件事：
  - 复用前缀缓存 block
  - 给剩余部分新建 block
- `block_table` 的建立不是随机的，而是按 token 前缀顺序对应 block
- prefix cache 命中的是“前缀 token 对应的 block 是否已缓存且内容一致”，不是随便相似就算命中
- `deallocate(seq)` 是按 block 反向回收的

## prefix cache 命中

prefix cache 命中的意思是：

> 这条 prompt 的前缀部分，之前已经作为 block 被缓存过，并且当前 token 内容完全一致，所以可以直接复用对应的 KV cache。

在实现里，`can_allocate(seq)` 会：

- 依次取 `seq.block(i)`
- 计算 block hash
- 去 `hash_to_block_id` 里查对应 block
- 再比较 token 内容是否完全一致

只要前面若干个 block 全部命中，就可以复用这段缓存。

## block 分配和回收

### 分配

```python
num_cached_blocks = self.block_manager.can_allocate(seq)
self.block_manager.allocate(seq, num_cached_blocks)
```

含义是：

- 先算出能复用几个旧 block
- 再把这些旧 block 接到 `seq.block_table`
- 剩余没命中的部分再申请新 block

### 回收

```python
self.block_manager.deallocate(seq)
```

含义是：

- 释放该 seq 占用的 block
- 若某个 block 的引用数降到 0，就回到 free list
- `seq.num_cached_tokens` 也会被清零
- `seq.block_table` 会被清空

## 与调度器的关系

这一讲和第 3 讲是连着的：

- 第 3 讲里，调度器通过 `can_allocate()` 判断一条 seq 这一轮能不能安排 prefill
- 如果能安排，就通过 `allocate()` 建立 block_table
- 如果 decode 阶段 KV cache 空间不够，就可能触发 `preempt()`，把 running 请求打回 waiting，并调用 `deallocate()`

所以 `BlockManager` 是调度器和模型执行之间的缓存支撑层。

## 验收结果

学习者已经能够说明：

- block 是怎么分配、复用、回收的
- prefix cache 命中的基本含义
- `hash_to_block_id`、`used_block_ids`、`free_block_ids` 的关系
- `num_cached_blocks` 与 `seq.num_blocks` 的关系
- `can_allocate()` 和 `allocate()` 的职责区别
- 为什么 block size 会影响吞吐和碎片

第 4 讲通过。

## 下一讲

第 5 讲进入模型结构和 attention：

- 看 Qwen3 decoder layer 的前向流程
- 看 RoPE、RMSNorm、Attention、MLP 的组合
- 理清模型层和缓存层如何衔接
