# 学习进度

## 当前状态

- 课程仓库已搭好
- GitHub SSH 推送已配置完成
- 第 1 讲：推理框架全景，已完成
- 第 0 讲：环境搭建与验证，已加入课程地图
- 下一步：如需跑代码先补第 0 讲；否则进入第 2 讲，请求生命周期与 `Sequence`

## 已完成课程

| 课程 | 主题 | 总结 |
| --- | --- | --- |
| 第 1 讲 | 推理框架全景 | `notes/lesson-01-overview-summary.md` |

## 下一讲准备

第 2 讲要重点阅读：

- `nanovllm/engine/llm_engine.py`
- `nanovllm/engine/sequence.py`
- `nanovllm/engine/scheduler.py` 的状态流部分

第 2 讲目标：

- 理解一个请求从 WAITING 到 RUNNING 到 FINISHED 的过程
- 理解 prompt token 和 generated token 如何一起存在 `Sequence.token_ids` 中
- 手工跟踪一次请求生命周期

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
