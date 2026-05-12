# 第 3 讲总结：调度器 Scheduler

## 本节目标

第 3 讲的目标是彻底看懂 `Scheduler.schedule()`：它为什么先处理 `waiting`，为什么 prefill 优先，什么时候进入 decode，以及什么时候会触发 preempt。

本节课不深挖 `BlockManager` 的 hash 细节和 KV cache 物理布局，只把它当作调度器判断资源是否可用的辅助模块。

## 读过的代码

- `nanovllm/engine/scheduler.py`
- `nanovllm/engine/sequence.py`

## 主流程

`Scheduler` 维护两个队列：

- `waiting`：还在做 prompt 处理的请求
- `running`：prompt 已经 prefill 完、可以进入 decode 的请求

`schedule()` 的返回值是：

```python
scheduled_seqs, is_prefill
```

- `scheduled_seqs`：这一轮真正要送去模型执行的 `Sequence` 列表
- `is_prefill`：这一轮是 prefill 还是 decode

调度顺序大致是：

1. 先看 `waiting`
2. 尽量做 prefill
3. 如果本轮有 prefill 任务，直接返回 `True`
4. 没有 prefill 时，才看 `running`
5. decode 阶段每条 seq 每轮只推进 1 个 token

## 关键概念

- `max_num_batched_tokens` 控制这一轮最多处理多少 token
- `max_num_seqs` 控制这一轮最多处理多少条 seq
- `seq.num_scheduled_tokens` 表示这一轮给某条 seq 安排了多少 token
- `seq.num_cached_tokens` 表示已经进入 KV cache 的 token 数
- `seq.num_tokens` 表示这条 seq 当前的总 token 数
- `seq.num_cached_tokens + seq.num_scheduled_tokens == seq.num_tokens` 表示这条 seq 的 prompt 已经 prefill 完成

## 重要纠偏

- `scheduled_seqs.append(seq)` 放进去的是同一个 `Sequence` 对象引用，不是副本
- `seq = self.waiting[0]` 表示取队首请求，调度器按 FIFO 顺序处理
- `waiting` 和 `running` 是不同阶段的队列，不是不同对象的拷贝
- prefill 不是生成回答 token，而是处理 prompt 并把上下文灌入 KV cache
- decode 才是根据上下文一轮一轮生成回答 token
- chunked prefill 不会创建新的 `Sequence`，只是同一个 `Sequence` 分多轮处理
- `remaining < num_tokens and scheduled_seqs` 会阻止后面的 seq 继续塞进本轮 batch

## 推演结论

本节课通过一组手工推演，确认了以下结论：

- `waiting[0]` 代表队首优先，调度器不会乱序取请求
- 一轮 `schedule()` 里可以处理多个不同的 seq
- 同一个 `Sequence` 会在“调度 -> 执行 -> postprocess -> 下一轮调度”之间连续推进
- 预设预算不够时，prefill 会被截断并留到下一轮继续
- decode 阶段如果 KV cache 不够，会触发 preempt，把某些 running 请求打回 waiting

## 结束条件

单条请求结束是在 `Scheduler.postprocess()` 里判断的：

```python
if (not seq.ignore_eos and token_id == self.eos) or seq.num_completion_tokens == seq.max_tokens:
    seq.status = SequenceStatus.FINISHED
    self.block_manager.deallocate(seq)
    self.running.remove(seq)
```

这表示：

- 遇到 eos 且没有忽略 eos，就结束
- 或者生成的 completion token 数达到 `max_tokens`，也结束
- 结束后释放 KV cache，并从 `running` 中移除

## 验收结果

学习者已经能够说明：

- `waiting` / `running` 的职责
- `schedule()` 的返回值和作用
- prefill 优先的策略
- `max_num_batched_tokens` 和 `max_num_seqs` 的区别
- `num_cached_tokens + num_scheduled_tokens == num_tokens` 的含义
- `scheduled_seqs.append(seq)` 为什么是同一个对象引用
- decode 阶段为什么每次只调度 1 个 token
- 什么时候会触发 preempt

第 3 讲通过。

## 下一讲

第 4 讲进入 KV cache 和 `BlockManager`：

- 看 block 是怎么分配和复用的
- 看 prefix cache 命中是怎么判断的
- 看 `allocate()` 和 `deallocate()` 的配合
- 看 `num_cached_blocks` 和 `seq.num_blocks` 的关系
