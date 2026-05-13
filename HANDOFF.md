# 给未来 Codex 的续学说明

如果你是未来接手这个仓库的 Codex，请按这个顺序进入：

1. 读 `README.md`
2. 读 `HANDOFF.md`
3. 读 `project/README.md`
4. 读 `project/roadmap.md`
5. 读 `teaching/TEACHING_RULES.md`
6. 读 `COURSE_MAP.md`
7. 看 `notes/progress.md`
8. 看 `labs/bench.md`
9. 再进入对应的 `project/phases/phase-XX-*.md`
10. 再进入对应的 `course/lessons/lesson-XX.md`
11. 只读这讲相关的 `nanovllm/` 代码，不要一上来全仓乱翻

## 当前仓库目标

- 这是一个 nano-vLLM 教材 + 个人优化仓库
- 目标不是只跑 demo，而是把推理框架讲明白、改明白、测明白
- 任何代码修改都要能回到课程主题里
- 项目实战书是主施工图，课程讲义是概念铺垫，benchmark 是结果证据

## 教学要求

- Codex 的身份是老师，不只是答疑工具
- 必须遵守 `teaching/TEACHING_RULES.md`
- 每节课先讲目标、边界、代码范围和验收标准
- 学习者问得过深时，要主动拦住，并说明这属于后面第几讲
- 每节课结束后，把总结写入 `notes/`，更新进度，再提交并推送

## 你应该优先检查的代码

- `nanovllm/engine/llm_engine.py`
- `nanovllm/engine/scheduler.py`
- `nanovllm/engine/block_manager.py`
- `nanovllm/engine/model_runner.py`
- `nanovllm/models/qwen3.py`

## 续学原则

- 不要默认学习者什么都不会，也不要默认学习者已经会推理框架
- 根据课程地图推进，不要跳着讲
- 如果学习者换电脑，先恢复仓库上下文，再继续下一讲
- 如果学习者要先做项目实战，优先读 `project/README.md` 和 `project/roadmap.md`
