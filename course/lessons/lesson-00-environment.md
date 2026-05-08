# 第 0 讲：环境搭建与验证

## 目标

把能支撑后续课程和实战优化的本地环境准备好。  
这节课不讲推理框架原理，重点是确认代码、模型、CUDA、Python 依赖和 GitHub 同步都能工作。

## 什么时候上

- 新电脑第一次 clone 仓库后
- 第一次需要跑 `example.py` 或 `bench.py` 前
- 进入性能分析和实战优化前
- 环境报错导致课程无法继续时

## 要检查的内容

- Python 版本满足 `pyproject.toml`
- CUDA / GPU 可用
- PyTorch 能识别 GPU
- `pip install -e .` 能完成
- Qwen3-0.6B 模型路径存在
- `example.py` 能跑通最小请求
- `bench.py` 能作为后续 benchmark 入口
- Git remote 指向个人 GitHub 仓库，且能 push

## 建议命令

```bash
python --version
pip install -e .
python -c "import torch; print(torch.__version__); print(torch.cuda.is_available())"
python example.py
```

## 验收

- 能 import `nanovllm`
- 能初始化 `LLM`
- 能跑一次最小生成
- 能把课程笔记 commit 并 push 到 GitHub

## 是否足够

这节课必须动手验证。  
只读文档不算通过。
