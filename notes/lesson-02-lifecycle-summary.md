# 第 2 讲总结：请求生命周期与 Sequence

## 本节目标

第 2 讲的目标是理解一条用户请求在 nano-vLLM 内部如何变成 `Sequence`，并从 `WAITING` 走到 `RUNNING`，最后变成 `FINISHED`。

本节课不是调度器和 KV cache 的深挖课。`schedule()` 和 `BlockManager` 只讲到能解释生命周期为止，细节放到第 3 讲和第 4 讲。

## 读过的代码

- `nanovllm/engine/llm_engine.py`
- `nanovllm/engine/sequence.py`
- `nanovllm/engine/scheduler.py` 的状态流部分

## 主流程

一条请求的大致生命周期：

1. 用户 prompt 进入 `generate()`
2. `add_request()` 把 prompt 编码成 token id 列表
3. `Sequence(prompt, sampling_params)` 创建请求对象
4. `scheduler.add(seq)` 把请求放进 `waiting`
5. `schedule()` 优先从 `waiting` 里做 prefill
6. prompt 相关 token 处理完后，`seq.status = RUNNING`
7. decode 阶段每轮通常生成 1 个 token
8. `postprocess()` 通过 `append_token()` 把生成 token 写回 `Sequence`
9. 命中 eos 或达到 `max_tokens` 后，`seq.status = FINISHED`
10. 释放 KV cache，并从 `running` 中移除

## 关键概念

- `Sequence` 表示一条用户请求，不是随机切出来的小段
- `token_ids` 是 prompt token 和 generated token 的完整轨迹
- `prompt_token_ids` 是用户输入部分
- `completion_token_ids` 是模型生成部分
- `seq_id` 是请求编号，不是 token 编号
- `num_prompt_tokens` 表示 prompt token 数
- `num_tokens` 表示当前总 token 数
- `num_completion_tokens = num_tokens - num_prompt_tokens`
- `num_scheduled_tokens` 表示本轮要处理多少 token
- `num_cached_tokens` 表示已经进入 KV cache 的 token 数

## 重要纠偏

- prefill 不是批量生成回答 token，而是处理 prompt 并建立上下文缓存
- decode 才是模型逐 token 生成回答的阶段
- chunked prefill 不会创建多个新 `Sequence`，只是同一个 `Sequence` 分多轮处理
- `scheduled_seqs.append(seq)` 只是把同一个 `Sequence` 放入本轮执行列表
- `BlockManager.can_allocate(seq)` 只是判断前缀缓存和资源是否可用，真正建立 `block_table` 的是 `allocate()`
- `num_cached_blocks` 表示能复用几个旧 block，新分配数量来自 `seq.num_blocks - num_cached_blocks`

## 结束条件

单条请求的结束逻辑在 `Scheduler.postprocess()`：

```python
if (not seq.ignore_eos and token_id == self.eos) or seq.num_completion_tokens == seq.max_tokens:
    seq.status = SequenceStatus.FINISHED
    self.block_manager.deallocate(seq)
    self.running.remove(seq)
```

这表示：

- 如果生成到 eos，且没有忽略 eos，就结束
- 或者生成 token 数达到 `max_tokens`，也结束
- 结束后释放缓存，并从 `running` 队列移除

整个 `generate()` 结束则由 scheduler 判断：

```python
return not self.waiting and not self.running
```

也就是所有请求都不在 waiting 和 running 中时，整体生成才结束。

## 本节边界

本节没有深入：

- `schedule()` 的完整调度策略
- preemption 细节
- prefix cache 的 hash 机制
- `BlockManager.allocate()` 的完整实现
- KV cache block 的物理布局

这些内容放到第 3 讲和第 4 讲。

## 验收结果

学习者已经能够说明：

- 用户 prompt 如何变成 `Sequence`
- `Sequence` 为什么表示一条请求的生命周期
- `WAITING -> RUNNING -> FINISHED` 的状态变化
- prefill 和 decode 的基本区别
- 为什么同一个 `Sequence` 可能被多轮调度
- 单条请求和整个 `generate()` 分别如何结束

第 2 讲通过。

## 下一讲

第 3 讲进入调度器：

- 系统拆解 `Scheduler.schedule()`
- 理解为什么先处理 `waiting`
- 理解 prefill 和 decode 调度分支
- 手工模拟 2 到 3 个请求的调度过程
