# Benchmark 模板

## 记录原则

- 同一组对比必须保持同模型、同提示词、同硬件、同环境
- baseline 和优化版必须成对记录
- 没有实测结果不要写结论
- 如果失败，也要记录失败原因

## 记录表

| 日期 | 阶段 | 场景 | 模型 | 输入长度 | 输出长度 | 批大小 | 模式 | 版本 | 总耗时 | prefill tok/s | decode tok/s | 峰值显存 | 备注 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

## 建议补充信息

- GPU 型号：
- 驱动版本：
- PyTorch 版本：
- CUDA 版本：
- 模型路径：
- Git commit：

## 常见对比项

- baseline vs 修改后
- eager vs CUDA Graph
- 不同 block size
- 不同 batch 组织方式
- 不同 prefill / decode 比例

