# 第 5 讲：模型结构

## 目标

理解 Qwen3 decoder layer 的前向数据流。

## 读代码

- `nanovllm/models/qwen3.py`
- `nanovllm/layers/attention.py`
- `nanovllm/layers/rotary_embedding.py`

## 练习

- 画出 `RMSNorm -> Attention -> MLP` 的路径
- 标出张量 shape 的变化
- 说清 RoPE 在哪里介入

## 验收

- 你能顺着一个 token 讲完整个 decoder layer

## 是否足够

- 读代码加画图基本够
