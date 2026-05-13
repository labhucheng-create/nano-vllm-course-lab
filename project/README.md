# nano-vLLM 项目实战书

这份文档是这整个仓库的“施工图”。

它不是课程笔记的附录，而是把课程学习、项目实战、benchmark、简历交付串成一条线的主手册。  
未来如果你换电脑、换 Codex、或者隔了几周再回来，只要按这里的顺序读，就能继续往下做，不需要重新猜上下文。

## 这个项目要做成什么

我们最终要把两条能力线做成一个可以讲、可以跑、可以写进简历的作品集：

1. `nano-vLLM` 大模型推理框架优化
2. `LayerNorm / RMSNorm` CUDA 算子优化

这两个方向不是分开的。

- 算子优化是局部能力，证明你会写 kernel、会做 correctness、会做 microbenchmark
- 推理框架优化是系统能力，证明你理解调度、KV cache、prefill/decode、CUDA Graph 和端到端性能
- 两条线最终会在 `nano-vLLM` 里汇合成一个完整项目

## 你应该先读什么

按这个顺序读，最稳：

1. [README.md](../README.md)
2. [HANDOFF.md](../HANDOFF.md)
3. [project/roadmap.md](./roadmap.md)
4. [project/phases/README.md](./phases/README.md)
5. 对应阶段的 `project/phases/phase-XX-*.md`
6. 对应课程讲义 `course/lessons/lesson-XX-*.md`
7. `notes/progress.md`
8. `labs/bench.md`

如果你是未来接手的 Codex，先读 `project/README.md` 和 `project/roadmap.md`，再决定从哪个阶段继续。

## 当前状态

- 课程仓库已经搭好
- 课程主线已经推进到 `KV cache / BlockManager` 这一段
- 项目实战书现在刚建立，后续会按照这里的阶段推进
- 默认从 **Phase 0：环境与基线** 开始
- 你的个人学习检查点：课程第 4 讲（KV cache 与 `BlockManager`）已完成，下一步进入第 5 讲（模型结构与 attention）

## 工作方式

每个阶段都必须留下四类产物：

1. 图：把数据流或控制流画出来
2. 代码：把最小可运行路径做出来
3. 数据：把 benchmark 记录下来
4. 文字：把结论写成能被未来 Codex 直接接着读的文档

每个阶段都要满足这三个条件才算过关：

- 能讲清楚
- 能做出来
- 能留下证据

## 仓库分工

- `course/`：概念教学和讲义
- `project/`：项目实战书和执行路线
- `project/phases/`：每个阶段的详细施工步骤
- `project/templates/`：阶段日志、benchmark、总结模板
- `project/resume-copy.md`：可直接回填简历的项目素材
- `labs/`：benchmark、实验记录和优化对比
- `notes/`：课程笔记、阶段总结、学习进度
- `nanovllm/`：教材代码和后续实战的主要修改对象

## 项目输出目标

最终要产出这些东西：

- 一份能投递的 LLM Infra 方向简历
- 一条完整的 `nano-vLLM` 推理优化链路说明
- 一套 `LayerNorm / RMSNorm` CUDA 算子优化说明
- 一份 benchmark 表和对照实验记录
- 一套未来换电脑后仍然能接着做的项目手册
- 一份能直接贴进简历的项目素材库（`project/resume-copy.md`）

## 现在从哪里开始

如果你刚打开仓库，现在应该做的事是：

1. 先看 `project/roadmap.md`
2. 再看 `project/phases/phase-00-environment.md`
3. 然后回到课程第 0 讲或当前阶段继续推进

不要直接跳到优化阶段。
