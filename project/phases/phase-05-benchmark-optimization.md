# Phase 5：Benchmark 与优化闭环

## 目标

把前面的学习和实现变成可量化、可对比、可复现的工程结果。

这一阶段是项目能不能写进简历的关键。

## 前置条件

- Phase 4 已经通过
- 框架和算子至少有一条优化路径跑通
- `bench.py` 能作为 benchmark 入口

## 读哪些文档和代码

- `bench.py`
- `labs/bench.md`
- `project/roadmap.md`
- `nanovllm/engine/model_runner.py`
- `nanovllm/engine/scheduler.py`

## 要做什么

### 1. 定义 benchmark 维度

至少要固定这些维度：

- 模型
- 输入长度
- 输出长度
- batch size
- eager / graph
- baseline / optimized

### 2. 定义对照组

每次优化都要明确：

- baseline 是什么
- 修改点是什么
- 只改了哪一层
- 预期影响什么指标

### 3. 记录指标

建议重点记录这些内容：

- 总耗时
- prefill tok/s
- decode tok/s
- 首 token 延迟
- 峰值显存
- kernel 时间（如果适用）

### 4. 做优化闭环

至少跑出一个完整闭环：

1. baseline
2. 修改
3. correctness
4. benchmark
5. 结论

## 怎么验证

这一阶段的关键不是“快了多少”，而是：

- 结果是不是可复现
- 对照条件是不是一致
- 结论是不是能从数据里推出来

如果结果没有变好，也要记录原因。

## 本阶段产出

- 完整 benchmark 表
- before / after 对比说明
- 瓶颈分析
- 可回滚的优化记录

## 不要做什么

- 不要只写“优化了”三个字
- 不要把猜测写成结论
- 不要拿没测过的数据写进简历

## 下一步

Phase 5 结束后，进入 Phase 6。

