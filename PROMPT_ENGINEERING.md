---
layout: default
title: 基础知识
parent: 提示词工程
nav_order: 1
---

# 提示词工程基础

提示词工程是指编写清晰、结构化指令的实践，用于引导 GitHub Copilot Agent 或 Cursor 等 AI 编程智能体。就像向初级开发者解释任务一样，你的职责是以 AI 能够理解并付诸行动的方式，描述你想要什么、在哪里做，以及如何做。

在我们的内部实验中，我们发现提示词质量是最关键的成功因素之一。好的提示词能产出干净、可扩展的代码，而模糊或过于宽泛的提示词则会导致幻觉、引入 bug 并浪费时间。

## 为什么这很重要

提示词是你向 AI 输入任务级上下文的主要方式。由于模型无法猜测你的想法，它完全依赖你说了什么——以及你是如何说的。

精心设计的提示词能够：

- 提升准确性和一致性
- 减少幻觉
- 让 AI 生成的代码更易于验证和维护
- 节省审查和返工时间

## 提示词的核心技巧

- 以清晰的操作、预期输出和重要约束条件开始提示词。
- 尽可能指定应进行更改的确切文件、服务或组件。
- 尽量将大型功能拆解为小而独立的提示词。
- 使用"逐步"或"像专家一样思考"的指令来引导更深入的推理。
- 尽可能附上代码片段、文件名、终端输出或 #codebase 上下文。
- 将每次提示词视为交给初级开发者的任务：详细但聚焦，且有清晰的结果期望。
- 如果 AI 给出了糟糕的输出，优化原始提示词而不是手动修复错误输出。
- 手动验证每个结果——绝不要在没有审查和测试的情况下信任 AI 输出。
- 在可能的情况下，优先描述功能背后的"原因"——这能改善 AI 的架构决策。
- 记住，模糊的提示词浪费的时间，远比多花几秒钟写一个更好的提示词更多。

## 提示词基础（含实践指导）

在继续阅读之前，请务必阅读以下页面：

- [VSCode 的 Copilot Chat 提示词工程指南](https://code.visualstudio.com/docs/copilot/chat/prompt-crafting)
- [Github 的 Copilot Chat 提示词工程指南](https://docs.github.com/en/copilot/using-github-copilot/copilot-chat/prompt-engineering-for-copilot-chat)
- [Anthropic 提示词库](https://docs.anthropic.com/en/resources/prompt-library/library)
- [Craft Perfect AI Prompts](https://shumerprompt.com/)

## 示例

### 清晰且具体

应该这样做：

```txt
Create a POST /users/login endpoint using NestJS. It should accept email and password, validate input, and return a JWT if credentials are correct. Use class-validator and JWT module.
```

不要这样做：

```txt
Add login functionality.
```

### 限定范围：一次一个任务

应该这样做：

```txt
Add email format validation to the user registration form in RegisterForm.tsx.
```

不要这样做（范围过于宽泛，可能产生不完整或分散的结果）：

```txt
Finish all validations for the signup flow.
```

### 添加上下文：包含文件名、项目结构和相关实现细节

应该这样做：

```txt
In auth.controller.ts, add a new endpoint that consumes authService.validateUser() and returns a JWT if valid.
Also, attach files or use the #codebase tag to help Copilot Agent or Cursor read project content.
```

### 描述预期行为和约束条件

应该这样做：

```txt
Add unit tests for parseMedicalReport() in report.utils.ts. Cover edge cases like empty file, invalid format, and corrupted content.
```

不要这样做：

```txt
Write tests for report parser.
```

### 使用"推理优先"的提示词格式

这些方法能改善架构决策，减少低质量响应。

#### 三专家法

```txt
Simulate three different experts answering the below questions. All experts will write down 1 step of their thinking and then share it with the group. Then all the experts will go on the next step, etc. If any expert realizes they are wrong at any point, then they leave. Stop once you have the final answer for each question.
```

#### 自我优化循环

```txt
Try solving this <add context from IDE>. Then improve your answer in 3 iterations by critiquing and rewriting each version.
```

#### 迭代优化

多轮提示词迭代是正常的。使用反馈循环：

```txt
Prompt → Validate → Adjust prompt or fix code → Continue
```

> 绝不要假设第一次输出就可以直接合并。

## 常见陷阱

太模糊：`添加错误处理` —— 什么错误？在哪里？

太宽泛：`构建用户资料页面、后端 API、测试和样式` —— 拆解为更小的部分。

假设 AI 知道你的意图——它不知道。请明确说明。

跳过验证——始终审查、测试和核验 AI 的输出。

## 有趣的开源提示词

https://shumerprompt.com/prompts/expert-conductor-reasoning-guide-prompt-2ff044e1-5e65-48b3-8004-5f51e10e4a94

https://shumerprompt.com/prompts/vibe-coding-documentation-prompt-de4b2917-b4e3-44bd-ba7c-90eb09b508cd

https://shumerprompt.com/prompts/super-prompt-generator-optimizer-prompt-22b2a360-9935-49d6-81db-684385866847

https://shumerprompt.com/prompts/o3-maximum-reasoning-prompt-71b5828e-3c09-4df3-a9b7-25ef399e8977

## 参考资料

- [Master the core principles of prompt engineering with GitHub Copilot](https://www.youtube.com/watch?v=hh1nOX14TyY)
- [Prompt engineering essentials: Getting better results from LLMs | Tutorial](https://www.youtube.com/watch?v=LAF-lACf2QY)
- [AI prompt engineering: A deep dive](https://www.youtube.com/watch?v=T9aRN5JkmL8)
- [Essential AI prompts for developers](https://www.youtube.com/watch?v=H3M95i4iS5c)

## 深入学习

- [三专家法](./PROMPT_THREE_EXPERTS_METHOD.md)
- [零样本、单样本和多样本提示词](./PROMPT_ZERO_ONE_N_SHOT_PROMPTS.md)
- [多轮迭代推理法](./PROMPT_MULTIPLE_ITERATIONS_REASONING.md)
- [查看更多示例](./examples-prompts/)

## 继续阅读

[工作流程](./WORKFLOWS.md)
