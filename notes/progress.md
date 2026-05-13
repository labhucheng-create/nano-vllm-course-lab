# 学习进度

## 当前状态
- 课程仓库已搭好，GitHub SSH 推送已完成
- 第 1 讲：推理框架全景，已完成
- 第 2 讲：请求生命周期与 `Sequence`，已完成
- 第 3 讲：调度器 `Scheduler`，已完成
- 第 4 讲：KV cache 与 `BlockManager`，已完成
- 当前学习检查点：已经学到第 4 讲
- 下一步：进入第 5 讲，模型结构与 attention
- 如果换电脑继续学，先做第 0 讲的环境验证，再从第 5 讲接着往下走

## 项目实战进度
- 项目实战书已建立，入口见 `project/README.md`
- 项目路线图见 `project/roadmap.md`
- 当前默认从 Phase 0：环境与基线 开始
- 课程和项目并行推进，但项目手册是主线
- 你的个人课程进度镜像：第 4 讲已完成，下一步第 5 讲
- 后续 benchmark 统一记录到 `labs/bench.md`

## 已完成课程
| 课程 | 主题 | 总结 |
| --- | --- | --- |
| 第 1 讲 | 推理框架全景 | `notes/lesson-01-overview-summary.md` |
| 第 2 讲 | 请求生命周期与 Sequence | `notes/lesson-02-lifecycle-summary.md` |
| 第 3 讲 | 调度器 Scheduler | `notes/lesson-03-scheduler-summary.md` |
| 第 4 讲 | KV cache 与 BlockManager | `notes/lesson-04-kvcache-summary.md` |

## 下一讲准备
第 5 讲建议重点阅读：

- `nanovllm/models/qwen3.py`
- `nanovllm/layers/attention.py`
- `nanovllm/layers/rotary_embedding.py`
- 如果先做项目实战，则先读 `project/phases/phase-00-environment.md`

第 5 讲目标：

- 搞清楚 decoder layer 的前向流程
- 看懂 RoPE、RMSNorm、Attention、MLP 各自的位置
- 继续把模型层和缓存层的关系串起来

## 以后每讲的总结模板
- 日期
- 第几讲
- 今天看了哪些代码
- 今天真正搞懂了什么
- 还有哪些没吃透
- 下一步要补什么

## 以后每阶段总结模板
- 这一阶段的主题
- 最有价值的一个理解
- 最有价值的一个代码点
- 还需要回头补的地方
- 下一阶段目标

## 项目阶段总结模板
- 当前 Phase
- 这一阶段做了什么
- 产出了什么图 / 代码 / 数据
- 哪些结论来自 benchmark
- 下一阶段入口
