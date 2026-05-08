# nano-vLLM 课程工作仓库

这是一个面向学习和实战的 nano-vLLM 工作仓库。  
这里同时保留了：

- `nanovllm/`：教材代码
- `course/`：每一讲的课程讲义
- `labs/`：实验和 benchmark 记录
- `notes/`：学习笔记与阶段总结
- `HANDOFF.md`：给未来 Codex 的续学说明

## 你从哪里开始

1. 先看 [COURSE_MAP.md](./COURSE_MAP.md)
2. 再看 [HANDOFF.md](./HANDOFF.md)
3. 接着按 `course/lessons/` 的顺序读课
4. 每讲结束后，把结果记到 [notes/progress.md](./notes/progress.md)
5. 实战和优化结果写到 [labs/bench.md](./labs/bench.md)
6. 课程索引也可以直接看 [course/README.md](./course/README.md)

## 课程节奏

- 第 1 周：框架总览、请求生命周期、调度
- 第 2 周：KV cache、模型结构、权重加载、张量并行
- 第 3 周：运行时、多进程通信、prefill/decode、CUDA Graph
- 第 4 周：benchmark、瓶颈分析、实战优化、结题总结

## 基线代码

当前教材代码基于 nano-vLLM：

- 入口：`nanovllm/llm.py`
- 调度：`nanovllm/engine/scheduler.py`
- 运行时：`nanovllm/engine/model_runner.py`
- KV cache：`nanovllm/engine/block_manager.py`
- 模型：`nanovllm/models/qwen3.py`

## 原始项目

原始上游仓库见 [UPSTREAM.md](./UPSTREAM.md)。

## 本地跑通

```bash
pip install -e .
python example.py
```

## 说明

这个仓库以后会持续承载你的课程材料、代码修改和优化记录。  
换电脑时，只要 clone 这里，未来 Codex 就能顺着这些文档继续带你学。
