# Phase 0：环境与基线

## 目标

把后续课程和实战能依赖的最小环境准备好。

这一阶段的重点不是理解推理框架原理，而是先确认：

- Linux / AutoDL 能连
- GPU 能识别
- PyTorch 能用 CUDA
- 依赖能装
- 模型能找到
- `example.py` 能跑
- `bench.py` 能记录 baseline

## 前置条件

- 有一台 Linux 机器或 AutoDL 实例
- 有 `Qwen3-0.6B` 模型路径
- 有 GitHub 访问权限
- 仓库已经 clone 到本机

## 读哪些文档和代码

- `README.md`
- `HANDOFF.md`
- `course/lessons/lesson-00-environment.md`
- `pyproject.toml`
- `example.py`
- `bench.py`

## 要做什么

### 1. 记录环境卡片

至少记录这些信息：

- 操作系统
- Python 版本
- CUDA / 驱动版本
- PyTorch 版本
- GPU 型号
- 模型路径

### 2. 安装和验证依赖

先用编辑模式安装：

```bash
pip install -e .
```

如果依赖缺失，先把缺失项补齐，再继续。

### 3. 跑最小生成

先只追求“能出字”，不要一开始就追求性能。

验证点：

- 能初始化 `LLM`
- 能 tokenizer
- 能生成一次最小输出

### 4. 跑 baseline benchmark

先跑 `bench.py` 的最小可行场景，至少得到一条 baseline 记录。

如果机器资源有限，可以先缩小批大小和长度，但要保留同一组对照条件。

### 5. 确认 Git 记录可回传

项目后续所有阶段都要能 commit / push。

### 建议先跑一遍的命令

```bash
python --version
nvidia-smi
python -c "import torch; print(torch.__version__); print(torch.cuda.is_available())"
pip install -e .
python example.py
python bench.py
git remote -v
```

## 怎么验证

这一阶段必须满足下面所有条件才算过：

- `import nanovllm` 成功
- `LLM(...)` 成功
- `python example.py` 成功
- 至少一条 benchmark 记录写进 `labs/bench.md`
- Git remote 正常
- 当前工作区可提交

## 本阶段产出

- 环境记录
- 基线 benchmark 记录
- 已知问题清单
- 可复用的安装命令

## 常见失败点

- Torch 和 GPU 架构不匹配
- `flash-attn` 装不上
- 模型路径不存在
- `pip install -e .` 失败
- `example.py` 因环境差异报错

## 下一步

Phase 0 结束后，再进入 Phase 1。
