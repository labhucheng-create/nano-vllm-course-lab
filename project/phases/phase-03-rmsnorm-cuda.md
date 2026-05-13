# Phase 3：RMSNorm / LayerNorm CUDA 算子

## 目标

把归一化算子做成一个真正的 CUDA 优化项目，而不是停留在“知道原理”。

这一阶段的核心，是让你能同时讲清楚这三件事：

1. 算子数学是什么
2. baseline 怎么写
3. 优化版为什么更快

## 前置条件

- Phase 2 已经通过
- 你知道归一化算子在 decoder layer 里的位置
- 你能接受先做 baseline，再做 kernel，再做对比

## 读哪些文档和代码

- `nanovllm/layers/layernorm.py`
- `nanovllm/models/qwen3.py`
- `nanovllm/layers/activation.py`
- `course/lessons/lesson-05-model.md`
- `course/lessons/lesson-06-kernels.md`

## 这一阶段要做什么

### 1. 先明确项目边界

这条线里，主目标是 `RMSNorm`，`LayerNorm` 作为同一套归一化骨架的对照和扩展。

你在简历里可以统一写成：

`LayerNorm / RMSNorm CUDA 算子优化`

但在实现时要分清：

- RMSNorm 关注的是平方均值归一化
- LayerNorm 还涉及均值和方差

### 2. 先做 PyTorch baseline

先把基线写清楚，再碰 CUDA。

你要准备：

- 一个 reference 实现
- 一个随机输入 correctness 测试
- 一个对照脚本

### 2.1 建议保持的接口边界

如果后续开始落代码，建议保持现有 `RMSNorm` 的调用方式不变：

- 外部调用者仍然只看到同样的 Python 接口
- 优化实现放在单独的 kernel / op 模块里
- baseline 和 optimized 要能通过一个开关切换

这样最利于做对照实验，也最利于后面接回 `nano-vLLM`。

### 3. 再做 kernel 设计

优化重点应围绕这些方向展开：

- 并行规约
- 访存合并
- shared memory 暂存
- warp 级优化
- 向量化加载
- 减少不必要的中间张量

### 4. 做 correctness 对比

只要是优化算子，就必须先证明“对”。

至少要记录：

- 输入形状
- dtype
- baseline 输出
- 优化版输出
- 误差情况

### 5. 做 microbenchmark

至少要看这些维度：

- hidden size
- batch size
- dtype
- eager / compile / kernel 版本

## 怎么验证

这一阶段过关的标准不是“写了一个 kernel”，而是：

- baseline 和优化版都能跑
- 正确性已经验证
- microbenchmark 已经记录
- 你能解释为什么这里更快

## 本阶段产出

- baseline 实现
- 优化版实现
- correctness 记录
- microbenchmark 表
- 可写进简历的项目描述

## 不要深挖什么

这一阶段不要急着把 kernel 和整个推理框架绑死。

先把单算子做实，再进入下一阶段集成。

## 下一步

Phase 3 结束后，进入 Phase 4。
