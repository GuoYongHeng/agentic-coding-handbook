---
layout: default
title: 零/单/多样本提示词
parent: 提示词工程
nav_order: 4
---

# 零样本、单样本和多样本提示词

提示词是与 AI 交流的语言。在智能体编程中，你构建提示词的方式不仅决定代码的质量——还决定模型是否真正理解你的意图。

本页介绍三种基础的提示词方法——零样本、单样本和多样本——并展示如何在真实工程场景中策略性地应用它们，同时提供实用的、可复用的提示词示例。

## 样本提示词技术

### 零样本提示词

- 直接告诉 AI 要做什么，不提供示例。
- 当任务广为人知或模型具备强大先验知识时效果最佳。
- 当任务模糊或对实现方式敏感时，幻觉风险增加。

### 单样本提示词

- 告诉 AI 要做什么，并给出一个操作示例。
- 当你希望与已有模式保持一致，或想复用已知的实现格式时效果最佳。

### 多样本提示词

- 在提问前给 AI 提供多个示例，让其继续模式。
- 对于需要生成多个类似输出（如测试用例、验证器、端点）且要求结构或格式一致时效果最佳。

## 常见使用场景

| **提示词类型** | **理想使用场景**                                                                                                   |
| --------------- | -------------------------------------------------------------------------------------------------------------------- |
| 零样本       | 快速代码片段、重构、文档生成、错误分析                                                                |
| 单样本        | 重复性逻辑（如端点结构、命名规范、表单组件）                      |
| 多样本      | 批量生成测试、跨模块转换逻辑、全仓库一致性（如 DTO、服务契约） |

## 示例

### 使用项目特定约定编写新的 API 端点

**目标**：创建一个新的 POST /users/invite 端点，使用与项目中其他端点相同的模式。

零样本提示词

```txt
Write a NestJS controller method to handle POST /users/invite. It should accept email and name, call UserInviteService.inviteUser(), and return a success response or validation error.
```

风险：输出可能偏离项目特定的命名规范、装饰器或 DTO 结构。

单样本提示词

```txt
Here's how we write our endpoints:

@Post('/users/register')
registerUser(@Body() body: RegisterUserDto) {
  return this.service.register(body);
}

Now create an endpoint for POST /users/invite that follows the same pattern, usingInviteUserDto and inviteUser().
```

优势：确保装饰器、命名和结构的一致性。

多样本提示词

```txt
Here's how we write our endpoints:

@Post('/users/register')
registerUser(@Body() body: RegisterUserDto) {
  return this.service.register(body);
}

@Post('/users/reset-password')
resetPassword(@Body() body: ResetPasswordDto) {
  return this.service.resetPassword(body);
}

Now write a controller method for POST /users/invite.
```

优势：让 AI 基于模式建模，而不仅仅是单个实例。适用于生成一系列对齐的端点。

## 继续阅读

[核心工作流程](./core-workflows.md)
