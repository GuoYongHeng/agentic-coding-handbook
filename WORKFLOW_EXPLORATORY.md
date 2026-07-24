---
layout: default
title: Exploratory Workflow
parent: Core Workflows
nav_order: 5
---

# 探索与重构工作流

> **_探索 → 规划 → 编码 → 提交_**

Claude、Cursor 和 GitHub Copilot 等 AI 工具不仅仅是代码生成器，更是推理引擎。有意识地使用时，它们能帮助工程师剖析复杂问题、绘制系统地图并实现安全、可扩展的解决方案。该工作流专注于在编码开始前使用 AI 进行探索和策略制定。

## 探索——理解问题与代码库

在进入实现之前，提示 AI 像资深工程师一样探索系统。让它：

- 总结模块或流程的工作方式：`How does the authentication middleware interact with the session manager?`
- 追踪依赖关系或调用层次：`Which services rely on PaymentService?`
- 审查相关文件但不编写任何代码：`Read the files related to logging, but do not write any code yet. Just summarize what they do.`
- 可视化架构：`Generate a component diagram showing the flow from createInvoice() to downstream services.`

这在规划解决方案之前建立上下文并暴露未知因素。

## 规划——推理解决方案

理解问题后，让 AI 制定计划：

- 分解问题：`What steps are required to decouple the billing module from user management?`
- 先思考再行动：使用 `Think hard before answering` 或 `Ultrathink mode: what are the tradeoffs of each solution path?` 等提示词
- 识别风险或影响：`If we refactor NotificationService, what might break downstream?`
- 生成分步实现路线图：`Write a plan to migrate this legacy feature without regressions.`

鼓励 AI 在适当时验证假设并提出替代解决方案。

## 编码——安全而迭代地构建

有了计划后，开始编码：

- 提示 AI 按计划逐步编写代码。`Implement step 1 of the plan: extract logging into a standalone module.`
- 使用安全网实践：
  - 在每次更改前后编写或运行单元测试
  - 使用 AI 为边界行为生成测试用例
  - 以小块重构，在每一步进行验证
  - 寻求帮助保持范围清晰：`Refactor this method but keep all existing tests green.`

## 提交——完成、记录并分享

解决方案完成后：

- 让 AI 总结更改：
  "Generate a changelog summary and commit message based on the last 3 modified files."
- 自动更新文档：
  "Update the README and Swagger docs to reflect changes to GET /users."
- 推送并开启 PR，可选择使用 Claude/Cursor 命令如 /commit、/pr 或 GitHub CLI 集成。

## 探索工作流的优势

- 在编写代码前减少盲点
- 改善推理、规划和系统理解
- 支持更安全、模块化的实现
- 提升对陌生代码库的熟悉速度
- 在不阻碍创造力的前提下培养纪律性

## 参考资料

- [Refactoring Code with AI Assistance](https://www.loom.com/share/bc30c068b8c54038aaa02697ea69a9bd?sid=9ba2d4db-239a-4017-838d-c3195e67fc38)

## 继续阅读

[视觉反馈工作流](./WORKFLOW_VISUAL_FEEDBACK.md)
