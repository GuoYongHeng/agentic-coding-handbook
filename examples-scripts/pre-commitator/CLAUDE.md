# CLAUDE.md

本文件为 Claude Code（claude.ai/code）在此仓库中工作时提供指引。

## 命令

- 安装：`pip install pre-commit && pre-commit install`
- 运行 pre-commit：`pre-commit run`（暂存文件）或 `pre-commit run --all-files`
- 运行测试：`./run_tests.sh`（全部）或 `./run_quality_check.sh tests/test_file.py`（单个文件）
- 质量检查：`./run_quality_check.sh`（暂存文件）或 `./run_quality_check.sh --all`（所有文件）
- Python 代码检查：`black src/`（自动格式化 Python 代码）
- JS/TS 代码检查：`npx eslint src/**/*.{js,ts,tsx}` 或 `npm run lint`（全部）或 `npm run lint:ts`（仅 TS）
- TypeScript 检查：`./run_ts_check.sh`（单个文件）或 `npx tsc --noEmit` 或 `npm run typecheck`（全部）
- 安全扫描：`bandit -r src/ --configfile=.bandit.yml`
- 代码指标：`lizard -l 5 .`（衡量复杂度）
- NPM 脚本：`npm run check`（暂存文件）或 `npm run check:all`（所有文件）

## 代码风格规范

- **Python**：
  - 遵循 Black 格式化规范（行长度 88）
  - 必须使用类型注解
  - 复杂度限制：CCN<10，函数长度<100，参数数量<5
  - 遵循 PEP 8 风格指南，使用 isort 整理导入

- **JavaScript/TypeScript**：
  - 遵循 .eslintrc.json 中的安全规则
  - 使用安全插件和 TypeScript 专用规则
  - 字符串使用单引号，必须加分号
  - 2 个空格缩进
  - 启用严格空值检查和 noImplicitAny

- **通用**：
  - 必须通过 YAML 验证
  - 不允许行尾空格
  - 文件必须以换行符结尾
  - 运行安全检查（Bandit、Horusec、Semgrep）
  - 避免硬编码凭证
  - 错误信息必须对人类和 AI 都清晰易懂
