# Phase 1：框架主链路

## 目标

把 `nano-vLLM` 的主调用链变成你脑子里的一张图。

这一阶段你不需要理解所有底层 kernel，但必须能够把下面这条链路讲清楚：

`generate -> add_request -> step -> schedule -> run -> postprocess`

## 前置条件

- Phase 0 已经通过
- `example.py` 能跑
- 你已经知道模型从哪里来、环境怎么起

## 读哪些文档和代码

- `README.md`
- `example.py`
- `nanovllm/llm.py`
- `nanovllm/engine/llm_engine.py`
- `course/lessons/lesson-01-overview.md`
- `course/lessons/lesson-02-lifecycle.md`

## 要做什么

### 1. 画总图

先画一张图，把以下对象放进去：

- 用户 prompt
- tokenizer
- `Sequence`
- `Scheduler`
- `ModelRunner`
- `Sampler`
- 输出文本

### 2. 讲清 `step()` 的职责

你必须能说清：

- 为什么 `step()` 一次只推进一轮
- 为什么有时候是 prefill，有时候是 decode
- 为什么 `token_ids` 不是最终文本，而是中间结果

### 3. 讲清 `generate()` 和 `step()` 的关系

你要能回答：

- `generate()` 为什么是总循环
- `step()` 为什么是单步推进
- 为什么进度条跟“请求完成数”有关，而不是跟 token 总数直接绑定

### 4. 把流程说成口头故事

把“一条 prompt 如何进入引擎并产生输出”讲成 3 分钟内能说完的口头版本。

## 怎么验证

这一阶段的验收不是写代码，而是你能不能稳定回答这几个问题：

- 一条 prompt 是怎么进来的
- 一轮调度里发生了什么
- 输出是怎么从 token 变成文本的
- 哪些步骤属于“调度”，哪些属于“运行”

## 本阶段产出

- 一张完整调用链图
- 一份口头讲解稿
- 一份阶段总结

## 不要深挖什么

这一阶段不要去死抠：

- `ModelRunner.__init__()` 每一行
- shared memory 传输细节
- CUDA Graph 细节
- FlashAttention 底层实现
- 张量并行权重切分细节

这些留到后面的阶段。

## 下一步

Phase 1 结束后，进入 Phase 2。

