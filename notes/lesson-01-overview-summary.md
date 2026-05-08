# 第 1 讲总结：推理框架全景

## 本节目标

第 1 讲的目标不是逐行看懂所有实现，而是建立 nano-vLLM 的整体调用链：

```text
example.py -> LLM -> LLMEngine -> Scheduler -> ModelRunner -> outputs
```

本节课只要求知道一次生成请求大致怎么流动，不要求深挖多进程、CUDA Graph、KV cache 细节。

## 读过的代码

- `example.py`
- `nanovllm/__init__.py`
- `nanovllm/llm.py`
- `nanovllm/engine/llm_engine.py`
- `nanovllm/engine/sequence.py`
- `nanovllm/engine/scheduler.py` 的局部逻辑

## 主链路

一次请求的大致流程：

1. 用户 prompt 进入 `LLM.generate()`
2. `generate()` 调用 `add_request()`，把 prompt 转成 `Sequence`
3. `Sequence` 被放入 `Scheduler` 的 waiting 队列
4. `generate()` 循环调用 `step()`
5. `step()` 调用 `scheduler.schedule()` 选择本轮要跑的序列
6. `ModelRunner.run()` 真正执行模型推理
7. `scheduler.postprocess()` 把新 token 写回 `Sequence`
8. 完成的序列被收集到 `outputs`
9. 最后按 `seq_id` 排序并 decode 成文本

## 关键概念

- `LLM` 基本只是壳，真正的主入口是 `LLMEngine`
- `Scheduler` 负责决定“本轮谁跑、跑 prefill 还是 decode”
- `ModelRunner` 负责真正跑模型
- `Sequence` 表示一条用户请求的完整生命周期
- `seq_id` 是序列编号，不是 token 编号
- `token_ids` 是文本经过 tokenizer 后得到的整数序列
- `completion_token_ids` 是模型生成出来的 token id 列表，不包含 prompt

## 重要纠偏

- `Sequence` 不是随机切出来的小段，而是一条用户请求
- prompt 可能会因为 chunked prefill 分多次处理，但仍属于同一个 `Sequence`
- prefill 不是“一次性生成多个回答 token”，而是处理 prompt 并建立 KV cache
- decode 才是模型一步一步生成回答 token 的阶段
- `outputs[seq_id] = token_ids` 不会每一步覆盖中间结果，因为只有 `seq.is_finished` 时才会写入最终结果

## 本节边界

本节课没有深挖：

- `ModelRunner.__init__()` 每一行
- 多进程 shared memory 通信
- CUDA Graph
- KV cache block 管理细节
- tensor parallel 权重切分

这些内容会在后续课程逐步拆开。

## 验收结果

学习者已经能够说明：

- 用户请求如何进入 `generate()`
- `Scheduler` 负责调度
- `ModelRunner` 负责跑模型
- `Sequence` 保存请求状态和 token 轨迹
- prefill 和 decode 的基本区别

第 1 讲通过。

## 下一讲

第 2 讲进入请求生命周期：

- 深入看 `Sequence`
- 理解 WAITING、RUNNING、FINISHED
- 手工跟踪一个 prompt 从进入系统到结束的状态变化
