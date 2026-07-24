---
layout: default
title: 项目指南
parent: 工具与配置
nav_order: 6
---

# 项目级指令

提升 AI 生成代码质量最有效的方法之一，是配置项目级指令来引导 GitHub Copilot 和其他编程智能体的行为。这些指令就像一个持久的记忆层——它们被自动注入到 LLM 的上下文中，帮助其遵循项目的特定约定和模式，而无需在每次提示词中重新解释。Copilot 指令可用于保持智能体按你所期望的方式工作。它既约束智能体将要执行的操作，也强制执行代码质量要求。

## 为什么这很重要

在实验过程中，我们发现提前将质量期望和设计模式编码化，会使 AI 更有可能生成正确的、可直接进入审查流程的代码。这也减少了提示词重复的需求，提升了开发者之间的一致性，并加速了新成员的上手速度。

这一概念与 Cursor 的 `.cursor/rules` 直接对应，后者是我们实验中的关键推动因素。好消息是：GitHub Copilot 现在也通过自定义指令支持这一功能。

## 项目指令应包含的内容

在你的仓库根目录创建一个名为 `.github/copilot-instructions.md` 的文件。在这个 Markdown 文件中，记录所有你希望 AI 视为"不可谈判"的团队工作方式，例如：

- 架构决策（如：本项目使用分层架构，包含独立的领域层和基础设施层文件夹。）
- 命名规范和文件结构指南
- 安全模式（如：所有用户输入在写入数据库前必须经过清洗。）
- 代码质量期望（如：使用 `??` 替代 `||` 进行空值检查。）
- 测试规则（如：每个服务必须包含使用 Jest 和 describe.each 进行参数化覆盖的单元测试。）
- 需要避免的事项（如：永远不要使用 any 作为类型。）

> 这些指令在你于项目中进行提示时，会被 Copilot Chat 自动加载，无需在聊天中显式引用。

## 如何编写优秀的项目指令文件

`.github/copilot-instructions.md`（或 `.cursor/rules`）文件帮助 AI 生成符合团队标准的代码。把它想象成为一位会严格遵守规则但不会主动提问的初级开发者编写的入职说明。

遵循以下指南：

### 清晰且具有指导性

- 使用指令式语言。
  ✅ `Use Axios for all HTTP requests`
  ❌ `We usually prefer Axios.`

### 具体明确

- 避免模糊的建议。
  ✅ `Put all utility functions in src/utils using camelCase.`
  ❌ `Keep things organized.`

### 优先考虑 AI 所需的信息

专注于结构、命名、首选库、测试和模式。跳过深层业务背景——这不会帮助代码生成，应作为实现规划和执行的一部分来分享。

### 使用标题对规则分组

按主题组织内容，例如：

- 项目结构
- 命名规范
- 错误处理
- 测试
- 需要避免的事项

**保持简短：** 控制在 1-2 页以内。如果太长，可能因上下文限制而被部分忽略。

**像维护代码一样维护它：** 随着项目模式的演进进行更新——尤其是在新成员入职或重大功能开发之前。

## 资源与示例

- 你可以在 cursor.directory 找到很多规则示例
- Cursor 的 `.cursor/rules` 和 GitHub 的 `.github/copilot-instructions.md` 采用相同的格式——Markdown——可以跨工具复用，几乎不需要调整。

- [Clean Code Rules Prompt](https://shumerprompt.com/prompts/clean-code-rules-prompt-554351c6-3bcb-4c20-9c77-f831b4aa6b0a)
- [Code Quality Guidelines Prompt](https://shumerprompt.com/prompts/-code-quality-guidelines-prompt-661c6a3f-cb69-46e6-b75c-97f7bfbb514b)
- [React Rules Prompt](https://shumerprompt.com/prompts/react-rules-prompt-76302cd0-5448-4056-a90e-4057388a9149)
- [Python Best Practices Prompt](https://shumerprompt.com/prompts/python-best-practices-prompt-ac25d837-ff42-4b89-92b1-5a7bbb558047)

```markdown
# COPILOT INSTRUCTIONS OPERATIONAL GUIDELINES

## PRIME DIRECTIVE

Avoid working on more than one file at a time.
Multiple simultaneous edits to a file will cause corruption.
Be chatty and teach about what you are doing while coding.

## LARGE FILE & COMPLEX CHANGE PROTOCOL

### MANDATORY PLANNING PHASE

When working with large files (>300 lines) or complex changes:

1. ALWAYS start by creating a detailed plan BEFORE making any edits
2. Your plan MUST include:

- All functions/sections that need modification
- The order in which changes should be applied
- Dependencies between changes
- Estimated number of separate edits required

3. Format your plan as:

## PROPOSED EDIT PLAN

Working with: [filename]
Total planned edits: [number]

### MAKING EDITS

- Focus on one conceptual change at a time
- Show clear "before" and "after" snippets when proposing changes
- Include concise explanations of what changed and why
- Always check if the edit maintains the project's coding style

### Edit sequence:

1. [First specific change] - Purpose: [why]
2. [Second specific change] - Purpose: [why]
3. Do you approve this plan? I'll proceed with Edit [number] after your confirmation.
4. WAIT for explicit user confirmation before making ANY edits when user ok edit [number]

### EXECUTION PHASE:

- After each individual edit, clearly indicate progress:
  "✅ Completed edit [#] of [total]. Ready for next edit?"
- If you discover additional needed changes during editing:
- STOP and update the plan
- Get approval before continuing

### REFACTORING GUIDANCE:

When refactoring large files:

- Break work into logical, independently functional chunks
- Ensure each intermediate state maintains functionality
- Consider temporary duplication as a valid interim step
- Always indicate the refactoring pattern being applied

### RATE LIMIT AVOIDANCE:

- For very large files, suggest splitting changes across multiple sessions
- Prioritize changes that are logically complete units
- Always provide clear stopping points

## General Requirements:

Use modern technologies as described below for all code suggestions. Prioritize clean, maintainable code with appropriate comments.
```

## 继续阅读

[示例](./examples.md)
