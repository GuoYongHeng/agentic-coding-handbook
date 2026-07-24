# 配置文件

本目录包含 Pre-Commitator 用于自定义其行为的配置文件。

## 可用配置文件

### settings.yaml

Pre-Commitator 的通用设置：

```yaml
database:
  host: localhost
  port: 5432
  name: test_db

logging:
  level: INFO
  format: '%(asctime)s - %(levelname)s - %(message)s'
```

## 其他配置文件

以下文件位于根目录：

### .pre-commit-config.yaml

配置执行哪些 pre-commit 钩子及其行为方式。这是自定义运行哪些检查的主配置文件。

### .bandit.yml

Bandit 安全扫描器的配置：

```yaml
# Skip these tests
skips:
  - B101 # assert used
  - B404 # subprocess without shell=True
```

### .eslintrc.json

ESLint JavaScript 代码检查工具的配置：

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:security/recommended"
  ],
  "plugins": [
    "security"
  ]
}
```

## 自定义配置

要自定义 Pre-Commitator 的行为：

1. 编辑相应的配置文件
2. 使用 `./run_quality_check.sh --all` 测试更改
3. 将更改提交到版本控制

更多详细信息，请参阅 [SETUP.md](../SETUP.md) 文件。
