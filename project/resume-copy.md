# 简历项目素材

这份文件存的是“可以直接贴进简历”的项目素材。  
后续如果 benchmark 数字更新，这里也要同步更新。

## 项目 1：nano-vLLM 大模型推理框架优化

**项目内容：**  
基于轻量级 nano-vLLM 推理框架，围绕大模型离线推理中的吞吐、显存利用率和解码延迟进行端到端优化。项目覆盖请求调度、KV cache 块管理、prefix cache、chunked prefill、decode 阶段 CUDA Graph、Attention 输入组织和 benchmark 评测体系，目标是构建一个可理解、可修改、可验证的小型 vLLM 推理引擎。

**技术栈：**  
Python、PyTorch、CUDA、Triton、FlashAttention、Transformers、Qwen3、vLLM 思路拆解

**简历 bullet 版：**

- 梳理并重构 `LLMEngine -> Scheduler -> ModelRunner -> Attention -> Sampler` 的完整执行链路，明确 prompt 输入、prefill、decode 到输出回收的请求生命周期。
- 优化请求调度策略，分析 `waiting/running` 队列切换、`max_num_batched_tokens`、`max_num_seqs` 对吞吐和延迟的影响，针对长短请求混合场景设计 chunked prefill 思路。
- 深入优化 KV cache 管理机制，基于 block table 管理物理 cache block，支持 block 分配、复用、回收和引用计数维护，降低长上下文和多请求并发场景下的显存碎片。
- 实现 prefix cache 复用机制，通过 token block hash 建立 `hash_to_block_id` 索引，在多请求共享相同前缀时复用已有 KV cache，减少重复 prefill 计算。
- 拆解并优化 prefill / decode 输入准备流程，完成 `input_ids`、`positions`、`slot_mapping`、`context_lens`、`block_tables` 等关键张量组织，保证 token 能准确映射到物理 KV cache 槽位。
- 优化 decode 阶段执行路径，分析 eager 模式与 CUDA Graph 模式的切换条件，减少小 batch 场景下的 Python 调度和 kernel launch 开销。

**面试讲法：**

1. 先讲主链路图。
2. 再讲为什么要分 prefill 和 decode。
3. 再讲 KV cache 为什么必须做 block 管理。
4. 最后讲 benchmark 结果和下一步还能怎么优化。

## 项目 2：LayerNorm / RMSNorm CUDA 算子优化

**项目内容：**  
围绕深度学习模型中的归一化算子开展 CUDA 优化实践，以 PyTorch 实现作为 baseline，针对 LayerNorm / RMSNorm 的计算流程进行并行化设计和性能优化，并结合推理框架中的 decoder layer 场景进行集成验证。

**技术栈：**  
CUDA、SIMT、PyTorch、Triton、C/C++、性能分析、microbenchmark

**简历 bullet 版：**

- 分析 LayerNorm / RMSNorm 的计算流程，拆解均值、方差或平方均值统计、归一化、缩放与偏置等步骤，明确优化切入点。
- 基于 SIMT 编程模型设计 CUDA kernel，将 hidden dimension 内的归约计算映射到 block / warp 并行执行。
- 围绕访存合并、线程块划分、shared memory 暂存、warp 级规约、向量化加载等方向进行优化设计，减少访存和同步开销。
- 构建 PyTorch baseline 和 correctness 对照脚本，验证 CUDA kernel 的输出误差，并设计不同 hidden size、batch size 和 dtype 下的 microbenchmark。
- 结合大模型推理中的 RMSNorm 使用场景，分析归一化算子在 decoder layer 中的位置，为后续接入推理框架和进一步降低 decode 延迟做准备。

**面试讲法：**

1. 先讲算子数学定义。
2. 再讲 baseline 和优化版的差异。
3. 再讲你是怎么证明“对”的。
4. 最后讲它为什么会影响推理端到端性能。

## 可以补的数字

后面 benchmark 跑出来后，把这些位置补上：

- decode tok/s：
- prefill tok/s：
- 首 token 延迟：
- 峰值显存：
- kernel 耗时：

## 不要写进简历的内容

- 没有测过的性能提升数字
- 你自己还讲不顺的技术点
- 只有学习笔记、没有工程结果的描述

