---
layout: default
title: Visual Feedback Loop
parent: Core Workflows
nav_order: 4
---

# AI 智能体视觉反馈工作流

在处理前端时，代码并不总能呈现全貌。用户所见的内容——以及 UI 在不同屏幕尺寸、状态和交互中的表现——对质量至关重要。该工作流介绍如何将截图和浏览器上下文作为 AI 辅助迭代的输入。

通过将截图与提示词配对，并通过基于浏览器的 MCP 增强上下文，开发者可以让 AI 直接看到问题所在，并收到精准的、符合设计意图的改进建议。

## 何时使用此工作流

- 用 AI 生成 UI 界面后，想要验证间距、布局或响应式表现
- 审查 QA 或设计师报告的视觉缺陷时
- 重构视觉组件或改善跨浏览器兼容性时

## 截图获取与反馈

使用浏览器内置工具或截图扩展程序来截取：

- 完整页面截图
- 裁剪后的组件或布局区域
- 特定设备视口（如移动端）

保存图片并直接拖入 Cursor（或支持的 Claude），然后使用如下提示词：

```txt
Here's what the UI looks like. The layout doesn't match the Figma design — spacing is off and the sidebar is misaligned. What improvements can we make to align with a clean, balanced layout?
```

## 技巧

- 指出具体问题（"按钮字体太大"、"图片溢出容器"）
- 如果工具支持注释功能，可以使用注释（如 cursor artifacts 或 Claude 截图）

## 通过 MCP 添加浏览器上下文

为了进行更深层的调试或响应式修复，可以通过基于浏览器的模块化上下文提供者（MCP）向 AI 提供更多信号：

通过 MCP 可用的浏览器上下文：

- 控制台日志提示词：`Use browser MCP to fetch latest errors and logs. What do these console warnings mean?`
- DOM 结构提示词：`Fetch current DOM and analyze why #sidebar has overlapping margin.`
- 网络活动提示词：`Analyze failed network calls on page load using captured logs. Suggest fixes.`

使用浏览器 MCP（如 Chrome DevTools MCP）自动提取这些值作为上下文，让 AI 推断布局错误、渲染问题或 API 失败。

## 迭代并应用修复

分析反馈后，提示 AI 逐步生成安全的更改：

```txt
Based on the feedback, update the CSS for the header to improve vertical alignment and add spacing between nav items.
```

### 使用 AI 来：

- 重构布局
- 从设计系统中建议样式令牌
- 检测版本间的视觉回归
- 每次迭代后截取新截图以验证进度。

### 视觉反馈工作流的优势

- 让不可见的问题变得可见——尤其是间距、颜色、字体和布局的不一致性
- 加速与设计意图的对齐（如 Figma 规范）
- 帮助非技术利益相关者（QA、设计）通过截图参与审查
- 将用户所见与代码底层运行状态结合起来

## 示例提示词

```txt
Here's a screenshot of the user profile screen. Improve the layout to match a clean card-based structure with better spacing.
```

```txt
Use the DOM structure to suggest accessibility fixes based on ARIA roles and contrast.
```

```txt
Given the failed API call shown in network logs, what's likely missing in the fetch logic?
```

## 参考资料

- [Using Copilot with Visual Feedback](https://www.loom.com/share/a811bd60a39e4bd38073637e24101af8?sid=f3e88fab-2768-44bb-8b66-970229dbaee6)

## 继续阅读

[调试工作流](./WORKFLOW_DEBUG.md)
