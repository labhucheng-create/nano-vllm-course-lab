# 学习进度

## 当前状态

- 课程仓库已搭好
- GitHub SSH 推送已配置完成
- 第 1 讲：推理框架全景，已完成
- 第 2 讲：请求生命周期与 `Sequence`，已完成
- 第 3 讲：调度器 Scheduler，已完成
- 第 0 讲：环境搭建与验证，已加入课程地图
- 下一步：如需跑代码先补第 0 讲；否则进入第 4 讲，KV cache 与 BlockManager

## 已完成课程

| 课程 | 主题 | 总结 |
| --- | --- | --- |
| 第 1 讲 | 推理框架全景 | `notes/lesson-01-overview-summary.md` |
| 第 2 讲 | 请求生命周期与 Sequence | `notes/lesson-02-lifecycle-summary.md` |
| 第 3 讲 | 调度器 Scheduler | `notes/lesson-03-scheduler-summary.md` |

## 下一讲准备

第 4 讲要重点阅读：

- `nanovllm/engine/block_manager.py`
- `nanovllm/engine/sequence.py`
- `nanovllm/engine/scheduler.py` 的少量辅助逻辑

第 4 讲目标：

- 系统理解 KV cache block 的分配和复用
- 理解 prefix cache 命中
- 解释 `allocate()` / `deallocate()` / `can_allocate()` 的配合

## 记录模板

- 日期：
- 第几讲：
- 今天看了哪些代码：
- 今天真正搞懂了什么：
- 还有哪些没吃透：
- 下一步要补什么：

## 阶段总结模板

- 这一阶段的主题：
- 最有价值的一个理解：
- 最有价值的一个代码点：
- 还需要回头补的地方：
- 下一阶段目标：
