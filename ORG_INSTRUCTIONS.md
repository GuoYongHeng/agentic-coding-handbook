---
layout: default
title: 组织指南
parent: 工具与配置
nav_order: 5
---

# 组织级指令

GitHub Copilot 现已支持组织级指令，允许管理员定义适用于 GitHub 组织内所有仓库和所有用户的指导方针。这是在所有 AI 辅助开发工作流中促进一致性、安全性和治理的强大工具。

本页说明组织级指令与项目级 `.copilot-instructions.md` 文件的区别，以及如何有效使用两者。

## 什么是组织级指令？

组织级指令在 GitHub 设置中（通过 Copilot 标签页）进行配置，并自动注入到组织成员的每次 Copilot Chat 交互中——无论他们正在处理哪个仓库。

这为组织伞下的所有团队建立了统一的 AI 行为基准模型。

## 与项目级指令的区别

| **功能**    | **组织级**                | **项目级**                                             |
| -------------- | ------------------------------------- | ------------------------------------------------------------- |
| 作用范围          | 组织内所有用户和仓库          | 仅适用于特定仓库                               |
| 管理者 | 组织管理员                            | 仓库维护者或开发者                                |
| 加载时机  | 始终在上下文中                     | 仅在处理该仓库时加载                                |
| 用途 | 公司级指导、政策、风格规范 | 仓库特定的架构、技术栈、命名规范等               |
| 格式 | GitHub 设置中的文本框           | 仓库根目录的 Markdown 文件：`.github/copilot-instructions.md` |

这两种类型的指令互为补充：组织级定义全局规则，项目级增加本地特定内容。

## 组织级指令的使用场景

- **安全和隐私规则**

```txt
Never suggest using secrets or API keys directly in code.
Avoid using eval() or direct SQL string construction.
```

- **文档和学习资源**

```txt
When asked about frontend theming, refer to the Confluence Docs at <name>.
Link to internal API documentation for auth-related questions.
```

- **跨团队一致性**

```txt
Use PascalCase for class names and camelCase for functions.
Always wrap DB calls with the internal SafeQuery abstraction.
```

- **代码风格和格式规范**

```txt
All logs must use LoggerService.debug() — never console.log().
Use ?? over || for nullish checks.
```

- **流程指引**

```txt
For any deployment questions, remind the user to check the InfraRunbook first.
When unsure about security decisions, suggest reaching out in #ask-security.
```

## 编写组织级指令的最佳实践

- **清晰且具有指导性：** 像撰写入职规范一样编写——而非建议。
- **避免仓库特定的逻辑：** 不要引用仓库文件路径或本地变量。
- **使用结构化分类：** 按"安全"、"日志"、"命名规范"等主题分节。
- **保持简短：** 目标控制在 1000 字以内，避免触达上下文压缩限制。
- **定期更新：** 在团队全面推广、语言风格更新或新安全策略出台时同步修改。

## 何时使用项目级 vs 组织级指令

| **场景**                                | **使用方式**       |
| ------------------------------------------- | ------------------ |
| 为 monorepo 定义架构规则  | 项目级      |
| 统一所有项目的日志规范   | 组织级 |
| 按团队强制执行命名规范       | 项目级      |
| 控制 AI 对密钥使用的建议 | 组织级 |
| 传授仓库特定的设计模式     | 项目级      |

## 配置方法

- 进入你的 GitHub 组织设置。
- 导航到 Copilot 标签页。
- 点击"自定义指令"。
- 在编辑框中输入你的组织级指令。
- 保存——这些指令即刻对组织内所有 Copilot Chat 交互生效。

组织指令是你的第一层 LLM 治理机制。它们建立了 AI 应体现的共同价值观和行为规范——让 Copilot 成为工程文化的延伸。

## 参考资料

- [Organization custom instructions now available](https://github.blog/changelog/2025-04-17-organization-custom-instructions-now-available/)

## 继续阅读

[项目级指令](./PRJ_INSTRUCTIONS.md)
