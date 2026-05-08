# 第 6 讲：基础算子

## 目标

看懂融合算子和并行线性层怎么组织。

## 读代码

- `nanovllm/layers/linear.py`
- `nanovllm/layers/layernorm.py`
- `nanovllm/layers/activation.py`

## 练习

- 对比普通实现和当前实现
- 标出并行维度、切分点、通信点
- 解释为什么这里要做融合

## 验收

- 你能说清每个层是怎么兼顾语义和性能的

## 是否足够

- 不够，必须拆 shape 和数据路径
