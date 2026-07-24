# Pre-Commitator：实现概述

## 架构

Pre-Commitator 由以下组件构成：

1. **质量门 Python 脚本**（`src/quality_gate.py`）：
   - 运行多种质量检查的核心验证引擎
   - 支持 Python、JavaScript 和其他语言检查
   - 提供清晰、格式化的错误信息
   - 设计为既可由 pre-commit 钩子运行，也可手动运行
   - 支持通过环境变量有选择地禁用验证器

2. **Pre-Commit 钩子集成**（`src/pre_commit_hook.sh`）：
   - 在 git 提交期间自动运行质量门
   - 如果发现质量问题则阻止提交
   - 提供彩色、清晰的输出
   - 从 .env 文件加载环境变量

3. **命令行界面**（`run_quality_check.sh`）：
   - 用于手动运行检查的用户友好型 CLI
   - 可检查暂存文件、特定文件或所有文件
   - 使提交前验证代码变得简单

4. **安装脚本**（`install.sh`）：
   - 安装所有必要的依赖
   - 设置 pre-commit 钩子
   - 使脚本可执行
   - 根据用户偏好配置 VS Code 或终端模式

5. **模式切换器**（`switch_mode.sh`）：
   - 在兼容 VS Code 的模式和终端优化模式之间切换
   - 创建适当的 .env 文件以在 VS Code 模式下禁用验证器
   - 使用预定义的配置模板进行一致的设置
   - 尝试安装 Horusec（可选安全扫描器）

6. **自动暂存钩子**（`src/auto_stage_hook.sh`）：
   - 自动暂存由 pre-commit 钩子修改的文件
   - 消除钩子修改后手动暂存文件的需要
   - 提供无缝的提交体验

7. **SSL 证书修复**（`fix_certificates.sh` 和 `fix_certificates_unix.sh`）：
   - 修复 macOS 上 Python 的 SSL 证书问题
   - 提供替代的证书安装方法
   - 通过 ssl_context_fix.py 创建临时 SSL 上下文修复
   - 设置 PYTHONPATH 环境变量以启用修复
   - 防止 "[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed" 错误
   - 对于需要网络访问的 pre-commit 钩子至关重要

8. **Horusec 安装**（`src/install_horusec.sh`）：
   - 根据平台安装 Horusec 安全扫描器
   - 优雅地处理安装失败
   - 提供特定于平台的安装方法

## 实现细节

### 质量验证

1. **文件卫生**：
   - 行尾空格
   - 文件末尾换行符
   - YAML 有效性
   - 文件大小限制

2. **代码质量**：
   - 复杂度指标（Lizard）
   - 代码格式化（Python 的 Black）
   - 静态分析

3. **安全性**：
   - Bandit 用于 Python 安全扫描
   - ESLint 安全插件用于 JavaScript/TypeScript
   - Horusec 多语言扫描器
   - Semgrep 用于基于模式的安全扫描
   - 依赖漏洞检查（pip-audit）

### 功能特性

1. **语言支持**：
   - 完全支持 Python、JavaScript 和 TypeScript
   - 架构支持扩展到 Java、Go、Ruby、PHP 等其他语言
   - 每个验证器仅在相关文件类型上运行

2. **性能**：
   - 仅检查正在提交的文件
   - 为开发者工作流快速执行

3. **清晰的错误报告**：
   - 错误信息设计为对人类和 AI 都易于理解
   - 将错误与警告分类
   - 显示文件、行号和问题描述

4. **可扩展性**：
   - 易于添加新的验证工具
   - pre-commit 配置允许自定义

5. **特定环境模式**：
   - VS Code 模式用于 IDE 集成
   - 终端模式用于完整验证能力
   - 环境变量控制启用的验证器

## 运行模式

### VS Code 模式

VS Code 模式禁用了在使用 VS Code 集成源代码控制时可能导致问题的某些验证器：

1. **实现**：
   - 使用 `.env` 文件设置环境变量
   - 通过环境变量禁用有问题的验证器
   - pre-commit 钩子在运行时加载这些变量

2. **禁用的验证器**：
   - ESLint（由于从 pre-commit 安装时的 SSL 证书问题）
   - Lizard（防止"命令未找到"错误）
   - Bandit（防止"命令未找到"错误和 SSL 证书问题）
   - Semgrep（防止"命令未找到"错误）

3. **配置**：
   - 使用 `config/pre-commit-vscode.yaml` 模板

### 终端模式

终端模式启用所有验证器以进行全面的代码质量检查：

1. **实现**：
   - 如果存在 `.env` 文件则删除
   - 默认启用所有验证器
   - 确保完整的验证能力

2. **启用的验证器**：
   - ESLint 用于 JavaScript 验证
   - Lizard 用于复杂度分析
   - Bandit 用于 Python 安全
   - Semgrep 用于安全扫描
   - Horusec 用于额外的安全扫描

3. **配置**：
   - 使用 `config/pre-commit-terminal.yaml` 模板

## 与 AI 配合使用

该工具设计为与 AI 生成的代码配合良好：

1. 当 AI 生成代码时，在使用之前运行质量检查
2. 错误信息经过结构化，易于被 AI 解析
3. AI 可以根据错误信息理解并修复问题

## 自动暂存实现

自动暂存功能的工作原理如下：

1. 将原始 pre-commit 钩子备份为 `pre-commit.original`
2. 创建新的 pre-commit 钩子，该钩子：
   - 运行原始 pre-commit 钩子
   - 检测 pre-commit 钩子修改文件的时机
   - 自动暂存这些修改的文件
   - 保留原始钩子的退出代码
3. 在 VS Code 和终端模式下都提供无缝体验
4. 指示用户再次提交以完成流程

该功能通过消除手动暂存由 pre-commit 钩子自动修改的文件（如空格或格式修复）这一手动步骤，显著改善了开发工作流。实现设计为对 pre-commit 重新安装具有弹性，通过维护原始钩子的备份来实现。

## 未来改进

1. **更多语言支持**：
   - 添加 Rust、C/C++ 专用验证器

2. **CI/CD 集成**：
   - GitHub Actions 工作流
   - GitLab CI 流水线

3. **报告仪表板**：
   - 代码质量随时间的趋势分析
   - 问题的可视化展示
