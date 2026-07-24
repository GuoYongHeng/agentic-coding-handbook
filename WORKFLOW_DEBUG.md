---
layout: default
title: 调试工作流
parent: 核心工作流程
nav_order: 6
---

# 智能体编程调试工作流

智能体编程工作流中的调试不是盲目修复错误，而是构建一个反馈循环，让 AI 系统性地帮助识别、解释和解决问题。

本页概述了使用 Cursor、Claude 和模型上下文提供者（MCP）等工具高效进行 AI 调试的核心技术，同时保持速度、质量和信心。

## 智能体调试的核心原则

- AI 能够思考，但它需要上下文——错误信息、日志、截图和预期结果。
- 调试不是猜谜游戏——而是对可能原因进行有条理的缩小范围。
- 先推理再修复——最好的 bug 修复来自深思熟虑的分析之后。
- 调试也是关于预防的——添加日志、测试和检查点有助于在 bug 扩大之前发现它们。

## 智能体调试工作流

### 让 AI 在自己的修复和测试循环中迭代

使用智能体模式从一个低风险的提示词开始：

```txt
The login form isn't submitting. Iterate through the loop of attempting to submit, understanding the issue and fixing.
```

当 bug 简单或可以安全地直接尝试修复时使用它。Cursor 特别擅长通过终端感知智能体以这种方式调试 React、Next.js 或后端项目。

### 提示 AI 先推理再行动

在允许代码更改之前使用推理优先的提示词：

```txt
List 5–7 possible causes for this issue and propose diagnostics for each. Don't write code yet.
```

这激活了思维链逻辑，防止 AI 过早跳到修复方案。

### 添加日志帮助 AI 调试

在解决问题之前，让 AI 注入日志或调试语句：

```txt
Add logs to print input payload, validation output, and DB query results in this flow.
```

运行应用后，将输出复制回对话，或使用 Copilot 终端或浏览器集成附加日志。

### 从正确的上下文中提供日志和错误

使用 MCP 提供运行时反馈：

- 浏览器控制台 → 用于 UI bug 和脚本失败
- 终端日志 → 用于后端和测试运行
- 云日志 → 来自 CloudWatch、Kibana、Grafana Loki

示例提示词：

```txt
Use MCP to fetch the last logs from CloudWatch and identify the cause of the 502 error.
```

当 MCP 不可用时直接粘贴日志：

```txt
This error occurred after login: TypeError: Cannot read properties of null — fix based on this trace.
```

### 通过 GitHub MCP 调查代码变更

有时 bug 是由最近的提交引起的。

使用：

```txt
Fetch the latest GitHub PRs that changed the auth.ts file. Summarize the changes and identify what could have broken session persistence.
```

这为 AI 提供了将变更与症状联系起来的关键历史上下文。

### 保持调试状态清洁

- 当上下文变得嘈杂时，开启新的对话或智能体会话
- 使用版本控制或 Cursor 的"回退到检查点"功能回退损坏的分支
- 提示词：`Reset the state to the last working version before commit a1b2c3.`

### 两阶段调试循环

当 bug 棘手时使用此方法：

- 让 AI 假设并解释。
- 只有当计划对你有意义时才实施。
- 优先使用 TDD 执行修复，在每次更改时编写新测试并运行现有测试。
- 逐步验证——不要让 AI 在多个文件中失控。

提示词：

```txt
Explain what's broken and how to fix it, but do not modify any files yet.
```

### 预防性调试实践

- 提交前始终在本地运行。确认 bug 是可复现的。
- 使用 `spec.md` 和提示词计划清晰描述预期行为。
- 在重要里程碑后创建检查点提交。
- 在重构前添加单元测试和行为测试以记录预期行为。

## 总结：正确地使用 AI 调试

| **实践**                | **优势**                    |
| --------------------------- | ------------------------------ |
| YOLO 模式                   | 快速修复明显的 bug    |
| 思维链提示词    | 防止盲目猜测          |
| 先记录日志再修复       | 更容易诊断               |
| 用 MCP 获取日志和错误      | 真实的生产环境上下文        |
| 用 GitHub MCP 追踪代码变更 | 追踪回归             |
| 两阶段调试         | 减少返工和范围蔓延 |
| 检查点与重置        | 保持开发环境清洁    |

调试是大多数工程师浪费时间的地方。通过结构化提示词、上下文注入和深思熟虑的推理，智能体调试将成为一种超能力——而不是一种挣扎。

## 参考资料

- [Using Copilot to Debug the Front End](https://www.loom.com/share/50de880c8ce5466d9d21c56e9d00bc30?sid=0c35aea6-596f-46e5-a9e3-3e6d6867b6fc)

## 继续阅读

[记忆库工作流](./WORKFLOW_MEMORY_BANK.md)
