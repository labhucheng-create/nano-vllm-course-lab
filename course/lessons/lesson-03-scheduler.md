# 第 3 讲：调度器

## 目标

弄懂谁先跑、谁后跑、什么时候会被抢占。

## 读代码

- `nanovllm/engine/scheduler.py`

## 练习

- 画出 prefill / decode 两条路径
- 模拟 2 到 3 个序列的调度过程
- 找出触发 preemption 的条件

## 验收

- 你能把 `schedule()` 的核心逻辑讲成流程图

## 是否足够

- 不够，要自己推演队列变化
