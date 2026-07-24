# 管理智能体上下文

管理智能体上下文——尤其是将功能拆分为更小的部分——至关重要，这与大语言模型的工作原理密切相关。大语言模型一次只能"思考"有限数量的信息。当被过多文件、规格说明或模糊请求所淹没时，它们会丢失重要细节——这是一种被称为"迷失在中间"的常见局限性。

当你要求大语言模型一次性构建过多内容时，它往往会漏掉依赖关系、混淆结构，或生成不一致的代码。这会增加缺陷和返工。通过将功能拆分为更小、定义明确的任务，你为大语言模型提供了清晰的目标，减少了幻觉，并创建了人工验证的检查点——从而提升速度和质量。

与编程智能体协作的关键在于：在正确的时机，以正确的粒度，提供正确的信息。工程师必须获取相关文件、呈现有意义的代码片段，并引导模型。AI 的效用完全取决于你所提供的上下文。

## 以下是在 Visual Studio Code 中使用 GitHub Copilot Agent 提供有效上下文的主要方式：

- **提供描述清晰、以行动为导向的实现计划：** 提供清晰的分步计划（如规格说明或提示词计划）有助于 AI 理解工作范围，并安全地逐步推进实现阶段。这种结构化规划能显著提升 AI 性能并减少错误。（来源：[The Vibe Coding Workflow](https://www.linkedin.com/pulse/vibe-coding-workflow-michael-papadopoulos-n3wpf/)）

- **编写精心设计的提示词：** 提示词应清晰、具体且聚焦。一个好的提示词应明确定义任务、预期的输入/输出，以及任何约束条件（如偏好的框架或库）。提示词的质量直接影响 AI 响应的质量。（来源：[Prompt engineering for Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/prompt-crafting)）

- **使用 GitHub Copilot 工具提供额外上下文：** 你可以通过附加文件、文件夹、代码库搜索结果、终端输出、问题报告，甚至获取公开网页内容来丰富你的 Copilot 提示词，确保 AI 拥有生成更好答案所需的全部信息。（来源：[Copilot Chat Context](https://code.visualstudio.com/docs/copilot/chat/copilot-chat-context)）

- **集成模块化上下文提供者（MCPs）：** MCPs 允许 Copilot 在会话期间动态连接到外部数据源和服务，将 AI 的能力扩展到静态文件之外，实现对 API、Figma、Jira、GitHub 及其他实时外部系统的访问。（来源：[Use MCP servers in VS Code](https://code.visualstudio.com/docs/copilot/chat/mcp-servers)）

- **添加视觉附件，如原型图：** 提供 UI 原型图、截图或设计参考，有助于 AI 更好地理解前端或产品相关开发任务中预期的视觉结构和用户体验。

- **为 Copilot 定义项目级指令：** 你可以创建指令文件来设置自定义项目编码规范、偏好模式、命名约定和规则。这确保 Copilot 在整个项目中自动遵循一致的实践。（来源：[Customize Copilot Chat Responses](https://code.visualstudio.com/docs/copilot/copilot-customization)）

- **确保你的代码库已建立索引：** 在使用 GitHub Copilot 的代码搜索和上下文功能时，确保你的工作区已正确建立索引，使 Copilot 能更准确地找到相关文件，并提供具有更好上下文感知能力的响应。（来源：[Copilot Tips and Tricks - Workspace Indexing](https://code.visualstudio.com/docs/copilot/copilot-tips-and-tricks#_workspace-indexing)）

- **策略性地利用对话记忆：** 大语言模型会记住你会话的流程。在之前交流的基础上继续推进，而不是从头重复所有内容。如果某个对话线索有效，就继续它；如果不行，则重置并重新开始，以恢复清晰度。

- **让 AI 先从小处着手，再逐步扩大：** 先要求更简单的版本。让模型在小任务上取得成功，然后逐步增加复杂度。这能提升性能并减少"不知所措"的情况。

- **复用真实代码示例：** 大语言模型从可运行的示例中推理，比从理论中推理效果更好。粘贴过去的补全结果、现有代码或类似功能的样例，将你的提示词锚定在现实中。

- **优先选择能暴露上下文的工具：** 选择能让你看到上下文窗口的编程工具（如 Cursor 或 Claude），而不是隐藏它的工具。可见性有助于你更快地调试、学习和迭代。

- **创建并维护项目文档：** 团队可以作为代码库的一部分（如指令文件）或在 MCPs 可访问的外部系统中（如 Confluence）维护文档，以向上下文提供项目核心信息。一个例子是 [UI/UX 规范](./exmaples-documents/UI_UX_GUIDELINES.md)，旨在定义 AI 构建用户界面时必须遵循的模式。其他示例还包括由 Modus 解决方案设计团队起草的解决方案设计交接文档，或软件架构文档。

## 参考资料

- [大语言模型上下文处理问题简介](https://www.loom.com/share/29cc930d60c0438eb9174ae90a568051)

## 继续阅读

[提示词工程基础](./PROMPT_ENGINEERING.md)
