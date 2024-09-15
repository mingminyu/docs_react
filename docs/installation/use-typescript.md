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
