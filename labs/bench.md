# Benchmark 记录

## 记录模板

| 日期 | 场景 | 模型 | 输入长度 | 输出长度 | 批大小 | eager/graph | 总耗时 | prefill tok/s | decode tok/s | 备注 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

## 复现实验要求

- 同一场景尽量保持相同模型和提示词
- 优化前后必须记录同一组指标
- 每次改动后都说明是吞吐变化、延迟变化，还是显存变化

## 常见对比项

- baseline vs 修改后
- eager vs CUDA Graph
- 不同 block size
- 不同 batch 组织方式
- 不同 prefill / decode 比例
