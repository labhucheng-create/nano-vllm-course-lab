# 项目路线图

这份路线图把课程内容和项目实战合成一条线。  
课程负责“理解”，项目负责“落地”。  
每个阶段都应该先完成最小闭环，再进入下一阶段。

## 总原则

- 先 baseline，再优化
- 先单点，再集成
- 先验证，再写结论
- 没有 benchmark，不写性能结论
- 没有手推验收，不进入下一阶段

## 阶段总览

| 阶段 | 核心主题 | 对应课程 | 必做产物 | 退出条件 | 简历价值 |
| --- | --- | --- | --- | --- | --- |
| Phase 0 | 环境与基线 | 第 0 讲 | 环境记录、`example.py`、`bench.py` 基线 | Linux/AutoDL、模型、依赖、推理、benchmark 全部跑通 | 证明你能把系统搭起来 |
| Phase 1 | 框架主链路 | 第 1-2 讲 | 调用链图、请求生命周期图、关键路径注释 | 能讲清 `generate -> step -> schedule -> run -> postprocess` | 证明你懂推理框架入口 |
| Phase 2 | 请求状态与 KV cache | 第 2-4 讲 | `Sequence` 状态推演、`BlockManager` 手推、prefix cache 图 | 能手推一条请求的 prefill / decode / 回收 | 证明你懂缓存与调度联动 |
| Phase 3 | RMSNorm / LayerNorm CUDA | 第 5-6 讲 | baseline、kernel、correctness、microbenchmark | 算子能跑、能对、能测 | 证明你会写 CUDA 优化 |
| Phase 4 | 接入 nano-vLLM | 第 7-10 讲 | 集成说明、API 边界、end-to-end 验证 | 算子能挂进推理链路且不破坏主流程 | 证明你能把局部优化放进系统 |
| Phase 5 | Benchmark 与优化闭环 | 第 11-12 讲 | benchmark 表、before/after 对比、瓶颈分析 | 能给出可重复实验和明确结论 | 证明你能做工程优化闭环 |
| Phase 6 | 简历与面试收口 | 全部阶段之后 | `resume-copy.md`、项目讲法、Q&A | 项目能被你口头讲清楚并写进简历 | 证明你能把成果变成求职材料 |

## 阶段衔接规则

### Phase 0 -> Phase 1
必须确认：

- Python、CUDA、PyTorch 都可用
- `pip install -e .` 成功
- `example.py` 能跑通
- `bench.py` 有可记录的 baseline
- 模型路径已经固定

### Phase 1 -> Phase 2
必须确认：

- 你能画出请求生命周期图
- 你能说明 `Sequence` 的状态字段
- 你知道 `step()` 里为什么有 prefill 和 decode 两条分支

### Phase 2 -> Phase 3
必须确认：

- 你能手推 block 分配、复用、回收
- 你知道 `block_table` 和 `slot_mapping` 的区别
- 你知道 prefix cache 的命中条件

### Phase 3 -> Phase 4
必须确认：

- CUDA baseline 和优化版都能跑
- 正确性对比已经完成
- microbenchmark 已经记录
- 算子接口是稳定的

### Phase 4 -> Phase 5
必须确认：

- 算子已经接回 `nano-vLLM`
- `example.py` 仍能跑
- 至少一次端到端 benchmark 已完成

### Phase 5 -> Phase 6
必须确认：

- 有至少一组可复现的 benchmark 结果
- 你能解释为什么这个优化有效或无效
- 项目结果已经整理成简历语言

## 项目最终成品

项目做完后，应该能回答这三个问题：

1. 你到底优化了什么
2. 你怎么证明它有效
3. 你为什么有资格把它写进简历

