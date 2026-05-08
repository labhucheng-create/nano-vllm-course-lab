# 第 7 讲：模型加载与张量并行

## 目标

看懂权重怎么从 safetensors 落到 TP shard 上。

## 读代码

- `nanovllm/utils/loader.py`
- `nanovllm/config.py`
- `nanovllm/layers/embed_head.py`

## 练习

- 追一条权重名映射到参数的过程
- 理解 embedding 和 lm head 的切分方式
- 说清 `packed_modules_mapping` 的作用

## 验收

- 你能解释一份 HF 权重是怎么被拆开加载的

## 是否足够

- 不够，要追一次真实映射
