# 新开发者快速入门指南

欢迎使用 Pre-Commitator 项目！本指南将帮助您在 5 分钟内快速上手。

## 即时配置

```bash
# One command setup (copy & paste this)
git clone https://github.com/yourusername/pre-commitator.git && cd pre-commitator && ./install.sh -y
```

## Pre-Commitator 的作用是什么？

Pre-Commitator 自动检查您的代码是否存在：
- 安全漏洞
- 代码复杂度问题
- 格式问题
- 最佳实践违规

它在代码提交到仓库**之前**运行这些检查，防止有问题的代码被提交。

## 基本命令

| 命令 | 描述 |
|---------|-------------|
| `./run_quality_check.sh file.py` | 检查特定文件 |
| `./run_quality_check.sh` | 检查暂存文件 |
| `./run_quality_check.sh --all` | 检查所有仓库文件 |
| `./run_quality_check.sh --help` | 显示所有选项 |

## 常见工作流

1. 编写代码
2. 使用 `git add myfile.py` 暂存文件
3. 运行 `./run_quality_check.sh` 检查问题
4. 修复发现的所有问题
5. 使用 `git commit -m "您的提交信息"` 提交代码

## 示例输出

```
🔍 Running Code Quality Gate...

🚫 ERRORS:
  - Security: ./src/demo.py:24 - subprocess call with shell=True identified, security issue.

⚠️ WARNINGS:
  - Complexity: src/demo.js:21: complexFunction has 34 lines, 7 CCN, 6 parameters
  - Security: ./src/demo.py:17 - Use of insecure function eval()

❌ Quality gate failed! Please fix the errors above.
```

## 如何修复常见问题

### 安全问题

- **subprocess 使用 shell=True**：
  ```python
  # Bad
  subprocess.call("command", shell=True)

  # Good
  subprocess.call(["command", "arg1", "arg2"])
  ```

- **eval() 的使用**：
  ```python
  # Bad
  result = eval(user_input)

  # Good
  import ast
  result = ast.literal_eval(user_input)
  ```

### 复杂度问题

- 将大型函数拆分成更小的函数
- 通过使用提前返回来减少嵌套层级
- 将复杂逻辑提取到辅助函数中

## 获取帮助

更多详细信息：
- 阅读完整的 [README.md](../README.md)
- 查看 [SETUP.md](../SETUP.md) 了解配置选项
- 访问[故障排除](./troubleshooting.md)指南

祝您编码愉快！
