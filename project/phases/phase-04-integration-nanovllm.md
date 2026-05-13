# Phase 4：接入 nano-vLLM

## 目标

把 Phase 3 的算子优化接进 `nano-vLLM` 推理链路，让它真正出现在 prefill / decode 路径里。

这一阶段的重点不是“再写一个算子”，而是：

- 明确它在 decoder layer 里的位置
- 保证原来的推理流程不被破坏
- 让优化从单算子变成系统收益

## 前置条件

- Phase 3 已经通过
- 算子的 correctness 已经确认
- 你知道 `RMSNorm` 在 `qwen3.py` 里的调用位置

## 读哪些文档和代码

- `nanovllm/models/qwen3.py`
- `nanovllm/layers/attention.py`
- `nanovllm/layers/layernorm.py`
- `nanovllm/engine/model_runner.py`
- `nanovllm/layers/linear.py`
- `course/lessons/lesson-07-loading-tp.md`
- `course/lessons/lesson-08-runtime.md`
- `course/lessons/lesson-09-inputs.md`
- `course/lessons/lesson-10-cudagraph.md`

## 要做什么

### 1. 画出算子在模型里的位置

你要明确：

- 哪些层会调用归一化
- 归一化和 Attention / MLP 的相对顺序
- prefill 和 decode 对它有没有不同路径

### 2. 明确集成边界

理想情况是：

- Python API 不变
- 模型结构不大改
- 只替换内部实现

这样最利于验证，也最利于后续讲面试故事。

### 3. 保留 fallback

优化版接入后，仍然保留 baseline 路径作为对照。

这样你可以做：

- baseline vs optimized
- eager vs optimized
- graph vs non-graph

### 4. 跑端到端验证

这里至少要验证：

- `example.py` 还能跑
- 输出没有明显偏差
- 单元测试或对照脚本没有炸

## 怎么验证

这一阶段过关的标志是：

- 算子已经挂进主链路
- 代码没有破坏原来的推理流程
- 你能指出优化发生在模型哪一层
- 你能说明它影响的是哪一段延迟或吞吐

## 本阶段产出

- 集成图
- 接口说明
- baseline / optimized 对照
- 端到端验证记录

## 不要深挖什么

这一阶段先不要把所有分布式细节都展开。

重点是“接进去并验证”，不是“把整个系统再重写一遍”。

## 下一步

Phase 4 结束后，进入 Phase 5。

