# 为 Pre-Commitator 做贡献

感谢您有兴趣为 Pre-Commitator 做贡献！本文档提供了为项目做贡献的指南和说明。

## 入门指南

1. 在 GitHub 上 **Fork 仓库**
2. 将您的 fork **克隆**到本地机器
3. 运行 `./install.sh` **安装依赖**
4. 为您的功能或 bugfix **创建新分支**
5. 遵循我们的代码风格规范**进行更改**
6. **运行测试**以确保您的更改不会破坏现有功能
7. 从您的 fork 向主仓库**提交 Pull Request**

## 开发环境搭建

```bash
# 克隆仓库
git clone https://github.com/yourusername/pre-commitator.git
cd pre-commitator

# 安装开发依赖
./install.sh

# 创建虚拟环境（推荐）
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 安装额外的开发依赖
pip install pytest pytest-cov mypy

# 如果遇到 SSL 证书问题（尤其是在 macOS 上）
./fix_certificates.sh  # for macOS
# OR
./fix_certificates_unix.sh  # for other Unix systems
# OR set the PYTHONPATH environment variable
export PYTHONPATH=$PWD  # Points to the SSL context fix
```

## 项目结构

```
pre-commitator/
├── config/              # Configuration files
├── docs/                # Documentation
├── src/                 # Source code
│   ├── __init__.py
│   ├── quality_gate.py  # Main validation engine
│   └── pre_commit_hook.sh # Pre-commit hook script
├── .pre-commit-config.yaml # Pre-commit configuration
├── install.sh           # Installation script
├── run_quality_check.sh # Main CLI script
└── README.md           # Project documentation
```

## 编码规范

我们遵循以下规范：

- **Python**：PEP 8 风格指南，由 Black 格式化工具强制执行
- **JavaScript**：带安全插件的 ESLint
- **Shell 脚本**：符合 Shellcheck 规范

## 添加新的验证器

要添加新的验证工具：

1. 将依赖添加到 `requirements.txt` 或 `package.json`
2. 在 `src/quality_gate.py` 中创建新方法
3. 从 `run_all_checks` 方法中调用您的方法
4. 添加相应的测试
5. 更新文档

添加新验证器的示例：

```python
def run_new_validator(self, files: List[str]) -> None:
    """Run new validation tool."""
    # Implementation here
    try:
        # Run the tool
        # Parse results
        # Add errors/warnings
    except FileNotFoundError:
        self.warnings.append("Warning: New tool not found. Install with 'pip install new-tool'")
```

## 测试

在提交 Pull Request 之前，请确保所有测试通过：

```bash
# 运行所有测试
./run_tests.sh

# 测试您的更改
./run_quality_check.sh --all

# 使用示例问题代码进行测试
./run_quality_check.sh src/demo.py src/demo.js
```

### 使用测试工件目录

测试 Pre-Commitator 时，请将所有临时测试文件放在 `tests/artifacts/` 目录中：

1. **创建测试文件**：
   ```bash
   # Create a test file with trailing whitespace
   echo "This has trailing spaces    " > tests/artifacts/test_whitespace.txt
   ```

2. **命名规范**：
   - 所有测试文件以 `test_` 为前缀
   - 使用能说明用途的描述性名称
   - 示例：`test_trailing_whitespace.txt`

3. **清理**：
   - 测试完成后清理测试文件
   - 该目录已被 git 忽略，以防意外提交

### 自动暂存钩子测试

对自动暂存钩子进行更改时，务必在 VS Code 和终端两种模式下进行测试：

```bash
# Install the auto-stage hook
./src/auto_stage_hook.sh

# Switch to VS Code mode and test
./switch_mode.sh vscode
# Create a test file in the artifacts directory
echo "Test file with trailing spaces    " > tests/artifacts/test_whitespace.txt
git add tests/artifacts/test_whitespace.txt
git commit -m "Testing auto-stage hook"
# Verify the hook works correctly

# Switch to terminal mode and test
./switch_mode.sh terminal
# Test with another file that will trigger hooks
echo "Another test file with trailing spaces    " > tests/artifacts/test_whitespace2.txt
git add tests/artifacts/test_whitespace2.txt
git commit -m "Testing auto-stage hook in terminal mode"
# Verify the hook works correctly

# Clean up test files
rm tests/artifacts/test_whitespace*.txt
```

## Pull Request 流程

1. 确保您的代码遵循我们的风格规范
2. 如有必要，更新文档
3. 确保所有测试通过
4. 如有必要，用您的更改详情更新 README.md
5. PR 应针对 main 分支

## 安全注意事项

- 所有依赖必须来自可信的经过验证的来源
- 主验证逻辑中不得有外部 HTTP 调用
- 安全检查应偏向谨慎
- 不得有硬编码的凭据或敏感信息

## 处理 SSL 证书问题

在开发 Pre-Commitator 时，您可能会遇到 SSL 证书验证错误，尤其是在 macOS 上。这通常发生在 pre-commit 尝试安装 Node.js 环境时，或者 Python 安全工具（如 Bandit）发出 HTTPS 请求时。

### 常见 SSL 错误症状

```
[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate
```

### 解决方案（按优先级排序）

1. **切换到 VS Code 模式**：用于测试/开发
   ```bash
   ./switch_mode.sh vscode
   ```

2. **修复证书**：
   ```bash
   ./fix_certificates.sh  # macOS
   ./fix_certificates_unix.sh  # Other Unix systems
   ```

3. **设置 PYTHONPATH**：指向我们的临时 SSL 上下文修复
   ```bash
   export PYTHONPATH=$PWD
   ```

4. **安装 Python 证书**：用于正确的 macOS 证书安装
   ```bash
   open /Applications/Python\ 3.x/Install\ Certificates.command
   ```

请记得记录您所做的任何与 SSL 相关的更改，因为它们会影响开发者和用户的体验。

## 代码审查标准

Pull Request 将根据以下标准进行评估：

- 代码质量和风格
- 测试覆盖率
- 文档
- 安全影响
- 性能考量

## 许可证

通过为 Pre-Commitator 做贡献，您同意您的贡献将在项目许可证下授权。

感谢您为让 Pre-Commitator 变得更好而做出的贡献！
