# 第 4 讲：KV cache 和块管理

## 目标

理解 block 是怎么分配、复用、回收的。

## 读代码

- `nanovllm/engine/block_manager.py`
- `nanovllm/engine/sequence.py`

## 练习

- 手算一次 block_table 的变化
- 解释 prefix cache 是怎么被命中的
- 说清 `hash_to_block_id` 和 `used_block_ids` 的关系

## 验收

- 你能解释为什么 block size 会影响吞吐和碎片

## 是否足够

- 不够，必须做推演
