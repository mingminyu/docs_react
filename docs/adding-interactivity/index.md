# 添加交互

界面上的控件会根据用户的输入而更新。例如，点击按钮切换轮播图的展示。在 React 中，随时间变化的数据被称为状态（**state**）。你可以向任何组件添加状态，并按需进行更新。在本章节中，你将学习如何编写处理交互的组件，更新它们的状态，并根据时间变化显示不同的效果。

!!! note "本章节"

    - [如何处理用户发起的事件](https://zh-hans.react.dev/learn/responding-to-events)
    - [如何用状态使组件“记住”信息](https://zh-hans.react.dev/learn/state-a-components-memory)
    - [React 是如何分两个阶段更新 UI 的](https://zh-hans.react.dev/learn/render-and-commit)
    - [为什么状态在你改变后没有立即更新](https://zh-hans.react.dev/learn/state-as-a-snapshot)
    - [如何排队进行多个状态的更新](https://zh-hans.react.dev/learn/queueing-a-series-of-state-updates)
    - [如何更新状态中的对象](https://zh-hans.react.dev/learn/updating-objects-in-state)
    - [如何更新状态中的数组](https://zh-hans.react.dev/learn/updating-arrays-in-state)

## 1. 响应事件

React 允许你向 JSX 中添加事件处理程序。事件处理程序是你自己的函数，它将在用户交互时被触发，如点击、悬停、焦点在表单输入框上等等。

`<button>` 等内置组件只支持内置浏览器事件，如 `onClick`。但是，你也可以创建你自己的组件，并给它们的事件处理程序的 `props` 指定你喜欢的任何特定于应用的名称。

```js title="App.js" linenums="1"
export default function App() {
  return (
    <Toolbar
      onPlayMovie={() => alert('Playing!')}
      onUploadImage={() => alert('Uploading!')}
    />
  );
}

function Toolbar({ onPlayMovie, onUploadImage }) {
  return (
    <div>
      <Button onClick={onPlayMovie}>
        Play Movie
      </Button>
      <Button onClick={onUploadImage}>
        Upload Image
      </Button>
    </div>
  );
}

function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}
```

## 2. 状态是组件的内存

作为交互的结果，组件通常需要更改屏幕上的内容。在表单中输入应该更新输入字段，单击图片轮播上的 “下一个” 应该更改显示的图片，单击 “购买” 将产品放入购物车。组件需要 “记住” 的东西：当前输入值，当前图片，购物车。在 React 中，这种特定于组件的内存称为状态。

你可以使用 [`useState`](https://react.nodejs.cn/reference/react/useState) 钩子将状态添加到组件。钩子是让你的组件使用 React 功能的特殊函数（状态是这些功能之一）。`useState` 钩子允许你声明一个状态变量，它采用初始状态并返回一对值：当前状态，以及一个让你更新它的状态设置函数。

```jsx linenums="1"
const [index, setIndex] = useState(0);
const [showMore, setShowMore] = useState(false);
```

以下是图片库如何在点击时使用和更新状态：

???+ example "示例"

    === "App.js"

        ```jsx linenums="1"
        import { useState } from 'react';
        import { sculptureList } from './data.js';
        
        export default function Gallery() {
          const [index, setIndex] = useState(0);
          const [showMore, setShowMore] = useState(false);
          const hasNext = index < sculptureList.length - 1;
        
          function handleNextClick() {
            if (hasNext) {
              setIndex(index + 1);
            } else {
              setIndex(0);
            }
          }
        
          function handleMoreClick() {
            setShowMore(!showMore);
          }
        
          let sculpture = sculptureList[index];
          return (
            <>
              <button onClick={handleNextClick}>
                Next
              </button>
              <h2>
                <i>{sculpture.name} </i>
                by {sculpture.artist}
              </h2>
              <h3>
                ({index + 1} of {sculptureList.length})
              </h3>
              <button onClick={handleMoreClick}>
                {showMore ? 'Hide' : 'Show'} details
              </button>
              {showMore && <p>{sculpture.description}</p>}
              <img
                src={sculpture.url}
                alt={sculpture.alt}
              />
            </>
          );
        }
        ```

    === "data.js"

        ```jsx linenums="1"
        export const sculptureList = [{
          name: 'Homenaje a la Neurocirugía',
          artist: 'Marta Colvin Andrade',
          description: 'Although Colvin is predominantly known for abstract themes that allude to pre-Hispanic symbols, this gigantic sculpture, an homage to neurosurgery, is one of her most recognizable public art pieces.',
          url: 'https://i.imgur.com/Mx7dA2Y.jpg',
          alt: 'A bronze statue of two crossed hands delicately holding a human brain in their fingertips.'
        }, {
          name: 'Floralis Genérica',
          artist: 'Eduardo Catalano',
          description: 'This enormous (75 ft. or 23m) silver flower is located in Buenos Aires. It is designed to move, closing its petals in the evening or when strong winds blow and opening them in the morning.',
          url: 'https://i.imgur.com/ZF6s192m.jpg',
          alt: 'A gigantic metallic flower sculpture with reflective mirror-like petals and strong stamens.'
        }, {
          name: 'Eternal Presence',
          artist: 'John Woodrow Wilson',
          description: 'Wilson was known for his preoccupation with equality, social justice, as well as the essential and spiritual qualities of humankind. This massive (7ft. or 2,13m) bronze represents what he described as "a symbolic Black presence infused with a sense of universal humanity."',
          url: 'https://i.imgur.com/aTtVpES.jpg',
          alt: 'The sculpture depicting a human head seems ever-present and solemn. It radiates calm and serenity.'
        }, {
          name: 'Moai',
          artist: 'Unknown Artist',
          description: 'Located on the Easter Island, there are 1,000 moai, or extant monumental statues, created by the early Rapa Nui people, which some believe represented deified ancestors.',
          url: 'https://i.imgur.com/RCwLEoQm.jpg',
          alt: 'Three monumental stone busts with the heads that are disproportionately large with somber faces.'
        }, {
          name: 'Blue Nana',
          artist: 'Niki de Saint Phalle',
          description: 'The Nanas are triumphant creatures, symbols of femininity and maternity. Initially, Saint Phalle used fabric and found objects for the Nanas, and later on introduced polyester to achieve a more vibrant effect.',
          url: 'https://i.imgur.com/Sd1AgUOm.jpg',
          alt: 'A large mosaic sculpture of a whimsical dancing female figure in a colorful costume emanating joy.'
        }, {
          name: 'Ultimate Form',
          artist: 'Barbara Hepworth',
          description: 'This abstract bronze sculpture is a part of The Family of Man series located at Yorkshire Sculpture Park. Hepworth chose not to create literal representations of the world but developed abstract forms inspired by people and landscapes.',
          url: 'https://i.imgur.com/2heNQDcm.jpg',
          alt: 'A tall sculpture made of three elements stacked on each other reminding of a human figure.'
        }, {
          name: 'Cavaliere',
          artist: 'Lamidi Olonade Fakeye',
          description: "Descended from four generations of woodcarvers, Fakeye's work blended traditional and contemporary Yoruba themes.",
          url: 'https://i.imgur.com/wIdGuZwm.png',
          alt: 'An intricate wood sculpture of a warrior with a focused face on a horse adorned with patterns.'
        }, {
          name: 'Big Bellies',
          artist: 'Alina Szapocznikow',
          description: "Szapocznikow is known for her sculptures of the fragmented body as a metaphor for the fragility and impermanence of youth and beauty. This sculpture depicts two very realistic large bellies stacked on top of each other, each around five feet (1,5m) tall.",
          url: 'https://i.imgur.com/AlHTAdDm.jpg',
          alt: 'The sculpture reminds a cascade of folds, quite different from bellies in classical sculptures.'
        }, {
          name: 'Terracotta Army',
          artist: 'Unknown Artist',
          description: 'The Terracotta Army is a collection of terracotta sculptures depicting the armies of Qin Shi Huang, the first Emperor of China. The army consisted of more than 8,000 soldiers, 130 chariots with 520 horses, and 150 cavalry horses.',
          url: 'https://i.imgur.com/HMFmH6m.jpg',
          alt: '12 terracotta sculptures of solemn warriors, each with a unique facial expression and armor.'
        }, {
          name: 'Lunar Landscape',
          artist: 'Louise Nevelson',
          description: 'Nevelson was known for scavenging objects from New York City debris, which she would later assemble into monumental constructions. In this one, she used disparate parts like a bedpost, juggling pin, and seat fragment, nailing and gluing them into boxes that reflect the influence of Cubism’s geometric abstraction of space and form.',
          url: 'https://i.imgur.com/rN7hY6om.jpg',
          alt: 'A black matte sculpture where the individual elements are initially indistinguishable.'
        }, {
          name: 'Aureole',
          artist: 'Ranjani Shettar',
          description: 'Shettar merges the traditional and the modern, the natural and the industrial. Her art focuses on the relationship between man and nature. Her work was described as compelling both abstractly and figuratively, gravity defying, and a "fine synthesis of unlikely materials."',
          url: 'https://i.imgur.com/okTpbHhm.jpg',
          alt: 'A pale wire-like sculpture mounted on concrete wall and descending on the floor. It appears light.'
        }, {
          name: 'Hippos',
          artist: 'Taipei Zoo',
          description: 'The Taipei Zoo commissioned a Hippo Square featuring submerged hippos at play.',
          url: 'https://i.imgur.com/6o5Vuyu.jpg',
          alt: 'A group of bronze hippo sculptures emerging from the sett sidewalk as if they were swimming.'
        }];
        ```


## 3. 渲染和提交

## 4. 状态快照

## 5. 队列一系列状态更新

## 6. 更新状态中的对象

## 7. 更新状态数组

