---
layout: default
title: 测试驱动开发
parent: 核心工作流程
nav_order: 2
---

# 测试驱动开发

测试驱动开发（TDD）和智能体编程看似相反——前者结构化而严谨，后者流畅而直觉驱动。但两者结合时，能创造出强大的反馈循环：TDD 为你的流程提供结构，智能体编程为你的结构提供速度。

当处理复杂逻辑文件时，这种组合尤为出色，例如定价引擎、基于规则的验证器或多条件工作流。不必一次性提示 AI 生成所有内容，而是通过测试逐一描述行为，让 AI 以增量、安全、清晰的方式构建逻辑。

## 为什么 TDD 能让智能体编程更好

- **测试充当提示词：** 在 AI 辅助工作流中，测试成为引导 AI 实现预期行为的自然语言规格说明。与其说"生成一个过滤有效邮件的函数"，不如写 it('should return only valid emails from a mixed list')，然后让 AI 编写代码来通过该测试。

- **减少幻觉：** 提示词越精确（在这个场景中即测试），生成结果越准确。
  TDD 让 LLM 专注于小型、可测试的目标，而不是臃肿的实现。

- **建立信心：** 当每个代码生成步骤都经过测试验证时，你知道它是有效的。当使用 AI 作为你的结对编程伙伴时，这一点至关重要。

- **保持工作流的流畅性：** 测试给你提供检查点。不必停下来调试模糊的输出，直接写下一个测试，让 AI 跟上来。

- **强化简洁的行为化思维：** TDD 强制你描述代码应该做什么，而不是如何编写。这正是我们应该提示 LLM 的方式。

## 智能体编程者的 TDD 技巧

- 先从高价值行为开始，而不是边界情况。
- 使用描述性的测试名称——测试越清晰，AI 结果越好。
- 保持测试范围紧凑：每次提示只验证一个行为。
- 让 AI 重构——要求它"清理逻辑但保持所有测试通过"。
- 使用预提交钩子运行测试，阻止不良代码合并。

## 提示词示例及其输出

```txt
### Prompt: Generate TDD Plan from Business Logic

I'm implementing a new feature based on the following business rules from a Jira ticket.
Please help me break it down into a clear **Test-Driven Development flow**, where each step represents a test to be written and the logic to be implemented to pass it.
Output the result as a **markdown checklist** (`tdd-plan.md`) using `[ ]` for each step.
The structure should guide me to:

* Write one test at a time
* Implement just enough code to pass it
* Add more rules incrementally
* Include edge cases and input validation
* Refactor at the end if needed

Here are the business rules:

\`\`\`markdown
Feature: calculate_invoice_total(items, customer_location)

Business rules:

1. Each item has:
   - price (number)
   - quantity (integer)
   - taxable (boolean)

2. Tax rules:
   - For customers in NY: apply 8.875% tax on taxable items
   - For customers in CA: apply 7.25% tax on taxable items
   - For all other locations: apply 5% tax on taxable items
   - Non-taxable items do not receive any tax

3. Discounts:
   - If subtotal (before tax) is greater than $1,000, apply a 5% discount on the subtotal
   - If the total quantity of all items is 10 or more, subtract a $15 shipping discount from the total

4. Final total = subtotal - discounts + tax

5. Edge cases:
   - If item list is empty, return total = 0
   - If location is missing or unknown, default to 5% tax
   - Negative prices or quantities are invalid and should raise an error
\`\`\`

Format the result like this:

\`\`\`markdown
# TDD Plan: [Feature Name]

\[ ] Write test for \[simple behavior]
\[ ] Implement logic to pass test
\[ ] Write test for \[next rule]
\[ ] Update implementation to pass
\[ ] Add test for edge case \[X]
\[ ] Refactor logic for clarity
\`\`\`

You can group tests by theme if needed. Only include things that should be verified through tests.
Avoid assumptions — reason from the rules given.
Before starting creating the file, you can ask questions to the user to clarify your understanding.
Check the boxes above as you progress. This is important, DO NOT FORGET ABOUT THIS.
```

输出：

```txt
# TDD Plan: Calculate Invoice Total

## Setup

- [ ] Create the basic function structure `calculate_invoice_total(items, customer_location)`
- [ ] Set up testing framework

## Basic Functionality

- [ ] Write test for empty items list returning 0
- [ ] Implement logic to return 0 for empty items list
- [ ] Write test for calculating subtotal with a single non-taxable item
- [ ] Implement logic to calculate basic subtotal (price * quantity)
- [ ] Write test for calculating subtotal with multiple non-taxable items
- [ ] Update implementation to handle multiple items

## Tax Rules

- [ ] Write test for default tax rate (5%) with a taxable item
- [ ] Implement logic to apply default tax rate
- [ ] Write test for NY tax rate (8.875%) with taxable item
- [ ] Update implementation to handle NY tax rate
- [ ] Write test for CA tax rate (7.25%) with taxable item
- [ ] Update implementation to handle CA tax rate
- [ ] Write test for mixing taxable and non-taxable items
- [ ] Update implementation to only apply tax to taxable items

## Discount Rules

- [ ] Write test for 5% discount on subtotal > $1,000
- [ ] Implement logic for subtotal discount
- [ ] Write test for $15 shipping discount when total quantity ≥ 10
- [ ] Implement logic for shipping discount
- [ ] Write test for both discounts applying simultaneously
- [ ] Update implementation to handle both discount types

## Edge Cases and Validation

- [ ] Write test for missing location defaulting to 5% tax rate
- [ ] Update implementation to handle missing location
- [ ] Write test for unknown location defaulting to 5% tax rate
- [ ] Update implementation to handle unknown locations
- [ ] Write test for negative price validation
- [ ] Implement error handling for negative prices
- [ ] Write test for negative quantity validation
- [ ] Implement error handling for negative quantities

## Final Calculations

- [ ] Write test for final total calculation (subtotal - discounts + tax)
- [ ] Verify implementation correctly calculates final total
- [ ] Write test for complex scenario with all rules applied
- [ ] Ensure implementation handles all business rules correctly

## Refactoring (if needed)

- [ ] Refactor tax calculation into separate method for clarity
- [ ] Refactor discount calculation into separate method
- [ ] Ensure all tests still pass after refactoring
```

## 参考资料

- [TDD with Github Copilot Agent](https://www.loom.com/share/d442996affe14bdea81014183f633988)

## 继续阅读

[自动代码验证](./WORKFLOW_AUTO_VALIDATIONS.md)
