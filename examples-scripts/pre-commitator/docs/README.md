# Pre-Commitator 文档

欢迎使用 Pre-Commitator 文档！本目录包含帮助您充分利用 Pre-Commitator 的指南和资源。

## 入门指南

- [快速入门指南](quickstart.md) - 5 分钟内快速上手
- [安装与配置](../SETUP.md) - 详细安装说明

## 故障排除与帮助

- [故障排除指南](troubleshooting.md) - 常见问题的解决方案
- [贡献指南](../CONTRIBUTING.md) - 如何为项目做贡献

## 参考资料

- [配置参考](../config/README.md) - 配置选项说明
- [主 README](../README.md) - 项目概述

## 目录结构

```
docs/
├── images/              # Diagrams and screenshots
├── quickstart.md        # Quick start guide for new users
├── troubleshooting.md   # Solutions to common issues
└── README.md            # This file
```

## 命令参考

| 命令 | 描述 |
|---------|-------------|
| `./install.sh` | 安装 Pre-Commitator |
| `./install.sh -y` | 非交互式安装 |
| `./run_quality_check.sh` | 检查暂存文件 |
| `./run_quality_check.sh file.py` | 检查特定文件 |
| `./run_quality_check.sh --all` | 检查所有仓库文件 |
| `./run_quality_check.sh --help` | 显示所有选项 |
