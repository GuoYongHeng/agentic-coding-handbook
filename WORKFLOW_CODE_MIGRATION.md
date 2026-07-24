# 迁移工作流

*借助 AI 现代化遗留代码的结构化方法*

## 概述

该工作流建立在[重构工作流](./WORKFLOW_REFACTORING.md)的基础上，但专注于**大型迁移**——从遗留代码库迁移到现代架构。代码迁移通常不仅仅是清理代码，还涉及重新思考架构、解耦职责，以及在原本缺失的地方引入测试和可观测性。

本指南概述了如何将智能体工具（如 Copilot Chat、Cursor、Claude）与深思熟虑的人工设计相结合，安全、增量地迁移遗留系统，并对结果充满信心。

> **重要：** 避免将迁移视为大爆炸式重写。将范围分解为可管理的、定义明确的块。这降低了风险，加速了反馈，并让新系统更早地交付价值。

---

## 第一步：理解遗留代码库

在触碰任何东西之前，先建立对遗留应用如何工作的心智模型。

### 使用 AI 智能体来：

* 从旧源文件中提取业务规则
* 生成遗留文件的高级摘要（如 `main.cob`、`operations.cob`）
* 识别组件之间的关系和依赖关系
* 通过注释、未使用路径或内联配置搜索遗留模式和未文档化的逻辑
* 建议可用于后续插入日志或适配器逻辑的边界或接缝
* 识别潜在的迁移挑战（如缺少测试、复杂依赖关系、自定义函数或模块）

#### 建议的提示词：

```
@workspace explain how the system works, including data flow and file responsibilities.
```

对每个模块重复此操作以降低幻觉风险。如果系统较大，使用思维链问答方式。

---

## 第二步：规划迁移策略

避免一次性重写所有内容。相反，定义一个**渐进式迁移计划**。

* 使用**绞杀者模式**逐步替换遗留模块
* 引入**适配器**允许新旧逻辑共存
* 根据风险、影响和变更频率确定领域优先级
* 定义**迁移切片**（如按领域、UI 路由、服务边界划分）并为每个切片规划可交付成果

### 使用 AI 智能体来：

* 根据代码结构和已识别的职责生成迁移计划草稿
* 通过分析模块职责和耦合度建议自然的切片策略
* 起草适配器层以连接遗留组件与新接口
* 使用变更频率、错误历史和业务重要性确定迁移项目的优先级
* 使用基于嵌入的语义相似性按功能、依赖关系或变更频率对文件进行聚类
* 评估新旧代码合同之间的兼容性（函数签名、对象结构）

#### 建议的提示词：

```
Based on this codebase, can you suggest a migration plan that replaces modules incrementally?
```

```
Given these legacy modules, which parts should be migrated first to reduce risk and unlock quick wins?
```

定期记录并回顾计划。

---

## 第三步：建立干系人沟通机制

**大多数迁移失败是因为期望不一致，而非代码质量问题。** 尽早建立清晰的沟通渠道。

### 关键沟通要素

* **进度仪表板**：迁移状态的可视化呈现（完成百分比、已迁移模块、已识别风险）
* **定期更新**：包含具体进度指标的每周/双周报告
* **风险沟通**：透明地说明潜在干扰、时间表和缓解策略
* **业务影响转化**：将技术进度转化为业务价值指标

### 使用 AI 智能体进行干系人沟通：

* 从技术进度报告生成执行摘要
* 使用迁移指标创建可视化进度仪表板
* 起草技术决策的干系人友好解释
* 为不同受众类型（高管、产品经理、最终用户）建议沟通模板

#### 建议的沟通模板

```markdown
Migration Progress Update - Week X
- Modules Completed: X/Y (Z% complete)
- Business Value Delivered: [specific features or improvements]
- Upcoming Milestones: [next 2-3 deliverables]
- Risks & Mitigations: [current blockers and solutions]
- Timeline Status: [on track, delayed, ahead of schedule]
```

---

## 第四步：在遗留端引入测试

在任何迁移或分析开始之前，建立安全网。

遗留系统通常缺乏测试覆盖——利用这一点。

* 围绕关键逻辑编写**单元测试**
* 围绕面向用户的流程编写**端到端（e2e）测试**
* 从现有系统中的**真实输入/输出示例**推导测试

这些测试将作为合同来验证新实现，确保你不是在盲目重写。

> **注意：** 确保覆盖关键路径和边界情况。如果某些功能没有被使用，考虑删除它们以降低复杂性或重写不必要的部分。

### 使用 AI 智能体来：

* 从现有代码生成样板测试用例
* 将手动验证的输入和输出转化为回归测试用例
* 建议模拟策略以解耦遗留组件
* 验证测试是否覆盖了所有路径（行覆盖率与分支覆盖率）

### 可选：

使用 AI 从现有代码片段引导测试脚手架。

---

## 第五步：映射功能和数据流

一旦单独理解了各个文件，就映射它们之间的交互方式。

### 使用 Copilot 或 Cursor 来：

* 创建**序列图**或 **Mermaid 流程图**
* 识别服务、API 和数据库之间的关键交互
* 推断数据库表、队列和 REST 调用之间未文档化的流程
* 通过共享全局状态或副作用可视化隐藏或间接的耦合

#### 建议的提示词：

```
Can you create a Mermaid sequence diagram showing how data flows across modules X, Y, Z?
```

将其保存为活文档，用于指导架构决策。

---

## 第六步：定义目标架构

没有目标的迁移只是重构的混乱。

使用此步骤定义：

* 模块结构（单体、模块化单体或微服务）
* 领域边界
* 每个服务/模块的职责
* 整洁架构层（UI / 应用层 / 领域层 / 基础设施层）

### 使用 AI 智能体来：

* 根据当前代码聚类生成候选模块拆分方案
* 提出反映领域边界的服务分解计划
* 将遗留文件映射到未来的层级并建议过渡接口
* 指出哪些逻辑应该移到领域层与基础设施层

这不需要面面俱到，但必须提供**方向**。

---

## 第七步：规划回滚和回退机制

遗留系统通常有隐式假设，在迁移过程中需要明确的安全网。

### 关键回滚注意事项

* **功能开关**：使用切换在遗留实现和新实现之间切换
* **数据同步**：确保回滚期间的数据一致性
* **流量路由**：能够将用户重定向回遗留系统
* **安全上下文**：处理认证/授权状态转换

### 安全迁移的注意事项

* **硬编码密钥**：遗留系统通常将凭证直接嵌入代码中
* **遗留认证模式**：基于会话的认证与现代基于令牌的系统
* **权限模型**：基于角色的访问控制可能需要转换层

### 使用 AI 智能体进行回滚规划：

* 识别硬编码密钥并建议安全替代方案
* 为渐进式发布生成功能开关实现
* 创建回滚清单和程序
* 分析认证流程并建议迁移路径
* 设计新旧系统之间的数据同步策略

#### 回滚清单模板

```markdown
Pre-Migration:
[ ] Feature flags implemented and tested
[ ] Data backup and sync mechanisms verified
[ ] Rollback procedure documented and rehearsed
[ ] Security context transition plan validated

During Migration:
[ ] Monitor error rates and performance metrics
[ ] Validate authentication/authorization flows
[ ] Confirm data consistency between systems
[ ] Test rollback triggers under load

Post-Migration:
[ ] Keep legacy system warm for X days
[ ] Monitor for delayed edge cases
[ ] Document lessons learned
[ ] Update rollback procedures for next iteration
```

---

## 第八步：识别和隔离技术债务

迁移是暴露并消除隐藏债务的绝佳时机。

### 使用 Cursor 或 Copilot 来：

1. 构建文件/模块清单
2. 发现债务的常见迹象：巨型函数、全局状态、复杂分支
3. 标注需要完全重写与轻量重构的组件
4. 测量复杂度分数（如圈复杂度）并找出热点
5. 估算重写与隔离各自所需的时间

#### 建议的提示词链：

```
Explain structure → Identify modules → List technical debt areas
```

手动验证 AI 的建议。

---

## 第九步：迁移的提示词工程

智能体需要上下文。这意味着：

* 直接引用遗留文件（`#file:...`）
* 要求以可复用的块形式输出
* 以架构目标为导向（如解耦、分层）

### 使用 AI 智能体来：

* 在重写的代码中强制执行风格和架构规则
* 使用现代模式重构遗留逻辑
* 增量构建与遗留合同兼容的新 API
* 生成过渡接口（适配器、代理、防腐层）

#### 示例提示词：

```
Based on the legacy function `processLegacyTransaction()`, can you extract the business rule and propose a cleaner version using service/repository layers?
```

---

## 第十步：现代化工具链和基础设施

**迁移不仅仅是代码**——通常还包括现代化整个开发和部署生态系统。尽可能将这些更新与代码迁移**并行**推进。

### 基础设施现代化领域

* **CI/CD 流水线**：Jenkins → GitHub Actions、CircleCI 或类似工具
* **可观测性栈**：遗留监控 → OpenTelemetry、结构化日志
* **部署策略**：手动部署 → 基础设施即代码（Terraform、CDK）
* **开发工具**：IDE 设置、Lint 检查、测试框架

### 使用 AI 智能体进行基础设施迁移：

* 从现有 Jenkins 流水线生成 GitHub Actions 工作流
* 为当前手动部署创建基础设施即代码模板
* 建议可观测性改进和埋点位置
* 生成数据库模式和数据的迁移脚本
* 为遗留系统和新系统设计监控仪表板

### 迁移时间线优先级

1. **首先是开发工具** - 立即提升开发者效率
2. **其次是 CI/CD** - 确保安全、可重复的部署
3. **尽早引入可观测性** - 提供迁移进度的可见性
4. **最后是基础设施** - 等代码模式确立后再进行

#### 基础设施迁移清单

```markdown
Development Environment:
[ ] Modern IDE configuration and extensions
[ ] Automated code formatting and linting
[ ] Local development environment automation
[ ] Testing framework modernization

CI/CD Pipeline:
[ ] Automated testing in CI
[ ] Security scanning integration
[ ] Deployment automation
[ ] Rollback mechanisms

Observability:
[ ] Structured logging implementation
[ ] Metrics collection and dashboards
[ ] Distributed tracing setup
[ ] Alerting and incident response

Infrastructure:
[ ] Infrastructure as Code adoption
[ ] Security baseline hardening
[ ] Backup and disaster recovery
[ ] Performance monitoring
```

---

## 第十一步：定义成功指标

在开始之前定义 KPI：

* 你如何知道迁移是否有效？
* 你如何衡量回归？

示例：

* 新旧技术栈的错误率对比
* 交付新功能的时间
* 已替换的遗留代码覆盖率
* 迁移逻辑与遗留逻辑的测试通过率
* 每次新版本发布时开发者信心评分

### 使用 AI 智能体来：

* 比较遗留系统和新系统的日志和指标
* 自动将提交标记为"迁移相关"并追踪速度
* 通过分析测试快照、日志或 API 差异检测回归

随时间追踪这些指标以证明投入的价值。

---

## 第十二步：准备开发者入职文档

**中途加入迁移的新开发者需要快速了解上下文。** 创建记录决策和理由的活文档。

### 必要的入职材料

* **迁移决策日志**：为何选择某些方法而不是替代方案
* **架构决策记录（ADR）**：记录重要的技术决策
* **代码地图**：显示已迁移、进行中和遗留内容的可视化指南
* **本地开发设置**：如何在本地同时运行遗留系统和新系统

### 使用 AI 智能体进行入职：

* 从代码变更和迁移决策生成 ADR
* 创建展示迁移进度的可视化代码地图
* 为新团队成员起草入职清单
* 根据当前代码库状态维护最新的设置说明
* 生成解释迁移特定模式的富含上下文的代码注释

#### ADR 模板示例

```markdown
# ADR-001: Migration Strategy for User Authentication

## Status: Accepted

## Context
Legacy system uses session-based auth with server-side state.
Modern requirements need stateless, scalable authentication.

## Decision
Implement JWT-based authentication with refresh token rotation.

## Consequences
- Positive: Stateless, horizontally scalable
- Negative: Requires token refresh logic
- Migration: Dual-mode support during transition

## Implementation Notes
[Technical details and code examples]
```

---

## 最终说明

迁移与其说是编码，不如说是**理解和文档化**。通过结合提示词工程、测试驱动的安全网和以架构为导向的规划，你可以在不盲目前行的情况下现代化遗留系统。

从小处着手。宏观规划。分批交付。始终让智能体在力所能及的地方提供帮助——但永远不要完全进入自动驾驶模式。

---

## 相关工作流

* [重构工作流](./WORKFLOW_REFACTORING.md)
* [提示词工程](./PROMPT_ENGINEERING.md)
* [调试工作流](./WORKFLOW_DEBUG.md)
* [规格优先开发](./WORKFLOW_SPEC_FIRST_APPROACH.md)
