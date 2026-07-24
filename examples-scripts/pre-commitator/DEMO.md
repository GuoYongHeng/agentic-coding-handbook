# Pre-Commitator 演示指南

本指南提供了从完全空白状态演示 Pre-Commitator 功能的分步说明。专为培训员工了解所有可能的使用场景而设计。此文件已从 git 中排除，以保持仓库整洁。

## 从零开始

### 环境搭建

1. **前提条件**

   - 确保已安装 Python 3.7+
   - 确保已安装 Git
   - 确保已安装 Node.js 14+（用于 JavaScript 验证）

   通过以下命令验证：

   ```bash
   python3 --version
   git --version
   node --version
   ```

2. **克隆仓库**

   ```bash
   # Start in a clean directory
   cd ~/Desktop

   # Clone the repository
   git clone https://github.com/yourusername/pre-commitator.git

   # Navigate to the project
   cd pre-commitator
   ```

3. **安装**

   ```bash
   # Install with interactive prompts (recommended for first-time setup)
   ./install.sh

   # Or non-interactive installation
   ./install.sh -y

   # Or verbose installation to see detailed progress
   ./install.sh -v
   ```

4. **验证安装**

   ```bash
   # Check if all dependencies are correctly installed
   python3 test_dependencies.py
   ```

5. **运行测试套件**
   ```bash
   # Verify all components work together
   ./run_tests.sh
   ```

## 基本使用示例

### 单文件验证

1. **验证单个 Python 文件**

   ```bash
   # 1. Example with a clean file (should pass)
   ./run_quality_check.sh tests/test_clean.py

   # 2. Example with security warnings (should show warnings)
   ./run_quality_check.sh src/demo.py

   # 3. Example with critical issues (should fail)
   ./run_quality_check.sh tests/test_python_issues.py
   ```

2. **验证单个 JavaScript 文件**

   ```bash
   # 1. JavaScript file with security and style issues
   ./run_quality_check.sh src/demo.js

   # 2. JavaScript test file with security vulnerabilities
   ./run_quality_check.sh tests/test_js_issues.js
   ```

### 批量验证

1. **验证多个文件**

   ```bash
   # Check multiple specific files at once
   ./run_quality_check.sh src/demo.py tests/test_clean.py

   # Show the difference in outputs for different file types
   ./run_quality_check.sh src/demo.py src/demo.js
   ```

2. **验证所有文件**
   ```bash
   # Validate all tracked files in the repository
   ./run_quality_check.sh --all
   ```

## 命令行选项

1. **帮助命令**

   ```bash
   # Display all available options
   ./run_quality_check.sh --help
   ```

2. **输出选项**

   ```bash
   # Verbose output
   ./run_quality_check.sh --verbose src/demo.py

   # Quiet output (minimal information)
   ./run_quality_check.sh --quiet tests/test_clean.py
   ```

## Git 集成工作流

1. **设置 Pre-commit 钩子**

   ```bash
   # Ensure pre-commit is installed
   pip install pre-commit

   # Install pre-commit hooks
   pre-commit install
   ```

2. **暂存文件验证**

   ```bash
   # Clear any staged files
   git reset

   # Stage a file with no issues
   git add tests/test_clean.py

   # Validate only staged files (default behavior)
   ./run_quality_check.sh
   ```

3. **验证失败工作流**

   ```bash
   # Stage a file with issues
   git add src/demo.js

   # Validate staged files
   ./run_quality_check.sh

   # See pre-commit hook in action (will block commit due to issues)
   ./src/pre_commit_hook.sh
   ```

4. **绕过验证（仅限紧急情况）**

   ```bash
   # Note: This would be used in a real scenario
   # git commit --no-verify -m "Emergency fix, bypass validation"

   # Instead, just demonstrate the command's existence
   echo "In emergency situations only: git commit --no-verify"
   ```

## 高级用例

### 自定义配置

1. **查看配置**

   ```bash
   # View existing configuration examples
   cat config/settings.yaml
   ```

2. **项目专属设置**（供未来演示）
   ```bash
   # Coming soon: Project-specific configuration
   # ./run_quality_check.sh --config custom_config.yaml
   ```

### 与 AI 生成代码配合使用

1. **创建示例 AI 生成代码**

   ```bash
   # Create a new file simulating AI-generated code with issues
   cat > ai_generated.py << 'EOF'
   #!/usr/bin/env python3
   """
   Example AI-generated code with security and quality issues
   """
   import os
   import subprocess
   import pickle

   # Security issue: eval with user input
   def process_input(user_input):
       return eval(user_input)

   # Complexity issue: too many parameters and nested conditions
   def complex_calculation(a, b, c, d, e, f, g, h):
       result = 0
       if a > 0:
           if b > c:
               if d < e:
                   if f > g:
                       result = a + b + c + d + e + f + g + h
                   else:
                       result = a + b + c + d + e
               else:
                   result = a + b + c
           else:
               result = a
       return result

   # Another security issue: using pickle
   def save_data(data):
       with open("data.pickle", "wb") as f:
           pickle.dump(data, f)

   # Main function with shell injection risk
   def main():
       user_input = input("Enter expression: ")
       print(process_input(user_input))

       # Shell injection risk
       cmd = input("Enter command: ")
       subprocess.run(cmd, shell=True)

   if __name__ == "__main__":
       main()
   EOF
   ```

2. **验证 AI 生成的代码**

   ```bash
   # Run validation on the AI-generated file
   ./run_quality_check.sh ai_generated.py
   ```

3. **根据反馈修复问题**

   ```bash
   # Create fixed version based on Pre-Commitator feedback
   cat > ai_generated_fixed.py << 'EOF'
   #!/usr/bin/env python3
   """
   Example AI-generated code with fixes applied
   """
   import ast
   import subprocess
   import json

   # Fixed: Using ast.literal_eval instead of eval
   def process_input(user_input):
       try:
           return ast.literal_eval(user_input)
       except (SyntaxError, ValueError):
           return None

   # Fixed: Reduced complexity, fewer parameters
   def calculate_result(values):
       """Calculate result from a list of values with simple logic."""
       if not values or len(values) < 2:
           return 0

       result = sum(values)
       if values[0] > values[1]:
           result *= 2
       return result

   # Fixed: Using JSON instead of pickle
   def save_data(data):
       with open("data.json", "w") as f:
           json.dump(data, f)

   # Fixed: No shell=True, uses list of arguments
   def run_command(command_args):
       if isinstance(command_args, str):
           command_args = command_args.split()
       return subprocess.run(command_args, capture_output=True, text=True, shell=False)

   # Main function with fixes
   def main():
       try:
           user_input = input("Enter a tuple or list of numbers: ")
           result = process_input(user_input)
           print(f"Result: {result}")

           # Safe command execution
           command = ["ls", "-la"]
           output = run_command(command)
           print(output.stdout)

       except Exception as e:
           print(f"Error: {e}")

   if __name__ == "__main__":
       main()
   EOF
   ```

4. **验证修复后的代码**
   ```bash
   # Run validation on the fixed file
   ./run_quality_check.sh ai_generated_fixed.py
   ```

### 特定语言功能

1. **Python 专属功能**

   ```bash
   # Demonstrate Python-specific security checks (bandit)
   ./run_quality_check.sh --verbose tests/test_python_issues.py | grep "bandit"
   ```

2. **JavaScript 专属功能**
   ```bash
   # Demonstrate JavaScript-specific security checks (ESLint)
   ./run_quality_check.sh --verbose tests/test_js_issues.js | grep "ESLint"
   ```

## 真实世界工作流示例

1. **创建新功能分支**

   ```bash
   # Create and checkout a feature branch
   git checkout -b feature/new-calculation
   ```

2. **创建一个有问题的新文件**

   ```bash
   # Create a new Python file with an issue
   cat > src/calculate.py << 'EOF'
   #!/usr/bin/env python3
   """
   New calculation module
   """

   def advanced_calculation(x, y):
       """Perform calculation based on inputs."""
       # Security issue: eval with user input
       return eval(f"{x} * {y}")

   def main():
       result = advanced_calculation(5, 10)
       print(f"Result: {result}")

   if __name__ == "__main__":
       main()
   EOF
   ```

3. **尝试提交（应该失败）**

   ```bash
   # Stage the new file
   git add src/calculate.py

   # Attempt to commit - pre-commit hook should block this
   git commit -m "Add new calculation function"
   ```

4. **修复问题**

   ```bash
   # Fix the file based on feedback
   cat > src/calculate.py << 'EOF'
   #!/usr/bin/env python3
   """
   New calculation module
   """

   def advanced_calculation(x, y):
       """Perform calculation based on inputs."""
       # Fixed: Direct calculation instead of eval
       return x * y

   def main():
       result = advanced_calculation(5, 10)
       print(f"Result: {result}")

   if __name__ == "__main__":
       main()
   EOF
   ```

5. **成功提交**

   ```bash
   # Stage the fixed file
   git add src/calculate.py

   # Verify it passes checks
   ./run_quality_check.sh

   # Now commit should succeed
   git commit -m "Add new calculation function"
   ```

6. **清理**
   ```bash
   # Discard the feature branch
   git checkout main
   git branch -D feature/new-calculation
   ```

## 演示期间的故障排除

- **缺少依赖**

  ```bash
  # If Python dependencies are missing
  pip install -r requirements.txt

  # If Node dependencies are missing
  npm install
  ```

- **权限问题**

  ```bash
  # Make scripts executable
  chmod +x *.sh src/*.sh
  ```

- **虚拟环境**

  ```bash
  # Create and activate virtual environment
  python -m venv venv
  source venv/bin/activate
  ```

- **SSL 证书问题**

  ```bash
  # Option 1: Use our certificate fix scripts
  ./fix_certificates.sh  # for macOS
  # OR
  ./fix_certificates_unix.sh  # for other Unix systems

  # Option 2: Set PYTHONPATH to use our SSL context fix
  export PYTHONPATH=$PWD
  echo "SSL certificate verification disabled (temporary fix)"

  # Option 3: Fix Python certificates properly (macOS)
  open /Applications/Python\ 3.x/Install\ Certificates.command

  # Option 4: Switch to VS Code mode (disables problematic validators)
  ./switch_mode.sh vscode
  ```

- **Pre-commit SSL 错误**

  ```bash
  # If pre-commit has SSL errors when downloading Node.js
  # Option 1: Switch to VS Code mode (recommended & easiest)
  ./switch_mode.sh vscode

  # Option 2: Install Node.js manually first
  brew install node  # On macOS with Homebrew

  # Option 3: Create a pre-commit config exclude
  cat > ~/.config/pre-commit/config.yaml << 'EOF'
  no_node: true
  EOF

  # Option 4: Run Python with our SSL context fix
  export PYTHONPATH=$PWD
  ```

## 演讲者备注

- 重点介绍专为人类和 AI 设计的清晰错误信息
- 强调与语言无关的能力
- 展示安全问题如何被优先标记为错误
- 演示该工具如何集成到 git 工作流中
- 解释 AI 友好的错误信息如何帮助改进 AI 生成的代码
- 展示模块化架构如何支持未来扩展
