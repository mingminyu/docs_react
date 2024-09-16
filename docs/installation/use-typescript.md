# 使用 TypeScript

TypeScript 是一种向 JavaScript 代码添加类型定义的常用方法。TypeScript 天然支持 JSX——只需在项目中添加 [`@types/react`](https://www.npmjs.com/package/@types/react) 和 [`@types/react-dom`](https://www.npmjs.com/package/@types/react-dom) 即可获得完整的 React Web 支持。

!!! note "你将学到"

    - [在 React 组件中使用 TypeScript](https://zh-hans.react.dev/learn/typescript#typescript-with-react-components)
    - [带有 Hook 的类型示例](https://zh-hans.react.dev/learn/typescript#example-hooks)
    - [来自 `@types/react` 的常见类型](https://zh-hans.react.dev/learn/typescript#useful-types)
    - [更多学习资源](https://zh-hans.react.dev/learn/typescript#further-learning)

## 1. 安装 

所有的 [生产级 React 框架](https://zh-hans.react.dev/learn/start-a-new-react-project#production-grade-react-framework) 都支持使用 TypeScript。请按照框架特定的指南进行安装：

- [Next.js](https://nextjs.org/docs/app/building-your-application/configuring/typescript)
- [Remix](https://remix.run/docs/en/1.19.2/guides/typescript)
- [Gatsby](https://www.gatsbyjs.com/docs/how-to/custom-configuration/typescript/)
- [Expo](https://docs.expo.dev/guides/typescript/)

### 1.1 在现有 React 项目中添加 TypeScript

使用下面命令安装最新版本的 React 类型定义：

```bash
npm install @types/react @types/react-dom
```

然后在 `tsconfig.json` 中设置以下编译器选项：

1. 必须在 [`lib`](https://www.typescriptlang.org/tsconfig/#lib) 中包含 `dom`（注意：如果没有指定 `lib` 选项，默认情况下会包含 `dom`）。
2. [`jsx`](https://www.typescriptlang.org/tsconfig/#jsx) 必须设置为一个有效的选项。对于大多数应用程序，`preserve` 应该足够了。 如果你正在发布一个库，请查阅 [`jsx` 文档](https://www.typescriptlang.org/tsconfig/#jsx) 以选择合适的值。


## 2. 在 React 组件中使用 TypeScript

!!! note "注意"

    每个包含 JSX 的文件都必须使用 .tsx 文件扩展名。这是一个 TypeScript 特定的扩展，告诉 TypeScript 该文件包含 JSX。

使用 TypeScript 编写 React 与使用 JavaScript 编写 React 非常相似。与组件一起工作时的关键区别是，你可以为组件的 props 提供类型。这些类型可用于正确性检查，并在编辑器中提供内联文档。

以 [快速入门](https://zh-hans.react.dev/learn) 指南中的 [`MyButton` 组件](https://zh-hans.react.dev/learn#components) 为例，我们可以为按钮的 `title` 添加一个描述类型：

![alt text](../images/qs-05.png)

```jsx linenums="1" title="App.tsx"
function MyButton({ title }: { title: string }) {
  return (
    <button>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>欢迎来到我的应用</h1>
      <MyButton title="我是一个按钮" />
    </div>
  );
}
```

!!! note "注意"

    这些沙盒可以处理 TypeScript 代码，但它们不运行类型检查器。这意味着你可以修改 TypeScript 沙盒以进行学习，但你不会收到任何类型错误或警告。要进行类型检查，你可以使用 [TypeScript Playground](https://www.typescriptlang.org/play) 或使用更完整的在线沙盒。


这种内联语法是为组件提供类型的最简单方法，但是一旦你开始描述几个字段，它可能变得难以管理。相反，你可以使用 `interface` 或 `type` 来描述组件的 props：

![alt text](../images/qs-06.png)

```jsx linenums="1" title="App.jsx"
interface MyButtonProps {
  /** 按钮文字 */
  title: string;
  /** 按钮是否禁用 */
  disabled: boolean;
}

function MyButton({ title, disabled }: MyButtonProps) {
  return (
    <button disabled={disabled}>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton title="我是一个禁用按钮" disabled={true}/>
    </div>
  );
}
```

描述组件 props 的类型可以根据需要变得简单或复杂，但它们应该是使用 `type` 或 `interface` 描述的对象类型。你可以在 [对象类型](https://www.typescriptlang.org/docs/handbook/2/objects.html) 中了解 TypeScript 如何描述对象，但你可能还对使用 [联合类型](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) 描述可以是几种不同类型之一的 prop，以及在 [从类型创建类型](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html) 指南中参考更高级的用例。

## 3. Hooks 示例

来自 `@types/react` 的类型定义包括内置的 Hook，因此你可以在组件中使用它们，无需任何额外设置。它们是根据你在组件中编写的代码构建的，所以你会得到很多 [类型推断](https://www.typescriptlang.org/docs/handbook/type-inference.html)，并且理想情况下不需要处理提供类型的细节。

但是，我们可以看一下如何为 Hook 提供类型的几个示例。

### 3.1 `useState`

[`useState` Hook](https://zh-hans.react.dev/reference/react/useState) 会重用作为初始 state 传入的值以确定值的类型。例如：

```ts
// 推断类型为 "boolean"
const [enabled, setEnabled] = useState(false);
```

这将为 `enabled` 分配 `boolean` 类型，而 `setEnabled` 将是一个接受 `boolean` 参数的函数，或者返回 `boolean` 的函数。如果你想为 state 显式提供一个类型，你可以通过为 `useState` 调用提供一个类型参数来实现：

```ts
// 显式设置类型为 "boolean"
const [enabled, setEnabled] = useState<boolean>(false);
```

在这种情况下，这并不是很有用，但是当你有一个联合类型时，你可能想要提供一个 `type`。例如，这里的 `status` 可以是几个不同的字符串之一：

```ts
type Status = "idle" | "loading" | "success" | "error";

const [status, setStatus] = useState<Status>("idle");
```

或者，如 [选择 state 结构原则](https://zh-hans.react.dev/learn/choosing-the-state-structure#principles-for-structuring-state) 中推荐的，你可以将相关的 state 作为一个对象分组，并通过对象类型描述不同的可能性：

```ts linenums="1"
type RequestState =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success', data: any }
  | { status: 'error', error: Error };

const [requestState, setRequestState] = useState<RequestState>({ status: 'idle' });
```

### 3.2 `useReducer`

[`useReducer`](https://zh-hans.react.dev/reference/react/useReducer) 是一个更复杂的 Hook，它接受一个 reducer 函数和一个初始 state 作为参数，并将从初始 state 推断出 reducer 函数的类型。你可以选择性地为 `useReducer` 提供类型参数以为 state 提供类型。但是更好的做法仍然是在初始 state 上添加类型：

```ts linenums="1"
import { useReducer } from 'react';

interface State {
   count: number 
};

type CounterAction =
  | { type: "reset" }
  | { type: "setCount"; value: State["count"] }

const initialState: State = { count: 0 };

function stateReducer(state: State, action: CounterAction): State {
  switch (action.type) {
    case "reset":
      return initialState;
    case "setCount":
      return { ...state, count: action.value };
    default:
      throw new Error("Unknown action");
  }
}

export default function App() {
  const [state, dispatch] = useReducer(stateReducer, initialState);

  const addFive = () => dispatch({ type: "setCount", value: state.count + 5 });
  const reset = () => dispatch({ type: "reset" });

  return (
    <div>
      <h1>欢迎来到我的计数器</h1>

      <p>计数： {state.count}</p>
      <button onClick={addFive}>加 5</button>
      <button onClick={reset}>重置</button>
    </div>
  );
}
```

我们在几个关键位置使用了 TypeScript：

- `interface State` 描述了 reducer state 的类型。
- `type CounterAction` 描述了可以 dispatch 至 reducer 的不同 action。
- `const initialState: State` 为初始 state 提供类型，并且也将成为 `useReducer` 默认使用的类型。
- `stateReducer(state: State, action: CounterAction): State` 设置了 reducer 函数参数和返回值的类型。

除了在 `initialState` 上设置类型外，一个更明确的替代方法是为 `useReducer` 提供一个类型参数：

```ts linenums="1"
import { stateReducer, State } from './your-reducer-implementation';

const initialState = { count: 0 };

export default function App() {
  const [state, dispatch] = useReducer<State>(stateReducer, initialState);
}
```

### 3.3 `useContext`

[`useContext`](https://zh-hans.react.dev/reference/react/useContext) 是一种无需通过组件传递 props 而可以直接在组件树中传递数据的技术。它是通过创建 provider 组件使用，通常还会创建一个 Hook 以在子组件中使用该值。

从传递给 `createContext` 调用的值推断 context 提供的值的类型：

```ts linenums="1"
import { createContext, useContext, useState } from 'react';

type Theme = "light" | "dark" | "system";
const ThemeContext = createContext<Theme>("system");

const useGetTheme = () => useContext(ThemeContext);

export default function MyApp() {
  const [theme, setTheme] = useState<Theme>('light');

  return (
    <ThemeContext.Provider value={theme}>
      <MyComponent />
    </ThemeContext.Provider>
  )
}

function MyComponent() {
  const theme = useGetTheme();

  return (
    <div>
      <p>当前主题：{theme}</p>
    </div>
  )
}
```

当你没有一个合理的默认值时，这种技术是有效的，而在这些情况下，`null` 作为默认值可能感觉是合理的。但是，为了让类型系统理解你的代码，你需要在 `createContext` 上显式设置 `ContextShape | null`。

这会导致一个问题，你需要在 context consumer 中消除 `| null` 的类型。我们建议让 Hook 在运行时检查它的存在，并在不存在时抛出一个错误：

```ts linenums="1"
import { createContext, useContext, useState, useMemo } from 'react';

// 这是一个简单的示例，但你可以想象一个更复杂的对象
type ComplexObject = {
  kind: string
};

// 上下文在类型中创建为 `| null`，以准确反映默认值。
const Context = createContext<ComplexObject | null>(null);

// 这个 Hook 会在运行时检查 context 是否存在，并在不存在时抛出一个错误。
const useGetComplexObject = () => {
  const object = useContext(Context);
  if (!object) { throw new Error("useGetComplexObject must be used within a Provider") }
  return object;
}

export default function MyApp() {
  const object = useMemo(() => ({ kind: "complex" }), []);

  return (
    <Context.Provider value={object}>
      <MyComponent />
    </Context.Provider>
  )
}

function MyComponent() {
  const object = useGetComplexObject();

  return (
    <div>
      <p>Current object: {object.kind}</p>
    </div>
  )
}
```

### 3.4 `useMemo`

[`useMemo`](https://zh-hans.react.dev/reference/react/useMemo) 会从函数调用中创建/重新访问记忆化值，只有在第二个参数中传入的依赖项发生变化时，才会重新运行该函数。函数的类型是根据第一个参数中函数的返回值进行推断的，如果希望明确指定，可以为该 Hook 提供一个类型参数以指定函数类型。

```ts
// 从 filterTodos 的返回值推断 visibleTodos 的类型
const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
```

### 3.5 `useCallback` 

[`useCallback`](https://zh-hans.react.dev/reference/react/useCallback) 会在第二个参数中传入的依赖项保持不变的情况下，为函数提供相同的引用。与 `useMemo` 类似，函数的类型是根据第一个参数中函数的返回值进行推断的，如果希望明确指定，可以为这个 Hook 提供一个类型参数以指定函数类型。

```ts
const handleClick = useCallback(() => {
  // ...
}, [todos]);
```

当在 TypeScript 严格模式下，使用 `useCallback` 需要为回调函数中的参数添加类型注解。这是因为回调函数的类型是根据函数的返回值进行推断的——如果没有参数，那么类型就不能完全理解。

根据自身的代码风格偏好，你可以使用 React 类型中的 `*EventHandler` 函数以在定义回调函数的同时为事件处理程序提供类型注解：

```ts linenums="1"
import { useState, useCallback } from 'react';

export default function Form() {
  const [value, setValue] = useState("Change me");

  const handleChange = useCallback<React.ChangeEventHandler<HTMLInputElement>>((event) => {
    setValue(event.currentTarget.value);
  }, [setValue])
  
  return (
    <>
      <input value={value} onChange={handleChange} />
      <p>值： {value}</p>
    </>
  );
}
```

