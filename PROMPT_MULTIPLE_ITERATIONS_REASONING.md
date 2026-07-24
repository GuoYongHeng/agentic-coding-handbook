---
layout: default
title: 多轮迭代推理法
parent: 提示词工程
nav_order: 3
---

# 多轮迭代推理法

多轮迭代推理提示词是一种结构化方法，旨在引导 AI 系统经历渐进式的自我改进轮次。这种方法利用迭代分析和优化，产出更健壮、更经过优化、更全面考量的解决方案。

在智能体编程中，这项技术对于需要分层分析的复杂问题尤为有价值，例如算法开发、系统架构设计或代码优化——在这些场景中，第一个解决方案很少是最佳方案。

## 方法是什么？

**核心提示词模式：**

```txt
I want you to solve the following problem/task: [DESCRIBE PROBLEM OR TASK HERE]

## Iterative Solution Process

### 1. Initial Solution
Provide a concise initial solution to the problem. Focus on core requirements and a working approach. Keep code examples minimal.

### 2. Analysis Rounds (3 iterations)
For each round:

#### a) Critical Analysis
- Strengths: What works well (2-3 key points)
- Weaknesses: Edge cases and limitations (2-3 key points)
- Potential optimizations (1-2 specific improvements)

#### b) Solution Refinement
- Implement key changes that address the most critical weaknesses
- Focus only on substantial improvements
- Note briefly what changed and why

### 3. Last Solution
Provide your optimized solution with:
- A brief summary of major improvements (2-3 sentences)
- Any remaining considerations
```

这创建了一个自我反思的循环，鼓励 AI 多次批判性地评估和改进自己的工作。

| **使用场景**                     | **为什么有效**                                       |
| -------------------------------- | ------------------------------------------------------ |
| 算法优化           | 强制考虑边界情况和性能     |
| 系统设计改进         | 分层构建错误处理和健壮性      |
| 代码质量提升         | 逐步增强可读性和可维护性 |
| 有约束条件的问题求解 | 针对日益复杂的标准测试方案   |
| 测试覆盖规划           | 从基础测试扩展到全面测试场景     |

## 示例一：优化搜索算法

**目标：** 开发一种高效的部分排序数据搜索算法。

提示词：

```txt
I want you to solve the following problem: Design an algorithm to find a target number in a partially sorted array (elements are sorted in ascending order, then rotated at some pivot).

## Iterative Solution Process

### 1. Initial Solution
Provide a concise initial solution to the problem. Focus on core requirements and a working approach. Keep code examples minimal.

### 2. Analysis Rounds (3 iterations)
For each round:

#### a) Critical Analysis
- Strengths: What works well (2-3 key points)
- Weaknesses: Edge cases and limitations (2-3 key points)
- Potential optimizations (1-2 specific improvements)

#### b) Solution Refinement
- Implement key changes that address the most critical weaknesses
- Focus only on substantial improvements
- Note briefly what changed and why

### 3. Last Solution
Provide your optimized solution with:
- A brief summary of major improvements (2-3 sentences)
- Any remaining considerations
```

**预期结果：**

- 初始方案可能使用线性搜索 O(n)
- 第一轮迭代可能发现二分搜索的潜力
- 第二轮迭代可能处理旋转的复杂性
- 最终方案可能优化至 O(log n) 并涵盖详细的边界情况

## 示例二：设计缓存策略

**目标：** 为数据密集型应用创建缓存实现方案。

提示词：

```txt
I want you to solve the following problem: Design a caching strategy for a web application that handles thousands of product queries per minute with data that changes infrequently (once per day).

## Iterative Solution Process

### 1. Initial Solution
Provide a concise initial solution to the problem. Focus on core requirements and a working approach. Keep code examples minimal.

### 2. Analysis Rounds (3 iterations)
For each round:

#### a) Critical Analysis
- Strengths: What works well (2-3 key points)
- Weaknesses: Edge cases and limitations (2-3 key points)
- Potential optimizations (1-2 specific improvements)

#### b) Solution Refinement
- Implement key changes that address the most critical weaknesses
- Focus only on substantial improvements
- Note briefly what changed and why

### 3. Last Solution
Provide your optimized solution with:
- A brief summary of major improvements (2-3 sentences)
- Any remaining considerations
```

**预期结果：**

- 初始方案可能使用简单的基于时间的缓存
- 渐进式迭代解决失效策略、内存问题
- 后续轮次可能引入 Redis、缓存分层或预热机制
- 最终方案可能包含带回退机制的综合策略

## 示例三：构建健壮的 API 错误处理系统

**目标：** 为微服务架构设计错误处理系统。

提示词：

```txt
I want you to solve the following problem: Design a standardized error handling system for a collection of microservices that needs to provide consistent error responses, logging, retries, and circuit breaking.

## Iterative Solution Process

### 1. Initial Solution
Provide a concise initial solution to the problem. Focus on core requirements and a working approach. Keep code examples minimal.

### 2. Analysis Rounds (3 iterations)
For each round:

#### a) Critical Analysis
- Strengths: What works well (2-3 key points)
- Weaknesses: Edge cases and limitations (2-3 key points)
- Potential optimizations (1-2 specific improvements)

#### b) Solution Refinement
- Implement key changes that address the most critical weaknesses
- Focus only on substantial improvements
- Note briefly what changed and why

### 3. Last Solution
Provide your optimized solution with:
- A brief summary of major improvements (2-3 sentences)
- Any remaining considerations
```

**预期结果：**

- 初始方案可能专注于基本的错误结构
- 中间迭代优化重试策略、熔断逻辑
- 后续迭代可能增加可观测性、错误聚合
- 最终方案将是带有实现示例的分层方法

## 总结：为什么使用多轮迭代推理模式？

| **优势**                          | **在智能体编程中的帮助**                 |
| ------------------------------------ | -------------------------------------------------- |
| 促进深度而非广度          | 强制方案超越第一个显而易见的思路 |
| 记录思维的演进过程  | 在方案开发中创造透明度   |
| 系统性地识别边界情况 | 减少"哦，我没想到这个"的情况    |
| 内置有理由的优化       | 每次改进都有明确的理由            |
| 模拟真实开发过程    | 与工程师实际解决问题的方式保持一致  |
| 适用于现代 AI 模型     | 充分利用 LLM 的自我批判能力       |

## 变体

针对特定领域进行更深度的优化：

```txt
For iteration 2, focus specifically on performance optimization.
For iteration 3, focus exclusively on edge case handling.
```

受限提示词：

```txt
Limit each solution to under 50 lines of code, forcing increasingly elegant solutions.
```

## 参考资料

- [Multiple Iterations Reasoning Prompt Pattern](https://www.loom.com/share/10ecca1aa5a54eaf95669f2fe16cd56f?sid=1607557b-5d22-4d49-935e-933bdde55442)

## 继续阅读

[零样本、单样本和多样本提示词](./PROMPT_ZERO_ONE_N_SHOT_PROMPTS.md)
