# 安装与配置指南

本指南提供了在开发环境中安装和配置 Pre-Commitator 的详细说明。

## 系统要求

- **操作系统**：macOS、Linux、Windows（推荐通过 WSL）
- **Python**：3.7 或更新版本
- **Git**：2.22.0 或更新版本
- **Node.js**：14.0 或更新版本（可选，用于 JavaScript 验证）

## 分步安装

### 1. 基本安装

最简单的安装方式是使用我们的安装脚本：

```bash
./install.sh
```

此脚本将会：
- 安装 Python 依赖
- 设置 pre-commit 钩子
- 使脚本可执行
- 配置路径
- 询问您是否要使用 VS Code 模式或终端模式
- 尝试安装 Horusec 安全扫描器（可选）

### 2. 手动安装

如果您希望手动安装：

#### 安装 Python 依赖

```bash
# Create and activate a virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
pip install pre-commit
```

#### 安装 JavaScript 依赖（可选）

```bash
npm install
```

#### 安装 Pre-commit 钩子

```bash
# Clean installation (recommended)
pre-commit uninstall
pre-commit install -f

# If you see "Running in migration mode" warnings, use:
pre-commit uninstall
pre-commit install -f
```

#### 设置执行权限

```bash
chmod +x run_quality_check.sh
chmod +x src/pre_commit_hook.sh
chmod +x src/quality_gate.py
chmod +x switch_mode.sh
chmod +x src/auto_stage_hook.sh
chmod +x src/install_horusec.sh
chmod +x fix_certificates.sh
```

#### 选择运行模式

根据您的开发环境选择要使用的模式：

```bash
# For VS Code users (disables problematic validators)
./switch_mode.sh vscode

# For terminal users (enables all validators)
./switch_mode.sh terminal
```

#### 安装自动暂存钩子（可选）

如果您希望在 pre-commit 钩子修复文件后自动暂存修改的文件：

```bash
# Install the auto-stage hook
./src/auto_stage_hook.sh
```

此钩子通过备份原始 pre-commit 钩子并创建一个新钩子来工作，新钩子会自动暂存由 pre-commit 钩子修改的文件。它在 VS Code 和终端模式下均可无缝工作，消除了 pre-commit 钩子修复问题后手动暂存文件的需要。

### 3. 可选工具安装

#### 安装 Semgrep（安全扫描器）

Semgrep 是一个用于发现代码安全问题的轻量级静态分析工具。

**所有平台**：
```bash
pip install semgrep==1.50.0
```

更多信息，请访问官方仓库：https://github.com/returntocorp/semgrep

#### 安装 Horusec（安全扫描器）

Horusec 是一个多语言安全扫描器。

```bash
# The install script will detect your OS and install appropriately
./src/install_horusec.sh
```

如果 Horusec 安装失败，钩子将被跳过，不影响其他验证。

#### 修复 SSL 证书问题（macOS）

如果在 macOS 上遇到 SSL 证书问题：

```bash
# Fix SSL certificate issues
./fix_certificates.sh
```

## 运行模式

Pre-Commitator 支持两种运行模式，以适应不同的开发环境：

### VS Code 模式

```bash
./switch_mode.sh vscode
```

VS Code 模式禁用了在使用 VS Code 的源代码控制集成时可能导致问题的某些验证器：

- 禁用 **ESLint**，防止 SSL 证书错误
- 禁用 **Lizard**、**Bandit** 和 **Semgrep**，防止"命令未找到"错误
- 使用环境变量控制哪些验证器处于活动状态
- 创建由 pre-commit 钩子加载的 `.env` 文件

### 终端模式

```bash
./switch_mode.sh terminal
```

终端模式启用所有验证器以进行全面的代码质量检查：

- 启用 **ESLint** 用于 JavaScript/TypeScript 验证
- 启用 **Lizard** 用于代码复杂度分析
- 启用 **Bandit** 用于 Python 安全检查
- 启用 **Semgrep** 用于多语言安全扫描
- 启用 **Horusec** 用于额外的安全扫描（如果已安装）

## 配置

### 自定义验证规则

编辑 `.pre-commit-config.yaml` 来修改验证设置：

```yaml
# Example: Adjust Lizard complexity thresholds
- repo: local
  hooks:
    - id: lizard
      name: Lizard (code complexity)
      entry: lizard
      language: python
      args: [
          '--warnings_only',
          '--CCN', '15',  # Change from 10 to 15
          '--length', '120',  # Change from 100 to 120
          '--arguments', '5',
        ]
```

### 特定模式配置

Pre-Commitator 为每种模式提供预定义的配置模板：

- `config/pre-commit-vscode.yaml` - VS Code 模式的配置
- `config/pre-commit-terminal.yaml` - 终端模式的配置

您可以自定义这些模板，以调整每种模式中启用的验证器。

### 环境变量

Pre-Commitator 使用环境变量来有选择地启用或禁用验证器：

- `DISABLE_ESLINT=1` - 禁用 ESLint JavaScript 验证
- `DISABLE_LIZARD=1` - 禁用 Lizard 复杂度分析
- `DISABLE_BANDIT=1` - 禁用 Bandit Python 安全检查
- `DISABLE_SEMGREP=1` - 禁用 Semgrep 安全扫描

这些环境变量由 `switch_mode.sh` 脚本创建的 `.env` 文件控制。

### 配置特定语言

#### Python 配置

修改 `.bandit.yml` 以更改安全扫描设置：

```yaml
# Add additional skips
skips:
  - B101  # assert used
  - B404  # subprocess without shell=True
```

#### JavaScript 配置

修改 `.eslintrc.json` 以自定义 JavaScript 代码检查：

```json
{
  "plugins": ["security"],
  "extends": ["plugin:security/recommended"],
  "rules": {
    "security/detect-object-injection": "warn",  # Change from error to warn
    "security/detect-non-literal-fs-filename": "error"
  }
}
```

## 与 CI/CD 集成

### GitHub Actions

创建 `.github/workflows/quality-check.yml`：

```yaml
name: Code Quality Check

on:
  pull_request:
    branches: [ main, develop ]

jobs:
  quality-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pre-commit
      - name: Run quality checks
        run: |
          ./run_quality_check.sh --all
```

## 故障排除

### 常见问题

1. **权限被拒绝**：
   ```bash
   chmod +x *.sh src/*.py src/*.sh
   ```

2. **Windows 上的行尾问题**：
   ```bash
   # Fix line endings
   sed -i 's/\r$//' *.sh src/*.sh
   ```

3. **缺少依赖**：
   ```bash
   # Verify Python installations
   pip list

   # Verify Node installations
   npm list
   ```

4. **macOS 上的 SSL 证书问题**：
   ```bash
   # Fix SSL certificate verification
   ./fix_certificates.sh

   # Or switch to VS Code mode
   ./switch_mode.sh vscode
   ```

5. **Pre-commit 脚本以迁移模式安装**：
   ```bash
   # Reinstall pre-commit hooks
   pre-commit uninstall
   pre-commit install -f
   ./switch_mode.sh vscode  # Or terminal mode
   ```

6. **自动暂存不工作**：
   ```bash
   # Reinstall auto-stage hook
   ./src/auto_stage_hook.sh

   # If still not working, check if the pre-commit hook was replaced
   ls -la .git/hooks/pre-commit*

   # Then reinstall again after switching modes
   ./switch_mode.sh terminal  # Or vscode mode
   ./src/auto_stage_hook.sh
   ```

### 获取帮助

如果您遇到持续性问题，请：

1. 检查所有依赖是否已安装
2. 验证文件权限
3. 检查 Python 和 Node.js 版本
4. 在我们的 GitHub 仓库上提交 issue

## 后续步骤

安装完成后，您可以：

1. 测试您的配置：`./run_quality_check.sh src/demo.py`
2. 查看 `config/` 目录中的示例配置
3. 通过复制相关文件集成到您现有的项目中
4. 根据需要在 VS Code 和终端模式之间切换
