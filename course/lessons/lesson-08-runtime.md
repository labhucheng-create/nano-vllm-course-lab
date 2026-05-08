# 第 8 讲：运行时与多进程

## 目标

理解 rank 0 和其他 rank 是如何协作的。

## 读代码

- `nanovllm/engine/model_runner.py`

## 练习

- 画出进程图
- 说清 shared memory 和 event 的作用
- 分清初始化、运行、退出三个阶段

## 验收

- 你能解释多卡模式下调用是怎么传过去的

## 是否足够

- 不够，必须画进程图
