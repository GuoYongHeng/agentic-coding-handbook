---
layout: default
title: 模型上下文提供者（MCP）
parent: 工具与配置
nav_order: 2
---

# MCP 使用场景

模型上下文提供者（MCP）通过将外部来源的上下文自动注入到你的提示词中，扩展了 AI 智能体的能力。开发者无需手动复制粘贴信息，MCP 可以让 AI 实时访问 Jira 工单、Confluence 文档、Figma 设计和 GitHub 讨论等关键数据。这提升了 AI 生成代码和响应的准确性、速度和质量。

在我们的内部实验中，我们验证了若干有价值的 MCP 使用场景，这些场景对生产力和输出质量产生了直接影响：

- 直接从 Jira 工单获取功能需求详情，以了解验收标准、边界情况和业务规则。
- 从 Confluence 页面获取架构定义、内部标准、可复用模式和产品策略说明，以指导实现方案。
- 从 Figma 获取 UI 原型图，生成与最新设计保持一致的前端组件。
- 获取 GitHub Pull Request 评论，将同行反馈融入 AI 驱动的编程流程。
- 获取外部 API 文档，更快速、更少错误地集成第三方服务。
- 获取浏览器实时数据（如控制台错误），在开发过程中进行上下文感知的调试。

通过使用 MCP，开发者减少了在工具之间切换标签页的时间，将更多精力集中在构建和改进软件上。

## 适用于我们工作流程的 MCP

- Atlassian MCP（用于 Jira 和 Confluence）：[Atlassian Remote MCP](https://www.atlassian.com/platform/remote-mcp-server)

- GitHub MCP（用于 Pull Request 和评论）：[GitHub - github/github-mcp-server: GitHub 官方 MCP 服务器](https://github.com/github/github-mcp-server)

- [Figma MCP 插件（Talk to Figma）](https://www.figma.com/community/plugin/1485687494525374295/cursor-talk-to-figma-mcp-plugin)

MCP 是大规模高效智能体编程的核心推动力，帮助 AI 在无需持续人工干预的情况下，使用更丰富、更可靠的信息进行工作。

## 使用 MCP 的安全与合规准则

在将模型上下文提供者（MCP）集成到智能体编程工作流中时，安全必须放在首位。请遵循以下安全规范，以保护你的代码库、数据和凭证：

| **最佳实践**                                                  | **重要原因**                                                                 | **操作建议**                                                                                                                                                             |
| ------------------------------------------------------------------ | ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 优先使用官方 MCP                                               | 供应商会维护自己的 MCP，及时修复漏洞和安全问题。       | 使用 API 提供商发布的 MCP（如 GitHub、Atlassian、Figma）。除非绝对必要，避免使用社区分叉版本。                                             |
| 不在 MCP 中硬编码密钥                                           | 硬编码的凭证随时可能导致安全泄露。                             | 将 API 密钥或令牌存储在密钥管理层（如 HashiCorp Vault、AWS Secrets Manager），并通过环境变量或密钥注入步骤传递。 |
| 应用最小权限原则                                         | 权限范围过广会在密钥泄露时扩大影响范围。                                      | 生成仅允许 MCP 所需最小操作（如可能仅读取）的限定范围 API 密钥，并定期轮换。                                             |
| 在没有官方 MCP 时使用可信中间件               | 第三方自动化中枢已对凭证进行隔离和加密处理。               | 配置中间件的官方连接器，让其代理请求，避免将密钥暴露在开源仓库中。                                                                  |
| 审计所有非官方开源 MCP                              | 社区软件包可能包含恶意代码、过时依赖或隐藏的遥测数据。 | Fork 仓库，逐行审查，运行静态分析（如 Semgrep），并在部署前锁定依赖版本。                                                       |
| 绝不信任索取凭证的非官方闭源 MCP | 看不到代码，就无从审查。                                            | 如果提供商不公开代码，直接拒绝——没有例外。                                                                                  |

在接下来的章节中，我们将逐步说明如何在编程工作流中配置和使用每个 MCP。

## 参考资料

- [Figma and Atlassian MCPs for Cursor](https://www.loom.com/share/2c651abeb3394c38a218f2860084da0d)
- [Introducing the GitHub MCP Server: AI interaction protocol | GitHub Checkout](https://www.youtube.com/watch?v=d3QpQO6Paeg)
- [The Only 3 Videos You Need to Get Started with MCP](https://www.youtube.com/watch?v=YRfOiB0Im64)

## 继续阅读

[组织级指令](./ORG_INSTRUCTIONS.md)
