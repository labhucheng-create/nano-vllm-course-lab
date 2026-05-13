# Phase 2：请求状态与 KV cache

## 目标

把请求状态、调度器、KV cache、block table 和 prefix cache 这几个概念串成一条线。

这一阶段结束后，你应该能手工推演一条请求在系统里的完整变化。

## 前置条件

- Phase 1 已经通过
- 你能讲清 `generate -> step -> schedule -> run -> postprocess`
- 你知道 prefill 和 decode 的区别

## 读哪些文档和代码

- `nanovllm/engine/sequence.py`
- `nanovllm/engine/scheduler.py`
- `nanovllm/engine/block_manager.py`
- `nanovllm/engine/model_runner.py` 中的 `allocate_kv_cache()`
- `course/lessons/lesson-03-scheduler.md`
- `course/lessons/lesson-04-kvcache.md`

## 要做什么

### 1. 手推 `Sequence`

你要知道这些字段分别在干什么：

- `status`
- `token_ids`
- `num_tokens`
- `num_prompt_tokens`
- `num_cached_tokens`
- `num_scheduled_tokens`
- `block_table`
- `is_prefill`

### 2. 手推 `Scheduler`

至少模拟一次：

- 1 条长 prompt
- 1 条短 prompt
- 1 条 decode 中的请求

你要能看懂：

- waiting 队列怎么变
- running 队列怎么变
- prefill 什么时候会被 chunk
- preemption 什么时候会发生

### 3. 手推 `BlockManager`

你要能说清：

- `free_block_ids` 和 `used_block_ids`
- `ref_count`
- `hash_to_block_id`
- `allocate / deallocate / hash_blocks`

### 4. 手推 prefix cache

你要能解释：

- 为什么相同前缀能复用
- 复用的是逻辑 block 还是物理 block
- `slot_mapping` 和 `block_table` 的区别

### 5. 画一张“请求状态 + 缓存”联动图

这张图要把下面几件事同时放进去：

- 请求状态流转
- block 分配和回收
- prefix cache 命中
- prefill chunk
- decode 单步生成

## 怎么验证

这一阶段的验收必须包含“你自己推过一遍”：

- 能写出一条请求在不同轮次的状态变化
- 能写出 `block_table` 的变化
- 能写出 `ref_count` 的变化
- 能解释 `slot_mapping` 为什么是 token 到物理槽位的映射

## 本阶段产出

- 手推表格
- 缓存联动图
- 阶段总结
- 如果方便，补一份小样例说明文档

## 不要深挖什么

这一阶段不要提前展开：

- FlashAttention 参数细节
- kernel 的逐行实现
- CUDA Graph 具体 capture 过程

这些属于后面的阶段。

## 下一步

Phase 2 结束后，进入 Phase 3。

