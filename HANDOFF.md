# 给未来 Codex 的续学说明

如果你是未来接手这个仓库的 Codex，请按这个顺序进入：

1. 读 `README.md`
2. 读 `COURSE_MAP.md`
3. 看 `notes/progress.md`
4. 看 `labs/bench.md`
5. 再进入对应的 `course/lessons/lesson-XX.md`
6. 只读这讲相关的 `nanovllm/` 代码，不要一上来全仓乱翻

## 当前仓库目标

- 这是一个 nano-vLLM 教材 + 个人优化仓库
- 目标不是只跑 demo，而是把推理框架讲明白、改明白、测明白
- 任何代码修改都要能回到课程主题里

## 你应该如何带学

- 每讲先建立图，再读代码
- 先理解 prefill / decode / KV cache / 调度，再碰优化
- 每次实战都要留：
  - 修改点
  - 验证方法
  - 指标变化
  - 回滚方案

## 你应该优先检查的代码

- `nanovllm/engine/llm_engine.py`
- `nanovllm/engine/scheduler.py`
- `nanovllm/engine/block_manager.py`
- `nanovllm/engine/model_runner.py`
- `nanovllm/models/qwen3.py`

## 续学原则

- 不要默认学员什么都不会，也不要默认学员已经会推理框架
- 根据课程地图推进，不要跳着讲
- 如果学员换电脑，先恢复仓库上下文，再继续下一讲
