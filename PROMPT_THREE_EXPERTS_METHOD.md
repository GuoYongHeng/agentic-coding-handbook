---
layout: default
title: 三专家法
parent: 提示词工程
nav_order: 2
---

# 三专家法

三专家提示词是一种强大的推理模式，旨在帮助 AI 模拟更深层次的思考，避免给出肤浅或泛泛的回答。在智能体编程中，当你需要处理复杂的架构决策、重构策略或调试多层次问题时，这种方法尤为有用。

这种方法将 AI 从代码生成器转变为一个思考伙伴小组，每位成员从不同角度探索问题——并最终汇聚出最经过深思熟虑的解决方案。

## 方法是什么？

**核心提示词模式：**

```txt
Simulate three different experts answering the question below.
Each expert will write down one step of their thinking and share it with the group.
Then all experts move to the next step.
If any expert realizes they're wrong at any point, they drop out.
Continue until there's consensus on the final answer.
```

这创建了一个多步骤推理循环，鼓励内部批判和更深层次的分析。

| **使用场景**                              | **为什么有效**                                  |
| ----------------------------------------- | ------------------------------------------------- |
| 在架构策略之间做选择 | 揭示技术决策中的权衡取舍               |
| 重构复杂模块               | 呈现备选路径及其风险                |
| 调试难以发现的 Bug                | 模拟根本原因分析                     |
| 设计数据模型或 API             | 平衡结构、性能和清晰度      |
| 生成边界情况测试                | 专家扮演 QA 工程师或系统思考者 |

## 示例一：重构横切模块

**目标：** 决定如何重构跨微服务共享的 NotificationService。

提示词：

```txt
Simulate three backend experts reviewing how to refactor the NotificationService, which is currently tightly coupled to both billing and auth services.
Each expert will write one step of reasoning at a time and discuss with the group.
If one is clearly wrong, they should drop out.
Keep going until they converge on a refactoring strategy that isolates the service and makes it reusable.
```

**预期结果：**

- 讨论权衡取舍（如发布/订阅 vs 抽象层）
- 通过淘汰有缺陷的方案达成共识
- 最终建议是经过推理得出的，而非猜测

## 示例二：调试难以发现的 UI 状态 Bug

**目标**：登录后 UI 显示过期数据。为什么？

提示词：

```txt
Simulate three frontend engineers exploring why the dashboard shows stale user data after login.
Each one will reason step-by-step based on their own assumption:

Expert A suspects caching issues

Expert B suspects async state update problems

Expert C suspects token propagation failure
They'll reason in steps and eliminate theories as they go.
Stop when they reach the most likely root cause.
```

**预期结果：**

- 逐步的根本原因分析
- 利用日志、网络追踪或事件时序
- 更有力的假设来指导后续调试提示词

## 示例三：设计健壮的验证系统

**目标：** 为多表单 Web 应用选择 Zod、Yup 还是 class-validator。

提示词：

```txt
Simulate three full-stack engineers debating which validation system to use for a multi-form onboarding flow with dynamic field types.
Each expert will write one reasoning step at a time.
Expert A prefers Zod, B prefers Yup, C prefers class-validator.
If any realize their approach won't scale or violates team standards, they should drop out.
Stop once consensus is reached.
```

**预期结果：**

- 明确权衡取舍（TypeScript 友好性、异步规则、嵌套支持）
- 基于真实需求做出选择（团队标准、后端集成）

## 总结：为什么使用三专家模式？

| **优势**                                       | **在智能体编程中的帮助**              |
| ------------------------------------------------- | ----------------------------------------------- |
| 深化推理                                 | 防止给出肤浅或默认的答案         |
| 模拟权衡取舍                               | 反映真实团队的决策动态       |
| 减少幻觉                             | 专家之间相互挑战逻辑            |
| 在编码前进行探索                   | 支持"先推理，后编码"的思维方式     |
| 适用于 Claude、GPT-4o、Cursor Agent 模式 | 高上下文工具擅长多步骤提示词 |

## 进阶变体

对于复杂决策：

```txt
Simulate a system architect, a QA lead, and a product manager reasoning together.
Each shares their perspective, step-by-step, to align on a final implementation plan.
```

## 参考资料

- [Leveraging AI with the Three Experts Technique](https://www.loom.com/share/50de91feb2ca4abdbca0521d8049d81d)

## 继续阅读

[多轮迭代推理法](./PROMPT_MULTIPLE_ITERATIONS_REASONING.md)
