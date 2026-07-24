# 运行模式

Pre-Commitator 支持两种运行模式，以适应不同的开发环境：VS Code 模式和终端模式。本文档解释这两种模式的工作原理以及如何在它们之间切换。

## 模式概述

### VS Code 模式

VS Code 模式专为使用 Visual Studio Code 集成源代码控制功能的开发者设计。它禁用了在 VS Code 中可能导致问题的某些验证器，特别是与 SSL 证书和"命令未找到"错误相关的问题。

在 VS Code 模式下：
- **ESLint** 被禁用，以防止 pre-commit 安装 Node.js 环境时出现 SSL 证书错误
- **Bandit** 被禁用，以防止 SSL 证书错误和"命令未找到"错误
- **Lizard** 被禁用，以防止"命令未找到"错误
- **Semgrep** 被禁用，以防止"命令未找到"错误
- 行尾空格、文件末尾换行等基本检查仍然启用
- Horusec 仍然被配置，但如果未安装则会被跳过

### 终端模式

终端模式专为使用终端命令进行 Git 操作的开发者设计。它启用所有验证器，以进行全面的代码质量和安全检查。

在终端模式下：
- **ESLint** 已启用，用于 JavaScript/TypeScript 验证
- **Lizard** 已启用，用于代码复杂度分析
- **Bandit** 已启用，用于 Python 安全检查
- **Semgrep** 已启用，用于安全扫描
- **Horusec** 已启用，用于额外的安全扫描（如果已安装）
- 所有基本检查也已启用

## 切换模式

您可以使用 `switch_mode.sh` 脚本在模式之间切换：

```bash
# Switch to VS Code mode
./switch_mode.sh vscode

# Switch to Terminal mode
./switch_mode.sh terminal

# Check current mode
./switch_mode.sh
```

## 工作原理

### 配置文件

模式切换通过两个配置模板实现：
- `config/pre-commit-vscode.yaml` - VS Code 模式的配置
- `config/pre-commit-terminal.yaml` - 终端模式的配置

切换模式时，相应的配置文件会被复制为 `.pre-commit-config.yaml`。

### 环境变量

除配置文件外，VS Code 模式还使用环境变量来有选择地禁用质量门中的验证器：

- `DISABLE_ESLINT=1` - 禁用 ESLint JavaScript 验证
- `DISABLE_LIZARD=1` - 禁用 Lizard 复杂度分析
- `DISABLE_BANDIT=1` - 禁用 Bandit Python 安全检查
- `DISABLE_SEMGREP=1` - 禁用 Semgrep 安全扫描

这些环境变量设置在由 `switch_mode.sh` 脚本创建的 `.env` 文件中。pre-commit 钩子在运行时加载这些环境变量。

这对于防止 macOS 上常见的 SSL 证书错误特别有效，因为当 ESLint 和 Bandit 等工具需要发出 HTTPS 请求时会发生这些错误。VS Code 模式可以有效绕过这些容易出错的操作。

### .env 文件

在 VS Code 模式下，将创建内容类似如下的 `.env` 文件：

```
# Created by switch_mode.sh
# This file disables certain tools in the quality gate for VS Code compatibility
# Prevents SSL certificate errors and "command not found" errors
DISABLE_ESLINT=1  # Prevents SSL certificate errors during pre-commit install
DISABLE_LIZARD=1  # Prevents "command not found" errors
DISABLE_BANDIT=1  # Prevents both SSL certificate errors and "command not found" errors
DISABLE_SEMGREP=1 # Prevents "command not found" errors
```

此文件由 pre-commit 钩子读取，环境变量被传递给 Python 质量门脚本。

在终端模式下，`.env` 文件会被删除，允许所有验证器运行。

## 实现细节

### 质量门脚本

质量门脚本（`src/quality_gate.py`）检查这些环境变量，如果已设置则跳过相应的验证器：

```python
# Example for ESLint
if os.environ.get("DISABLE_ESLINT") == "1":
    self.warnings.append("ESLint skipped: Disabled by DISABLE_ESLINT=1 environment variable")
    return
```

### Pre-Commit 钩子

pre-commit 钩子脚本（`src/pre_commit_hook.sh`）从 `.env` 文件加载环境变量：

```bash
# Get the absolute path of the script's directory
SCRIPT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

# Source .env file if it exists
if [ -f "$SCRIPT_DIR/../.env" ]; then
    echo "Loading environment from $SCRIPT_DIR/../.env"
    source "$SCRIPT_DIR/../.env"
    # Export to make it available to Python subprocess
    export DISABLE_ESLINT
    export DISABLE_LIZARD
    export DISABLE_BANDIT
    export DISABLE_SEMGREP
fi
```

## 自定义模式

您可以通过编辑特定模式的配置文件来自定义每种模式中启用的验证器：

1. 编辑 `config/pre-commit-vscode.yaml` 以自定义 VS Code 模式
2. 编辑 `config/pre-commit-terminal.yaml` 以自定义终端模式

您还可以通过创建新的配置模板并更新 `switch_mode.sh` 脚本以支持它们，来创建额外的模式。

## 最佳实践

- 如果您主要使用 VS Code 进行 Git 操作，请使用 **VS Code 模式**
- 如果您想获得全范围的验证并使用终端命令进行 Git 操作，请使用**终端模式**
- 如果您想检查在 VS Code 模式下可能被禁用的问题，请在提交前手动运行质量检查（`./run_quality_check.sh`）
