# Benchmark 记录

## 记录模板

| 日期 | 阶段 | 场景 | 模型 | 输入长度 | 输出长度 | 批大小 | eager/graph | 版本 | 总耗时 | prefill tok/s | decode tok/s | 峰值显存 | 备注 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

## 复现实验要求

- 同一场景尽量保持相同模型和提示词
- 优化前后必须记录同一组指标
- 每次改动后都说明是吞吐变化、延迟变化，还是显存变化
- 每一条记录都要能回到项目阶段和 Git commit
- 如果结果不稳定，也要记录失败原因和复测条件
- 不要把“推测”写成“结论”

## 常见对比项

- baseline vs 修改后
- eager vs CUDA Graph
- 不同 block size
- 不同 batch 组织方式
- 不同 prefill / decode 比例

## 与项目实战的关系

- Phase 0：写第一条 baseline
- Phase 3：写算子 microbenchmark
- Phase 5：写 before / after 对照表
- Phase 6：把可证明的数据整理成简历素材
