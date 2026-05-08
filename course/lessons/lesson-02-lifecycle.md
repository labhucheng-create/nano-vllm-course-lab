# 第 2 讲：请求生命周期

## 目标

理解一个请求从进入到结束，状态是怎么变化的。

## 读代码

- `nanovllm/engine/llm_engine.py`
- `nanovllm/engine/sequence.py`

## 练习

- 手工跟踪一个 prompt 的状态变化
- 区分 `num_tokens`、`num_prompt_tokens`、`num_completion_tokens`
- 说清 prefill 和 decode 的差别

## 验收

- 你能不看代码，说明一个 sequence 的生命周期

## 是否足够

- 不够，必须结合一个例子跑一遍
