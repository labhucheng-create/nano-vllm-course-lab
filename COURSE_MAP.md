# 课程地图

这份地图面向 4 周学习节奏。每一讲都按同一种结构组织：

- 先读哪些代码
- 这讲要画什么图
- 这讲要做什么练习
- 读完是否足够

## Week 1

### 第 0 讲：环境搭建与验证
- 读：`pyproject.toml`、`README.md`、`example.py`、`bench.py`
- 查：Python、CUDA、PyTorch、模型路径、GitHub SSH
- 做：跑通最小 import、最小生成、Git push
- 结论：必须动手验证；新电脑或进入实战前都要补这节

### 第 1 讲：推理框架全景
- 读：`README.md`、`example.py`、`nanovllm/llm.py`
- 画：一次 `LLM.generate()` 的完整调用链
- 做：口头讲清“入口 -> 引擎 -> 调度 -> 运行时 -> 输出”
- 结论：读代码基本够，重点是建立总图

### 第 2 讲：请求生命周期
- 读：`nanovllm/engine/llm_engine.py`、`nanovllm/engine/sequence.py`
- 画：请求从 WAITING 到 RUNNING 到 FINISHED 的状态变化
- 做：手工跟踪一个 prompt 的 prefill / decode 切换
- 结论：不够，必须配合一个小例子跑一遍

### 第 3 讲：调度器
- 读：`nanovllm/engine/scheduler.py`
- 画：prefill / decode 分支和 preemption 条件
- 做：模拟 2 到 3 个序列的调度结果
- 结论：不够，要自己推演队列变化

## Week 2

### 第 4 讲：KV cache 和块管理
- 读：`nanovllm/engine/block_manager.py`、`nanovllm/engine/sequence.py`
- 画：block_table、hash cache、free / used block 的关系
- 做：手算一次 block 分配、复用、回收
- 结论：不够，必须做推演

### 第 5 讲：模型结构
- 读：`nanovllm/models/qwen3.py`、`nanovllm/layers/attention.py`、`nanovllm/layers/rotary_embedding.py`
- 画：decoder layer 的数据流
- 做：解释张量 shape 在每一步怎么变
- 结论：读代码加画图基本够

### 第 6 讲：基础算子
- 读：`nanovllm/layers/linear.py`、`nanovllm/layers/layernorm.py`、`nanovllm/layers/activation.py`
- 画：普通实现和 fused / parallel 实现对比
- 做：标出并行维度、切分点、通信点
- 结论：不够，必须拆 shape 和数据路径

## Week 3

### 第 7 讲：模型加载与张量并行
- 读：`nanovllm/utils/loader.py`、`nanovllm/config.py`、`nanovllm/layers/embed_head.py`
- 画：safetensors 到 shard 权重的映射
- 做：追一条权重名如何落到参数上
- 结论：不够，要追一次真实映射

### 第 8 讲：运行时与多进程
- 读：`nanovllm/engine/model_runner.py`
- 画：rank 0、其他 rank、shared memory、event 的协作图
- 做：说清初始化、运行、退出三个阶段
- 结论：不够，必须画进程图

### 第 9 讲：prefill / decode 输入准备
- 读：`nanovllm/engine/model_runner.py` 里的 `prepare_prefill()`、`prepare_decode()`、`prepare_sample()`
- 画：`slot_mapping`、`context_lens`、`block_tables` 的来源
- 做：推导一次 tensor 形状和索引含义
- 结论：不够，必须手推一次

## Week 4

### 第 10 讲：CUDA Graph
- 读：`nanovllm/engine/model_runner.py` 里的 `capture_cudagraph()`、`run_model()`
- 画：eager path 和 graph path 的切换条件
- 做：解释为什么 decode 更适合 graph
- 结论：读代码不够，最好配 benchmark

### 第 11 讲：性能分析
- 读：`bench.py`
- 画：prefill throughput 和 decode throughput 的关系
- 做：记录一次性能结果，解释瓶颈
- 结论：不够，必须跑实验

### 第 12 讲：实战优化
- 读：前 11 讲暴露出来的相关代码
- 画：优化前后的路径差异
- 做：实现一个可测优化并记录结果
- 结论：必须动手，且要保留对比
