# Pre-Commitator

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Verified Sources](https://img.shields.io/badge/dependencies-verified-brightgreen)

一个强大的提交前代码质量验证工具，确保您的代码在提交之前符合质量和安全标准。

## 概述

Pre-Commitator 旨在在开发过程中的最早阶段——代码提交到仓库之前——检测代码质量和安全问题。它特别适用于验证 AI 生成的代码，并确保所有代码贡献符合项目的质量标准。

> **安全说明**：Pre-Commitator 仅使用经过验证的可信来源作为所有依赖项。所有工具均来自官方仓库（PyPI、npm）以及 PSF（Black）、PyCQA（Bandit、Flake8）和 r2c（Semgrep）等知名组织。

## 功能特性

- **✅ 多语言支持**：完全支持 Python、JavaScript 和 TypeScript，架构已准备好支持其他语言
- **⚡ 快速执行**：仅扫描已暂存待提交的文件或您选择的特定文件
- **🧠 AI 友好**：提供人类和 AI 都能理解的清晰错误信息
- **🔍 全面验证**：
  - 代码复杂度分析
  - 安全漏洞检测
  - 代码风格和格式
  - 最佳实践强制执行

## 系统要求

- Python 3.7+
- Git
- Node.js 14+（可选，用于 JavaScript 验证）

## 快速开始

### 新开发者（90 秒安装）

```bash
# One command setup (copy & paste this)
git clone https://github.com/yourusername/pre-commitator.git && cd pre-commitator && ./install.sh -y
```

![Getting Started Diagram](docs/images/getting-started.png)

### 安装选项

```bash
# Interactive installation (recommended for first-time users)
./install.sh

# Non-interactive installation
./install.sh -y

# Installation with detailed output
./install.sh -v
```

安装程序将会：

- 安装所需的 Python 依赖
- 设置 pre-commit 钩子
- 配置工具以供立即使用
- 询问您是否要使用 VS Code 模式或终端模式

### 基本使用

```bash
# Check staged files (files about to be committed)
./run_quality_check.sh

# Check specific files
./run_quality_check.sh path/to/file1.py path/to/file2.js

# Check all files in the repository
./run_quality_check.sh --all

# Get help
./run_quality_check.sh --help

# Switch between VS Code and terminal modes
./switch_mode.sh vscode  # For VS Code users (disables problematic validators)
./switch_mode.sh terminal  # For terminal users (enables all validators)
```

### 查看我们的文档

- [快速入门指南](docs/quickstart.md) - 5 分钟内快速上手
- [故障排除](docs/troubleshooting.md) - 常见问题的解决方案
- [贡献指南](CONTRIBUTING.md) - 了解如何贡献

## 运行模式

Pre-Commitator 提供两种运行模式，以适应不同的开发环境：

### VS Code 模式

```bash
./switch_mode.sh vscode
```

VS Code 模式禁用了在使用 VS Code 的源代码控制集成时可能导致问题的某些验证器：

- 禁用 **ESLint**，防止从 pre-commit 安装时出现 SSL 证书错误
- 禁用 **Bandit**，防止 SSL 证书错误和"命令未找到"错误
- 禁用 **Lizard** 和 **Semgrep**，防止"命令未找到"错误
- 确保在 VS Code 内 Git 操作顺畅
- 仍运行行尾空格、文件末尾换行等基本检查

推荐主要使用 VS Code 进行 Git 操作的开发者使用此模式。

### 终端模式

```bash
./switch_mode.sh terminal
```

终端模式启用所有验证器，以进行全面的代码质量和安全检查：

- 启用 **ESLint** 用于 JavaScript/TypeScript 验证
- 启用 **Lizard** 用于代码复杂度分析
- 启用 **Bandit** 用于 Python 安全检查
- 启用 **Semgrep** 用于多语言安全扫描
- 启用 **Horusec** 用于额外的安全扫描（如果已安装）

推荐使用终端命令进行 Git 操作并希望获得全范围验证的开发者使用此模式。

## 详细文档

### 验证类型

Pre-Commitator 执行多种类型的验证：

1. **代码复杂度**

   - 圈复杂度（CCN < 10）
   - 函数长度（< 100 行）
   - 函数参数（< 5 个参数）

2. **安全漏洞**

   - Python：Bandit 识别常见安全问题（来自 PyCQA）
   - JavaScript：ESLint 安全插件检测 Web 漏洞（官方 ESLint 插件）
   - 多语言：Semgrep 扫描更广泛的安全问题（来自 r2c）
   - 多语言：Horusec 检测多种语言的安全漏洞（可选）

3. **代码风格**
   - Python：Black 格式化检查
   - JavaScript/TypeScript：ESLint 用于风格强制执行
   - 通用：行尾空格、文件末尾换行符

### 配置

Pre-Commitator 使用 pre-commit 的配置系统。编辑 `.pre-commit-config.yaml` 以：

- 调整阈值（复杂度、长度等）
- 添加或删除验证钩子
- 配置特定语言的工具

对于特定模式的配置，请使用预定义的模板：

- `config/pre-commit-vscode.yaml` - VS Code 模式的配置
- `config/pre-commit-terminal.yaml` - 终端模式的配置

## 环境变量

Pre-Commitator 使用环境变量来有选择地启用或禁用特定验证器：

- `DISABLE_ESLINT=1` - 禁用 ESLint JavaScript 验证
- `DISABLE_LIZARD=1` - 禁用 Lizard 复杂度分析
- `DISABLE_BANDIT=1` - 禁用 Bandit Python 安全检查
- `DISABLE_SEMGREP=1` - 禁用 Semgrep 安全扫描

这些环境变量由 `switch_mode.sh` 脚本根据选择的模式自动设置。在 VS Code 模式下，这些验证器被禁用以防止 SSL 证书和"命令未找到"错误。

## 自动暂存功能

Pre-Commitator 包含一个自动暂存功能，可以自动暂存由 pre-commit 钩子修改的文件：

```bash
# Install the auto-stage hook
./src/auto_stage_hook.sh
```

此功能解决了一个常见痛点：pre-commit 钩子修复问题（如行尾空格），但需要您在再次提交之前手动暂存这些更改。

启用自动暂存钩子后：

1. pre-commit 钩子运行并可能修改文件
2. 修改的文件自动被暂存
3. 您只需再次运行 `git commit` 即可完成提交

## 与 AI 生成代码配合使用

Pre-Commitator 与 AI 编程助手配合效果极佳：

1. 使用 AI 助手生成代码
2. 将代码保存到文件
3. 运行 `./run_quality_check.sh filename` 进行验证
4. 如果发现问题，请 AI 根据具体错误信息进行修复
5. 重新运行验证，直到所有问题得到解决

## 故障排除

### 常见问题

**Q：提交时 pre-commit 钩子没有运行。**
A：确保已使用 `pre-commit install` 或运行安装程序安装了 pre-commit 钩子。

**Q：我收到"命令未找到"错误。**
A：确保所有依赖均已安装。重新运行 `./install.sh`。

**Q：如何临时绕过检查？**
A：使用 `git commit --no-verify`（不推荐用于生产代码）。

**Q：Pre-commit 钩子修复了问题，但我必须在再次提交之前手动暂存修复后的文件。**
A：安装自动暂存钩子：`./src/auto_stage_hook.sh`。这将自动暂存由 pre-commit 钩子修改的文件。

**Q：安装 eslint 环境或运行其他 pre-commit 钩子时出现 SSL 证书错误，如 `[SSL: CERTIFICATE_VERIFY_FAILED]`。**
A：这是 macOS 上 Python SSL 证书验证的常见问题。您有三个选项：

- 运行 `./fix_certificates.sh` 或 `./fix_certificates_unix.sh` 修复 Python SSL 证书验证
  - 这将创建一个临时 SSL 上下文修复并设置 PYTHONPATH 环境变量
- 手动设置 PYTHONPATH：`export PYTHONPATH=/path/to/pre-commitator`
- 切换到 VS Code 模式：`./switch_mode.sh vscode`（禁用需要网络访问的有问题验证器）

VS Code 模式选项是最可靠的解决方案，因为它完全绕过了证书验证问题。

**Q：我看到关于"pre-commit 脚本以迁移模式安装"的警告。**
A：当安装了多个 pre-commit 钩子时会发生这种情况。解决方法如下：

```bash
pre-commit uninstall
pre-commit install -f
./switch_mode.sh vscode  # Or terminal, depending on your preference
```

**Q：如何安装 Horusec？**
A：Horusec 的安装由 `./src/install_horusec.sh` 脚本处理，运行 `./switch_mode.sh` 时会调用该脚本。如果安装失败，Horusec 钩子将被跳过，而不会影响其他验证。

## 贡献

欢迎贡献！请随时提交 Pull Request。

## 许可证

本项目采用 MIT 许可证 - 详情请参阅 LICENSE 文件。
