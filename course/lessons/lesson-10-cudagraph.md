# 第 10 讲：CUDA Graph

## 目标

理解为什么 decode 路径会切到 graph。

## 读代码

- `nanovllm/engine/model_runner.py` 里的 `capture_cudagraph()`
- `nanovllm/engine/model_runner.py` 里的 `run_model()`

## 练习

- 画出 eager path 和 graph path 的切换条件
- 解释为什么 prefill 一般不走 graph
- 说清 graph capture 的限制

## 验收

- 你能解释这个优化解决的是什么问题

## 是否足够

- 读代码不够，最好配 benchmark
