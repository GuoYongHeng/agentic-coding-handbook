---
layout: default
title: 调试 MCP
parent: 工具与配置
nav_order: 3
---

# 使用 MCP 进行调试

目前有多种模型上下文协议（MCP）解决方案，可用于提取浏览器运行时数据以进行调试和上下文感知的 AI 辅助。我们评估了几种现有选项，并比较了它们在捕获控制台错误、网络活动和应用状态方面的能力，同时确保符合安全和隐私要求。以下是基于评估结果推荐的首选方案。

## Puppeteer MCP

全面的浏览器运行时数据访问能力

- 对复杂场景的 JavaScript 执行更加一致
- 依赖体积更小
- API 模式更简洁
- 使用来自 @modelcontextprotocol/servers 仓库的官方参考实现

## 配置

文档：[puppeteer mcp](https://github.com/modelcontextprotocol/servers/tree/main/src/puppeteer)

安装：`npm install --save-dev @modelcontextprotocol/server-puppeteer`

该软件包来自官方模型上下文协议参考实现

在 `.vscode/mcp.json` 中的配置：

```json
"puppeteer_mcp": {
  "command": "npx",
  "args": [
    "@modelcontextprotocol/server-puppeteer"
  ]
}
```

## 限制与挑战：连接到现有浏览器会话

一个显著的限制是无法轻松连接到发生错误时正在运行的浏览器实例。AI 助手会使用 MCP 根据正在运行的应用 URL 导航到该应用，这会重新加载浏览器窗口/标签页，或打开一个新的浏览器会话。

这为实际调试带来了若干挑战：

- **错误复现：** 开发者必须在由 MCP 工具控制的新浏览器会话中复现错误，这对于偶发性问题或依赖特定用户操作或状态的问题来说可能很困难。

- **上下文丢失：** 当用户浏览器中发生错误时，如果无法直接从该会话中提取，宝贵的上下文（控制台历史、网络请求、应用状态）就会丢失。

- **开发体验：** 工作流程变得繁琐，因为开发者需要提供详细指令，以便在独立的 MCP 控制浏览器实例中复现错误。

## 继续阅读

[调试工作流程](./WORKFLOW_DEBUG.md)
