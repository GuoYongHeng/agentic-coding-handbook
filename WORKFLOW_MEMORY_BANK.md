---
layout: default
title: 记忆库系统
parent: 核心工作流程
nav_order: 3
---

# 记忆库

Copilot 和 Cursor 等 AI 模型没有持久记忆。关闭标签页后，它们会忘记一切。这就是记忆库的用武之地。记忆库是一个基于 Markdown 的结构化文档系统，充当 AI 智能体的长期记忆。它让助手能够跨会话"记住"你的项目上下文、决策和进度。

## 为何重要

在使用 AI 编程智能体时，连续性至关重要。没有记忆，你会浪费时间反复解释项目是什么、做了什么以及要遵循哪些模式。记忆库通过将记忆外化到一组智能体可以读取、引用和更新的文件中来解决这个问题。

## 典型的记忆库文件

- `projectbrief.md`：整体范围和目标
- `productContext.md`：用户体验、用户和待解决的问题
- `systemPatterns.md:`架构、设计模式和决策
- `techContext.md`：技术栈、依赖关系和约束条件
- `activeContext.md`：当前任务、上下文和工作笔记
- `progress.md`：已完成和待完成内容的状态日志

这些文件存放在 `/memory-bank/` 或 `.github/copilot-instructions.md` 等文件夹中，在每次会话开始时读取。可以通过自定义指令引导 Cursor 或 Copilot 智能体始终加载和更新这些文件。

## 实际工作原理

- 你和智能体在推进过程中更新记忆。
- 你可以在取得进展时要求智能体"更新记忆"。
- 这让 AI 保持专注，减少幻觉或返工。
- 借助 MCP 服务器（如 @alioshr/memory-bank-mcp），你甚至可以远程管理多个项目的记忆库。

优势

- **持久性：** 记忆在会话之间得以保留。
- **更少错误：** 过去的问题和决策会被记住。
- **更快上手：** 任何人（AI 或人类）都能快速了解项目背景。
- **更好的协作：** 记忆库成为你的唯一真实来源。

这就是我们将无状态 LLM 转变为项目感知伙伴的方式。你编写代码——AI 更有效地提供帮助。

## 记忆库中应该包含什么

- **架构决策和设计模式：** 如 `We use hexagonal architecture in the backend. Controllers should not contain business logic.`
- **项目约束和技术栈特定规则：** 如 `Use TanStack Query for all data fetching. Avoid raw fetch or Axios.`
- **当前重点和开放的技术挑战：** 如 `Currently refactoring authentication flow. Login and refresh token logic is under review.`

## 记忆库中不应该包含什么

- **敏感凭证或密钥：** 永远不要包含令牌、密码、数据库 URL 或访问密钥——这些对 AI 没有帮助，还会带来安全风险。
- **详细的代码片段或实现块：** 避免粘贴大型函数或完整类——使用摘要或引用文件名代替。
- **未经过滤的聊天记录或原始会议笔记：** 保持内容结构化、相关且有目的性——记忆是为上下文服务的，而不是杂乱信息的堆积。

## 使用 GitHub Copilot 设置记忆库

GitHub Copilot 在会话之间没有内置记忆，但你可以通过使用充当持久记忆库的项目级指令文件来模拟这一功能。这些文件在你在项目内提示时会自动加载到模型上下文中，让 AI 能够"记住"决策、架构、约定和当前任务。

## 分步设置

- 创建 `.github/copilot-instructions.md` 文件：这是 GitHub Copilot 智能体将读取项目级上下文的文件。它应该位于项目根目录下的 `.github/` 文件夹中。
- 定义结构：我们推荐使用实验中的记忆库模式来构建文件：

```txt
# projectbrief.md
Describes the overall goals of the project, what we're building, and for whom.

# productContext.md
Why this project exists, the problems it solves, user experience goals, and business context.

# systemPatterns.md
Document architecture choices, common design patterns, and preferred technical structures.

# techContext.md
Describe technologies used, external APIs, dev setup, tooling decisions, and known constraints.

# activeContext.md
Current task, recent changes, open issues, known challenges, and next steps.

# progress.md
Track completed features, blockers, and evolving decisions over time.
```

你可以将所有内容嵌入 `.github/copilot-instructions.md`，或将它们分割到 `/memory-bank/` 文件夹下的文件中，然后在 Copilot 指令文件中引用该文件夹。

- **保持 Markdown 格式：** Copilot 和 Cursor 都期望 Markdown 格式。这确保内容对人类和模型都可读。

- **保持简短、结构化和最新：** 虽然 Copilot 可以读取多达 128K token（取决于模型可能达到 100 万 token），但最好保持每个文件简洁。专注于 AI 在代码生成或审查过程中可以使用的高价值细节。

- **定期更新：** 每当发生重大变化时——新架构、重构或 bug 修复——更新 `activeContext.md` 和 `progress.md`。你也可以提示 AI：`Summarize the last 3 pull requests and update activeContext.md`。

- **可选：使用文件夹代替单个文件：** 如果你的项目规模较大或想要模块化记忆，可以将记忆库文件放在名为 `/memory-bank/` 的文件夹中，然后将摘要或关键部分复制到 `.github/copilot-instructions.md`。这让人类和 AI 可以引用相同内容，而不会使单个文件过载。

## 指令示例（用于 `.github/copilot-instructions.md`）

```markdown
# Copilot Memory Bank

This project uses React + NestJS. All backend code should follow a hexagonal architecture. Frontend code should use TanStack Query and controlled inputs.

Use JWT for authentication and handle all errors using a standard ErrorHandler service.

Refer to the following memory files for deeper project understanding:

- `memory-bank/projectbrief.md`
- `memory-bank/systemPatterns.md`
- `memory-bank/techContext.md`
- `memory-bank/activeContext.md`
```

## 参考资料

- [How to Use a Memory Bank in Copilot](https://www.loom.com/share/152cea77575148b8af9fe8538ed30c30?sid=e3dd85c5-60e4-4d54-973c-4d4a3ff89917)
- [10x your Cursor Workflow with Memory Bank](https://www.youtube.com/watch?si=EiHdLnUQMBanl_eO&v=Uufa6flWid4&feature=youtu.be&themeRefresh=1)

## 继续阅读

[模型上下文提供者（MCP）](./MCPS.md)
