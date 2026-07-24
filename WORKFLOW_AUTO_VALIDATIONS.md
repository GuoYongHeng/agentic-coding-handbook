---
layout: default
title: 自动验证
parent: 核心工作流程
nav_order: 7
---

# 自动代码验证

AI 编程智能体的一项强大能力是运行代码验证工具、分析反馈并自我修正代码，直到满足所有定义的质量标准。这将你的提示词循环变成更智能、更可靠的工作流，AI 不仅编写代码，还从验证输出中学习并自动修复问题。

## 为何重要

在我们的实验中，这种方法大幅减少了返工，提升了代码质量，并使 AI 成为更有用的编码助手。智能体能够实时捕获并修复隐藏的 lint 错误或高复杂度问题，而无需等到人工审查。

## 工作原理

- 开发者提示 AI 实现某个函数或功能。
- AI 编写代码并运行验证脚本（lint 检查器、格式化工具、测试套件等）。
- 如果验证失败，AI 使用终端输出的反馈作为新的上下文并进行迭代。
- 一旦所有验证通过，AI 可以继续提交代码。

## 示例：使用 Lizard 检测认知复杂度

我们使用预配置的脚本运行 [Lizard](https://github.com/terryyin/lizard) 并强制执行认知复杂度上限 10。

- AI 编写一个新函数。
- 运行 Lizard 脚本。
- 脚本返回：`Function X has cognitive complexity of 15`
- AI 获取这个反馈并重写函数，使复杂度降到限制以下。

这个循环可以应用于许多验证工具。

## 循环中常用的验证工具

- Lint 检查器（如 ESLint）
- 格式化工具（如 Prettier）
- 单元测试（如 Jest、Vitest）
- 代码复杂度分析器（如 Lizard、SonarQube）
- 静态分析工具（如 TypeScript 编译器、Horusec、Bandit）

## 与预提交/预推送钩子的 Git 集成

这个自我修正循环也可以扩展到 Git 命令。例如：

- 你要求 AI：`Stage and commit all changes that pass our validations.`
- AI：
  - 运行预提交钩子
  - 捕获输出
  - 修复出现的任何问题
  - 重复直到验证通过
  - 然后提交更改

这确保了没有无效代码被提交，保持仓库的整洁并符合团队规范。

## 参考资料

- [Enhancing Code Quality with AI](https://www.loom.com/share/32bd23d355d9438587d55d7a87b58ed1)

## 继续阅读

[探索与重构工作流](./WORKFLOW_EXPLORATORY.md)
