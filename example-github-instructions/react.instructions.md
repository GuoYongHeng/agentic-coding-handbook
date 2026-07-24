---
applyTo: "**/*.tsx"
---

# React 开发规范与最佳实践

## React + TypeScript 编码标准

- 必须始终使用 TypeScript 的**函数式组件**。
- 必须使用 `interface` 或 `type` 定义**属性类型**，而不是依赖 `PropTypes`。
- 必须始终显式地为组件属性定义类型，即使属性为空（例如 `FC<>` 或 `React.FC<{ propName: string }>`）。
- 绝不使用 `any` 类型。请使用更具体的类型，如 `unknown`、`string` 或自定义类型/接口。
- 必须使用 `useState` 为所有状态变量定义类型（例如 `const [count, setCount] = useState<number>(0);`）。
- 必须始终显式地为事件处理函数定义类型（例如 `(event: React.ChangeEvent<HTMLInputElement>) => void`）。

## JSX + TypeScript 最佳实践

- 必须始终将多行 JSX 表达式用括号包裹，以提高可读性。
- 必须对没有子元素的元素使用**自闭合标签**（例如 `<img>`、`<input />`）。
- 绝不在 JSX 中直接内联复杂逻辑。将其提取到辅助函数或变量中以提高清晰度。
- 渲染元素列表时必须始终提供 `key` 属性，以确保 React 正确进行协调。
- 应始终使用 `React.RefObject<T>` 或 `React.MutableRefObject<T>` 为 ref 定义类型。

## 组件设计指南

- 设计组件时必须遵循**单一职责原则（SRP）**。每个组件应只做好一件事。
- 绝不创建超过 **200 行代码**的组件。必要时将其拆分为更小的可复用组件。
- 创建可复用组件时，必须始终优先考虑**组合而非继承**。
- 绝不向单个组件传递超过 **5 个属性**。如需传递更多数据，请使用对象或上下文。
- 对于可复用组件，必须在适用时使用**泛型类型**（例如对列表或表单使用 `<T>`）。

## 使用 TypeScript 进行状态管理

- 必须使用 `useState` 或 `useReducer` 管理本地组件状态，并且必须显式地为状态定义类型。
  - 示例：`const [state, setState] = useState<{ count: number }>({ count: 0 });`
- 对于全局状态，应在适当的时候使用专用的状态管理库（如 Redux、Zustand）或 React Context API，并确保所有 action 和 reducer 都有类型定义。
- 绝不在状态中存储派生数据。应从现有状态或属性中动态计算。

## 性能优化

- 必须始终使用 `useMemo` 对昂贵的计算进行记忆化，使用 `useCallback` 对函数进行记忆化，并确保两者都有正确的类型定义。
  - 示例：`const memoizedValue = useMemo<number>(() => computeExpensiveValue(a, b), [a, b]);`
- 绝不将匿名函数作为属性传递，除非不需要记忆化。
- 必须对不需要频繁重渲染的函数组件使用 `React.memo`。
- 对于大型列表，应始终使用虚拟化库，如 `react-window` 或 `react-virtualized`。

## 样式指南

- 应优先使用 **CSS-in-JS 库**（如 styled-components、Emotion）或实用优先的 CSS 框架（如 TailwindCSS）来为组件添加样式。
- 使用 styled-components 或 Emotion 时，必须在 styled 组件中显式地为属性定义类型：

```

const StyledButton = styled.button<{ isActive: boolean }>` background-color: ${(props) => (props.isActive ? 'blue' : 'gray')};  `;

```

- 必须将样式限定在组件范围内，以避免全局 CSS 冲突。
- 绝不在 JSX 中直接硬编码样式，除非样式是动态的且无法通过 CSS 处理。

## 测试指南

- 必须使用指定的测试框架（如 React Testing Library）为所有关键组件编写单元测试。
- 测试应关注用户行为和组件输出，而非实现细节。
- 在测试中应模拟外部依赖，以隔离组件行为。
- 对于复杂的交互，必须编写跨越多个组件的集成测试。

## 错误处理

- 必须在适用的地方使用错误边界（`React.ErrorBoundary`）优雅地处理错误。
- 错误边界应显式地定义类型：

```

class ErrorBoundary extends React.Component<
{ children: React.ReactNode },
{ hasError: boolean }
> {
constructor(props: { children: React.ReactNode }) {
super(props);
this.state = { hasError: false };
}
// Implementation here
}

```

## 无障碍规范

- 必须确保所有交互元素都有可访问的标签（`aria-label`、`aria-labelledby` 等）。

## 代码检查与格式化

- 代码库必须遵循 `.eslintrc` 中定义的代码检查规则。在提交代码前修复所有代码检查问题。
- 使用 ESLint 插件，如 `eslint-plugin-react` 和 `eslint-plugin-react-hooks`。
- 通过 `@typescript-eslint` 包含 TypeScript 专属规则。
- 应使用 Prettier 作为默认格式化工具，以确保文件间样式一致。
