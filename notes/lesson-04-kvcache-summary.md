# 第 4 讲总结：KV cache 与 BlockManager

## 本节目标
这一讲的目标是把“逻辑上的 block 管理”与“物理上的 KV cache 内存”连起来，彻底弄懂：

- `num_kvcache_blocks` 是怎么来的
- `BlockManager` 是怎么做分配、复用、回收的
- `seq.block_table`、`slot_mapping`、`slot` 分别表示什么
- `prepare_prefill()` / `prepare_decode()` 如何把 token 送进真实 KV cache

## 读过的代码
- `nanovllm/engine/block_manager.py`
- `nanovllm/engine/sequence.py`
- `nanovllm/engine/model_runner.py`

## 这节课的主线

### 1. 先理解物理 KV cache 怎么铺出来
在 `ModelRunner.allocate_kv_cache()` 里，系统会先根据 GPU 剩余显存、模型层数、block size、KV head 数量、head dim 这些因素，计算出这张卡能给 KV cache 留出多少空间。

关键点是：

- `block_bytes` 不是单层单 token 的大小，而是“一个 logical block id 在所有层上的总成本”
- `num_kvcache_blocks` 不是“全模型共用几个 block”，而是“整套 KV cache 能支持多少个 block 编号”
- 真正的 cache 会被展开成：
  - `2` 份：K 和 V
  - `num_hidden_layers` 层
  - 每层 `num_kvcache_blocks` 个 block
  - 每个 block 里有 `block_size` 个 token 槽位

也就是说，`block_id` 只是逻辑编号，真正的物理内存是 `kv_cache` 里的切片。

### 2. 再理解 BlockManager 在管什么
`BlockManager` 管的是“哪个逻辑 block 还能用、哪个还能复用、哪个该回收”。

它最重要的几个字段：

- `free_block_ids`：空闲池
- `used_block_ids`：正在被引用的 block
- `ref_count`：有多少条 seq 共享这个 block
- `hash_to_block_id`：前缀缓存索引表

关键方法：

- `can_allocate(seq)`：先判断前缀缓存能命中几个 block，以及剩余资源够不够
- `allocate(seq, num_cached_blocks)`：把命中的旧 block 接到 `seq.block_table`，剩下的再从空闲池申请
- `hash_blocks(seq)`：把已经处理完的完整 block 写入 hash 索引
- `deallocate(seq)`：引用计数减到 0 后回收到 free list

这一层的核心结论是：

**`BlockManager` 管的是逻辑 block；`kv_cache` 才是物理内存。**

### 3. `prepare_block_tables()` 做了什么
`seq.block_table` 是一个 seq 自己的 block id 列表，但 batch 里每条 seq 长度不同，不能直接喂给模型。

所以 `prepare_block_tables()` 会把多个 `seq.block_table` 补齐成一个二维 tensor：

- 第 0 维：第几个 seq
- 第 1 维：这个 seq 的第几个逻辑 block
- 值：对应的物理 `block_id`

补位用 `-1`，表示没有真实 block。

### 4. `prepare_prefill()` 怎么把 prompt 写进 cache
prefill 阶段处理的是整段 prompt。这里最重要的是 `slot_mapping`。

它的作用是：

**把每一个 token 精确映射到 KV cache 的某个物理槽位。**

流程可以拆成四步：

1. 先取当前 seq 已缓存的位置 `start`
2. 再算这轮要处理到哪里 `end`
3. 把 token 区间换算成 block 区间
4. 再把 block 区间展开成逐 token 的物理槽位编号

这里最容易混淆的地方已经讲清楚了：

- `block_table` 只表示逻辑 block 到物理 block id 的映射
- `slot_mapping` 才是 token 到物理 cache 槽位的映射
- `slot` 可以理解成“token 在 KV cache 里的物理位置编号”

所以 `prepare_prefill()` 的本质不是算模型，而是：

**先把 token 和 cache 槽位一一对应好，再把这些信息交给 attention 去真正写入 KV cache。**

### 5. `prepare_decode()` 怎么接着往后推
decode 阶段只喂最后一个 token，不再把整段 prompt 重算一遍。

这一步主要做四件事：

- `input_ids` 只取 `seq.last_token`
- `positions` 取最后一个 token 的绝对位置
- `context_lens` 记录这条 seq 当前的历史长度
- `slot_mapping` 记录这个最后 token 应该写到哪个 KV cache 槽位

这里的关键公式是：

```python
seq.block_table[-1] * self.block_size + seq.last_block_num_tokens - 1
```

意思是：

- 找到最后一个逻辑 block 对应的物理 block id
- 再加上它在 block 里的偏移
- 得到这个 token 的物理槽位

`prepare_decode()` 的本质就是：

**只处理最后一个 token，然后把它接到已有的历史 cache 上继续生成。**

## 这节课里已经纠正清楚的点

- `num_kvcache_blocks` 是容量计算结果，不是“所有层共享的一个 block 数”
- `block_bytes` 已经把 `num_hidden_layers` 和 `block_size` 算进去了
- `BlockManager` 管的是逻辑 block，不是真正的张量内存
- `slot_mapping` 不是 block 表，而是 token 到物理 cache 槽位的映射
- `slot` 是槽位编号，不是原始指针地址
- `last_block_num_tokens - 1` 是因为要把“数量”转成“最后一个位置下标”

## 这节课不再展开的边界
为了不把节奏带散，这一讲到这里先不深挖：

- `slot -> cache_offsets -> tl.store` 的 kernel 细节
- `Attention.forward()` 里的底层 FlashAttention 调用参数
- prefill / decode 里的具体算子实现

这些会留到后面的 attention / kernel 课继续讲。

## 这节课的课堂注释
你已经在代码里加了教学注释，主要落在：

- `nanovllm/engine/model_runner.py`
- `nanovllm/engine/block_manager.py`

这些注释会作为第 4 讲的“代码版讲义”，方便未来换电脑后直接读代码续课。

## 本节结论
第 4 讲已经把主线串起来了：

**`BlockManager` 负责逻辑 block 的复用与回收，`ModelRunner` 负责把这些逻辑 block 翻译成物理 KV cache，`prepare_prefill()` / `prepare_decode()` 负责把 token 精确落到 cache 槽位。**

## 下一讲
第 5 讲开始进入模型结构与 attention：

- `nanovllm/models/qwen3.py`
- `nanovllm/layers/attention.py`
- `nanovllm/layers/rotary_embedding.py`
