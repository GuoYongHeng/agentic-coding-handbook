# React 企业级设置助手

## 使命
使用最新的企业标准创建生产就绪的 React 应用，实现零配置开发环境。所有工具必须经过配置、测试并验证可完美运行。

---

## 强制 WEB 验证
**在任何安装之前，请访问官方文档：**
1. **最新版本：** React (https://react.dev/versions), Storybook (https://storybook.js.org/docs/get-started/install), Tailwind (https://tailwindcss.com/docs/installation)
2. **Vite 兼容性：** 查看 GitHub 上 (React、Tailwind、Storybook) 最新版本的 (package.json)，确定与这三者兼容的最新 Vite 版本。
3. **使用各包官方文档中的精确安装命令**
4. **关键：确定兼容的 Vite 版本** - 使用与所有包兼容的最高版本，不一定是 @latest

---

## 项目设置

### 初始化与验证
```bash
npm create vite@[compatible-version] my-app -- --template react-ts
cd my-app
# 其中 [compatible-version] = 与 React、Tailwind、Storybook 兼容的最新 Vite 版本
# 示例：npm create vite@5.4.2 my-app -- --template react-ts
```

### 包安装 - 遵循官方文档
**核心：** react-router-dom, @tanstack/react-query + devtools, axios
**UI：** tailwindcss + @tailwindcss/vite, shadcn/ui (https://ui.shadcn.com/docs/installation/vite), tailwind-merge, clsx, class-variance-authority
**表单：** react-hook-form, @hookform/resolvers, zod, @tanstack/react-table
**开发工具：** eslint + @eslint/js + typescript-eslint, prettier + prettier-plugin-tailwindcss, vitest + @vitest/ui + @testing-library/react + @testing-library/jest-dom
**Storybook：** 使用 https://storybook.js.org/docs/get-started/frameworks/react-vite 中的命令
**工具库：** date-fns, lodash, recharts, @react-spring/web, @fortawesome 包, i18next + react-i18next + i18next-browser-languagedetector
**测试：** msw（仅用于测试），jsdom

---

## 架构

### 文件夹结构
```
src/
├── common/
│   ├── components/    # 共享 UI（Shadcn/ui 组件）
│   ├── hooks/         # 自定义 Hook（每个文件一个）
│   ├── utils/         # 辅助函数
│   └── api/           # API 客户端逻辑
├── pages/
│   ├── HomePage/      # 页面子目录
│   ├── AboutPage/
│   └── ContactPage/
└── assets/
```

### 规范
✅ **应做：** 页面子目录、同位测试、每个文件一个 Hook
❌ **避免：** 桶文件（影响 tree shaking）、独立测试目录

---

## 配置与功能

### 需求
- **路由：** Home、About、Contact、404，带错误边界
- **组件：** 带 CVA 变体的 Shadcn/ui Button、Card、Input、Dialog、Table
- **测试：** Vitest + 同位测试，MSW 仅用于测试模拟
- **质量：** ESLint 9+ 平铺配置，带 Tailwind 插件的 Prettier
- **文档：** 用于组件展示的 Storybook

### 核心功能
- 联系表单（React Hook Form + Zod 验证）
- 数据获取（TanStack Query，带加载/错误状态）
- 国际化（i18next，支持语言切换）
- 响应式设计（Tailwind 工具类）

---

## 验证

### 自动化检查（所有检查必须通过）
```bash
npm run dev       # 开发服务器启动
npm run build     # 生产构建成功
npm run test      # 所有测试通过，0 个失败
npm run lint      # 零错误/警告
npm run storybook # 组件正确加载
```

### 功能测试
- 导航正常、表单验证、数据正确获取
- 组件带变体渲染、i18n 语言切换正常

---

## 交付物

### 实现需求
1. **安装步骤**，包含来自官方文档的精确命令
2. **配置文件**（ESLint、Prettier、Vitest、Storybook）
3. **可用示例**（表单、数据获取、组件）
4. **同位测试**，带覆盖率
5. **验证命令**，确认功能正常

### 文档
- 列出每个包访问的官方 URL
- 确认使用的最新稳定版本（React、Storybook、Tailwind）
- 记录任何兼容性决策

### 质量标准
- TypeScript 严格模式，使用最新稳定版本
- 零构建/代码检查错误，所有功能正常运行
- 安装命令来自官方文档

---

## 成功标准
**生产就绪的 React 应用，具备完整工具链、通过测试、团队入职零配置。**

**质量保证：** 在所有验证检查通过之前，任务未完成。
