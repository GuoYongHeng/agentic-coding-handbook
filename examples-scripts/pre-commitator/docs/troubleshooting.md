# 故障排除指南

本文档提供了使用 Pre-Commitator 时可能遇到的常见问题的解决方案。

## 安装问题

### 找不到 Python

**错误**：`Python 3 is required but not installed`

**解决方案**：
1. 从 [python.org](https://www.python.org/downloads/) 安装 Python 3.7 或更新版本
2. 确保 Python 已添加到您的 PATH
3. 使用 `python3 --version` 进行验证

### 运行脚本时权限被拒绝

**错误**：`Permission denied: ./install.sh`

**解决方案**：
```bash
chmod +x *.sh src/*.sh src/*.py
```

### Windows 上的行尾问题

**错误**：`bad interpreter: /bin/bash^M: no such file or directory`

**解决方案**：
```bash
# Option 1: Using sed
sed -i 's/\r$//' *.sh src/*.sh

# Option 2: Using dos2unix
dos2unix *.sh src/*.sh
```

## 运行模式问题

### VS Code 提交问题

**错误**：从 VS Code 提交时出现 SSL 证书错误

**解决方案**：
```bash
# Switch to VS Code mode
./switch_mode.sh vscode
```

此模式禁用 ESLint、Lizard、Bandit 和 Semgrep，以防止 VS Code 中的 SSL 证书问题和"命令未找到"错误。

### Pre-commit 脚本以迁移模式安装

**错误**：`Running in migration mode with existing hooks at .git/hooks/pre-commit.legacy`

**解决方案**：
```bash
# Completely reinstall pre-commit hooks
pre-commit uninstall
pre-commit install -f

# Then reactivate your preferred mode
./switch_mode.sh vscode  # Or terminal mode

# Reinstall the auto-stage hook
./src/auto_stage_hook.sh
```

当 pre-commit 检测到多个钩子安装或之前安装的遗留物时，会出现此错误。解决方法是完全卸载，然后强制重新安装钩子。之后，请记得重新安装自动暂存钩子，以启用钩子修改文件的自动暂存功能。

### SSL 证书错误

**错误**：`[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed`

**解决方案**：
```bash
# Option 1: Fix certificates
./fix_certificates.sh

# Option 2: Switch to VS Code mode (disables problematic validators)
./switch_mode.sh vscode
```

## 自动暂存问题

### Pre-commit 修复问题但需要手动暂存

**问题**：pre-commit 钩子修复了问题（如行尾空格），但您必须手动暂存它们

**解决方案**：
```bash
# Install the auto-stage hook
./src/auto_stage_hook.sh
```

这将自动暂存由 pre-commit 钩子修改的文件，让您只需再次运行 `git commit` 即可完成提交。

### 自动暂存钩子不工作

**问题**：由 pre-commit 钩子修改的文件未自动暂存

**解决方案**：
```bash
# Check if auto-stage hook is installed
ls -la .git/hooks/pre-commit.original

# Reinstall the auto-stage hook
./src/auto_stage_hook.sh
```

## Horusec 问题

### Horusec 安装失败

**错误**：`Horusec installation could not be completed`

**解决方案**：
这是正常的，不会影响其他验证。Horusec 被设计为可选的，如果未安装，钩子将跳过它。如果您想手动安装，可以尝试：

```bash
# Manual installation attempt
./src/install_horusec.sh
```

## 运行时问题

### 没有要检查的文件

**错误**：`No files to check` 或 `No staged files found`

**解决方案**：
1. 确保已使用 `git add` 暂存文件
2. 明确指定文件：`./run_quality_check.sh path/to/file.py`

### Pre-commit 未初始化

**错误**：`pre-commit: command not found` 或提交时钩子未运行

**解决方案**：
```bash
# Install pre-commit
pip install pre-commit

# Install the hooks
pre-commit install
```

### 缺少依赖

**错误**：`Warning: Lizard not found` 或类似关于缺少工具的信息

**解决方案**：
```bash
# Install Python dependencies
pip install -r requirements.txt

# Install JavaScript dependencies
npm install

# Or switch to VS Code mode to disable problematic validators
./switch_mode.sh vscode
```

## 验证问题

### 复杂度错误

**错误**：`warning: someFunction has 25 NLOC, 15 CCN...`

**解决方案**：
1. 将复杂函数重构为更小、更专注的函数
2. 减少嵌套条件，考虑使用提前返回
3. 或在 `.pre-commit-config.yaml` 中调整阈值：
   ```yaml
   - repo: local
     hooks:
       - id: lizard
         args: [
             '--CCN', '15',  # Increase from 10 to 15
             '--length', '120',  # Increase from 100 to 120
           ]
   ```

### 安全问题

**错误**：`Security: subprocess call with shell=True identified`

**解决方案**：
```python
# Bad
subprocess.check_output(command, shell=True)

# Good
subprocess.check_output(command.split())
```

**错误**：`Use of possibly insecure function - consider using safer ast.literal_eval`

**解决方案**：
```python
# Bad
result = eval(user_input)

# Good
import ast
result = ast.literal_eval(user_input)
```

### JavaScript 安全问题

**错误**：`ESLint: Object injection vulnerability detected`

**解决方案**：
```javascript
// Bad
const userData = users[userInput];

// Good
const allowedUsers = ['admin', 'user'];
if (allowedUsers.includes(userInput)) {
  const userData = users[userInput];
}
```

## 高级故障排除

### 调试模式

要获取更详细的输出，请使用 verbose 标志：

```bash
./run_quality_check.sh --verbose
```

### 环境变量

要检查哪些环境变量用于控制验证器：

```bash
# View the .env file content
cat .env

# Check if environment variables are loaded in the pre-commit hook
export -p | grep DISABLE
```

### Git 钩子未运行

如果 pre-commit 钩子未自动运行：

1. 检查钩子是否已安装：
   ```bash
   ls -la .git/hooks/
   ```

2. 重新安装钩子：
   ```bash
   pre-commit install
   ```

3. 验证 pre-commit 是否已配置：
   ```bash
   pre-commit --version
   ```

### 自定义规则不起作用

如果您的自定义验证规则未被应用：

1. 检查 `.pre-commit-config.yaml` 中的格式
2. 使用 verbose 输出运行以查看详细错误
3. 测试单个钩子：
   ```bash
   pre-commit run lizard --verbose
   ```

## 模式切换

如果需要在 VS Code 和终端模式之间切换：

```bash
# Check current mode
./switch_mode.sh

# Switch to VS Code mode
./switch_mode.sh vscode

# Switch to terminal mode
./switch_mode.sh terminal
```

## 获取更多帮助

如果您的问题在此处未得到解决：

1. 检查 `.pre-commit-config.yaml` 文件是否有配置问题
2. 在 verbose 模式下查看日志
3. 查阅 pre-commit 和各验证工具的文档
4. 在我们的 GitHub 仓库上提交 issue
