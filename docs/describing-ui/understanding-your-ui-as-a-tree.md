# 将 UI 视为树

当 React 应用程序逐渐成形时，许多组件会出现嵌套。那么 React 是如何跟踪应用程序组件结构的？

React 以及许多其他 UI 库，将 UI 建模为树。将应用程序视为树对于理解组件之间的关系以及调试性能和状态管理等未来将会遇到的一些概念非常有用。

!!! note "你将学习到"

    - React 如何看待组件结构
    - 渲染树是什么以及它有什么用处
    - 模块依赖树是什么以及它有什么用处

## 1. 将 UI 视为树

树是项目和 UI 之间的关系模型，通常使用树结构来表示 UI。例如，浏览器使用树结构来建模 HTML（[DOM](https://developer.mozilla.org/docs/Web/API/Document_Object_Model/Introduction)）与CSS（[CSSOM](https://developer.mozilla.org/docs/Web/API/CSS_Object_Model)）。移动平台也使用树来表示其视图层次结构。

![](https://zh-hans.react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fpreserving_state_dom_tree.png&w=1920&q=75)

与浏览器和移动平台一样，React 还使用树结构来管理和建模 React 应用程序中组件之间的关系。这些树是有用的工具，用于理解数据如何在 React 应用程序中流动以及如何优化呈现和应用程序大小。

## 2. 渲染树

组件的一个主要特性是能够由其他组件组合而成。在 [嵌套组件](https://zh-hans.react.dev/learn/your-first-component#nesting-and-organizing-components) 中有父组件和子组件的概念，其中每个父组件本身可能是另一个组件的子组件。

当渲染 React 应用程序时，可以在一个称为渲染树的树中建模这种关系。

下面的 React 应用程序渲染了一些鼓舞人心的引语。

???+ example "示例"

    === "App.js"

        ```ts linenums="1"
        import FancyText from './FancyText';
        import InspirationGenerator from './InspirationGenerator';
        import Copyright from './Copyright';

        export default function App() {
        return (
            <>
            <FancyText title text="Get Inspired App" />
            <InspirationGenerator>
                <Copyright year={2004} />
            </InspirationGenerator>
            </>
        );
        }
        ```

    === "FancyText.js"

        ```ts
        export default function FancyText({title, text}) {
        return title
            ? <h1 className='fancy title'>{text}</h1>
            : <h3 className='fancy cursive'>{text}</h3>
        }
        ```
    
    === "InspirationGenerator.js"

        