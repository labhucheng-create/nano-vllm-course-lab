# 第 9 讲：prefill / decode 输入准备

## 目标

理解输入张量和上下文描述是怎么构造的。

## 读代码

- `nanovllm/engine/model_runner.py` 里的 `prepare_prefill()`
- `nanovllm/engine/model_runner.py` 里的 `prepare_decode()`
- `nanovllm/engine/model_runner.py` 里的 `prepare_sample()`

## 练习

- 推导 `slot_mapping`、`context_lens`、`block_tables`
- 解释 prefill 和 decode 的输入差异
- 标出 warmup 和 prefix cache 的特殊处理

## 验收

- 你能手推一次 tensor 形状和索引含义

## 是否足够

- 不够，必须手推一次
