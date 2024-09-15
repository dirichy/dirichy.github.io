---
layout: post
title: react jsx
subtitle: react short
date: 2019-2-27 12:03:16
author: gankai
header-img: img/react-perf.webp
catalog: true
tags:
  - react
---

React 是最流行的前端框架之一，除了由 Facebook 提供资金外，它还围绕一些关键概念（单向数据流、不可变数据、函数组件、hooks）构建，这使得创建强大的应用程序比以往任何时候都更加容易。也就是说，它并不是没有陷阱。

在 React 中编写低效的代码很容易，无用的重新渲染是经常发生。通常，你从一个简单的应用程序开始，然后逐渐在其之上开发功能。起初，应用程序小到足以使低效率不明显，但随着复杂性的增加，组件层次结构也会增加，因此重新渲染的次数也会增加。然后，一旦应用程序速度变得无法忍受（根据你的标准），你就开始分析和优化有问题的区域。

在本文中，我们将讨论列表的优化过程，列表是 React 中臭名昭著的性能问题来源。这些技术中的大多数都适用于 React 和 React Native 应用程序。

## 从一个有问题的例子开始

![image.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a4a4a6d6f9b446b4a777ad7e8441aa6d~tplv-k3u1fbpfcp-zoom-1.image)

示例是一个简单的可选项目列表，但存在一些性能问题。单击一个项目会切换选择状态，但操作明显滞后。我们的目标是让选择感觉很快。

```js
import { useState } from "react";

// 创建模拟数据
const data = new Array(100)
  .fill()
  .map((_, i) => i + 1)
  .map((n) => ({
    id: n,
    name: `Item ${n}`,
  }));

export default function App() {
  // 包含所选项的数组
  const [selected, setSelected] = useState([]);

  const toggleItem = (item) => {
    if (!selected.includes(item)) {
      setSelected([...selected, item]);
    } else {
      setSelected(selected.filter((current) => current !== item));
    }
  };

  return (
    <div className="App">
      <h1>List Example</h1>
      <List data={data} selectedItems={selected} toggleItem={toggleItem} />
    </div>
  );
}

const List = ({ data, selectedItems, toggleItem }) => {
  return (
    <ul>
      {data.map((item) => (
        <ListItem
          name={item.name}
          selected={selectedItems.includes(item)}
          onClick={() => toggleItem(item)}
        />
      ))}
    </ul>
  );
};

const ListItem = ({ name, selected, onClick }) => {
  // 运行昂贵的操作来模拟负载,在现实世界的JS应用程序中也可以有其他的相关业务代码
  expensiveOperation(selected);

  return (
    <li
      style={selected ? { textDecoration: "line-through" } : undefined}
      onClick={onClick}
    >
      {name}
    </li>
  );
};

// 这是一个昂贵的 JS 操作示例，我们可能会在渲染函数中执行该操作以模拟负载。
const expensiveOperation = (selected) => {
  // 这里我们使用 selected 只是因为我们想模拟一个依赖于 props 的操作
  let total = selected ? 1 : 0;
  for (let i = 0; i < 200000; i++) {
    total += Math.random();
  }
  return total;
};
```

## 缺少关键 props

我们可以从控制台注意到的第一件事是`key`在渲染列表项时我们没有传递 prop。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/14560591ce6847b6af0a7e4fb6bad81c~tplv-k3u1fbpfcp-watermark.image?)

这是由下面代码引起的 👇：

```jsx
{
  data.map((item) => (
    <ListItem
      name={item.name}
      selected={selectedItems.includes(item)}
      onClick={() => toggleItem(item)}
    />
  ));
}
```

正如你可能已经知道的那样，`key` 对于动态列表在 React 中至关重要，因为它有助于框架识别哪些项目已更改、添加或删除。

一般的初学者通常是通过传递每项的 index 来解决问题：

```jsx
{
  data.map((item, index) => (
    <ListItem
      key={index}
      name={item.name}
      selected={selectedItems.includes(item)}
      onClick={() => toggleItem(item)}
    />
  ));
}
```

尽管适用于简单的用例，但当列表是动态的添加或删除项目时，这种方法会导致多种意外行为。例如，如果你在索引 N 处删除列表中间的项目，则位于位置 N+1 的所有列表项目现在将具有不同的 key。这会导致 React “混淆”哪个映射组件属于哪个项目。如果你想了解更多关于使用索引作为键的潜在陷阱，[这篇文章](https://robinpokorny.medium.com/index-as-a-key-is-an-anti-pattern-e0349aece318)是一个很好的资源。

因此，你应该为每一项指定一个唯一的 key，如果你收到的数据来自后端，你也许可以使用数据库的唯一 ID 作为键。否则，也可以在创建项目时使用[nanoid](https://www.npmjs.com/package/nanoid)生成客户端随机 id 。

幸运的是，我们自己的每个项目都有自己的 id 属性，所以我们应该下面这种方式处理它：

```jsx
{
  data.map((item) => (
    <ListItem
      key={item.id}
      name={item.name}
      selected={selectedItems.includes(item)}
      onClick={() => toggleItem(item)}
    />
  ));
}
```

添加完了 key 解决了之前的警告，就可以真正开始分析啦。

## 分析列表

现在我们解决了`key`警告，我们准备解决性能问题。在这个阶段，使用分析器可以帮助追踪慢速区域，从而指导我们的优化，这就是我们要做的。

使用 React 时，你可以使用两种主要的分析器：浏览器的内置分析器，例如 Chrome 的开发工具中可用的分析器，以及由 React DevTools 扩展提供的分析器。它们在不同的场景中都很有用。根据我的经验，React DevTools 的分析器是一个很好的起点，因为它为开发者提供了一个组件感知的性能表示，这有助于追踪导致问题的特定组件，而浏览器的分析器工作在较低级别，并且它在性能问题与组件没有直接关系的情况下非常有用，例如，由于方法缓慢或 Redux reducer。

出于这个原因，我们将从 React DevTools 的分析器开始，因此请确保安装了扩展。然后，我们可以从 Chrome 的开发工具 > Profiler 访问 Profiler 工具。在开始之前，我们将设置两个有助于优化过程的设置：

- 在 Chrome 的性能选项卡中，将 CPU 节流设置为 x6。这将模拟较慢的 CPU，使减速更加明显。

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eb00bc0dbda04ca89feb81a7f16f606f~tplv-k3u1fbpfcp-watermark.image?)

- 在 React DevTools Profiler 选项卡中，单击齿轮图标 > Profiler > Record why each component rendered while profiling。这将帮助我们追踪无用重新渲染的原因。

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cf62c2878d5a4431bc1a9076a0be0ef2~tplv-k3u1fbpfcp-watermark.image?)

配置完成后，我们就可以分析我们的示例 todo 应用程序了。继续并单击“记录”按钮，然后选择列表中的一些项目，最后单击“停止记录”。这是我们选择 3 个项目后得到的结果：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/60f5421133964b9e9285d8445960b1c6~tplv-k3u1fbpfcp-watermark.image?)

在右上角，你会看到以红色突出显示的提交，简而言之，它们是导致 DOM 更新的渲染。如你所见，当前提交花费了 2671 毫秒来渲染。通过将鼠标悬停在各种元素上，我们可以看出大部分时间都花在了渲染列表项上，平均每个项需要 26 毫秒。

花费 26 毫秒渲染单个项目本身并不是坏事。只要整个操作花费的时间少于 100 毫秒，用户仍然会认为该操作是敏捷的。我们最大的问题是选择单个项目会导致所有项目重新渲染，这就是我们将接下来需要解决的问题。

> 此时我们应该问自己的一个问题是：在一个动作之后重新渲染的预期项目数量是多少？在这种特殊情况下，答案是一个，因为单击的结果是选择了一个新项目，而其他项目都没有受到影响。另一种情况可能是单选列表，其中在任何给定时间最多可以选择一个项目。在这种情况下，单击一个项目会导致重新渲染两个项目，因为我们需要同时渲染选定的一个和未选择的一个。

## 使用 React.memo 防止重新渲染

在上面我们讨论了选择单个项目如何导致整个列表重新呈现，理想情况下，我们只想重新渲染单击受新选择影响的这项。我们可以使用[React.memo](https://reactjs.org/docs/react-api.html#reactmemo)高阶组件来做到这一点。

简而言之，`React.memo`将新 props 与旧 props 进行比较，如果它们相等，它会重用以前的渲染。否则，如果 props 不同，它会重新渲染组件。
需要注意的是，React 执行的是 props 的**浅比较**，在将对象和方法作为 props 传递时必须考虑到这一点。

你也可以覆盖比较函数，但我建议不要这样做，因为它会使代码更难维护（稍后会详细介绍）。

现在我们了解了`React.memo`，让我们来创建另一个组件`ListItem`：

```jsx
import { memo } from "react";

const MemoizedListItem = memo(ListItem);
```

我们现在可以在列表中使用`MemoizedListItem`而不是：`ListItem`

```jsx
{
  data.map((item) => (
    <MemoizedListItem
      key={item.id}
      name={item.name}
      selected={selectedItems.includes(item)}
      onClick={() => toggleItem(item)}
    />
  ));
}
```

好的！我们现在已经使用 memo 处理了`ListItem`.  如果你继续尝试点击切换选择，你会发现有问题--那就是仍然很慢！

如果我们像以前一样打开分析器并记录一个选择，我们应该会看到如下内容：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/141214637cfd45c9be11358e23b7ac51~tplv-k3u1fbpfcp-watermark.image?)

如你所见，**仍在重新渲染所有项目**！为什么会这样？

如果你将鼠标悬停在其中一个列表项上，你会看到“why did this reander？”  部分。在我们的例子中，它表示`Props changed: (onClick)`，这意味着由于我们传递给每个项目的回调，我们的项目正在重新渲染。`onClick`

正如我们之前所讨论的，默认情况下`React.memo`会对 props 进行*浅层比较*。这基本上意味着在每个 props 上调用严格的相等运算符`===`,在我们的例子中大致相当于：

```js
function arePropsEqual(prevProps, nextProps) {
  return (
    prevProps.name === nextProps.name &&
    prevProps.selected === nextProps.selected &&
    prevProps.onClick === nextProps.onClick
  );
}
```

而`name`和`selected`按*值*比较（因为它们是原始类型，分别是字符串和布尔值），  按*引用*`onClick`比较（作为函数）。  当我们创建列表项时，我们将回调作为匿名闭包传递：

`onClick`

```jsx
onClick={() => toggleItem(item)}
```

> 每次列表重新渲染时，**每个项目都会收到一个新的回调函数**从严格相等的角度来看，回调*已经改变*，因此`MemoizedListItem`被重新渲染。如果你仍然不清楚相等性方面，请继续并在浏览器中打开 JS 控制台。如果你键入`true === true`，你会注意到结果是`true`.但是如果你输入`(() => {}) === (() => {})`，你会得到`false`结果。那是因为两个函数只有当它们共享相同的身份时才相等，并且每次我们创建一个新的闭包时，我们都会生成一个新的身份。

因此，我们需要一种方法来保持回调的身份`onClick`稳定，以防止无用的重新渲染，这就是我们接下来需要讨论的内容。

### 常见的处理方式

在讨论建议的解决方案之前，让我们分析一下在这些情况下使用的常见处理方式，鉴于该`React.memo`方法接受自定义函数比较器，你可能会想不比较`onClick`类似于以下内容：

```jsx
const MemoizedListItem = memo(
  ListItem,
  (prevProps, nextProps) =>
    prevProps.name === nextProps.name &&
    prevProps.selected === nextProps.selected
  // 不比较 onClick props
);
```

在这种情况下，即使使用更改的`onClick`回调，列表项也不会重新呈现，除非`name`或`selected`更新。
如果你继续尝试这种方法，你会注意到列表现在感觉很灵巧，但有些地方是错误的，当多次来回点击选择几个项目的时候就有问题，item 被随机选择和取消。

发生这种情况是因为 **`toggleItem`函数不是纯函数**，因为它取决于`selected`项目的先前值。如果在`React.memo`的回调检查函数中不检查`onClick`的更新，那么这个组件可能会收到过时（陈旧）版本的回调，从而导致发生这些故障。

在这种特殊情况下，`toggleItem`实现的方式并不是最优的，我们可以轻松地将其转换为纯函数，但我的观点是：**通过从`memo`比较器中排除比较`onClick`回调的做法，将会使得应用程序暴露于细微的过时错误问题**。

有些人可能会争辩说，只要`onClick`回调保持*pure*，那么这种方法是完全可以接受的。就我个人而言，我认为这是也不好，原因有两个：

- 在复杂的代码库中，将纯函数错误地转换为非纯函数相对容易。
- 通过编写自定义比较器，会增加额外的维护负担。如果将来`ListItem`需要接受另一个参数怎么办？比如新增一个`color`属性，然后你就需要重构`memo`的比较器，如下所示。如果你忘记添加它（这在具有多人合作的复杂代码中相对容易发生），那么刚才的问题又出现了 。

```jsx
const MemoizedListItem = memo(
  ListItem,
  (prevProps, nextProps) =>
    prevProps.name === nextProps.name &&
    prevProps.selected === nextProps.selected &&
    prevProps.color === nextProps.color // 假设新增color属性，但是忘记加如比较呢？？？
);
```

如果不建议使用自定义比较器，那么我们应该如何解决这个问题呢？

### 使`onClick`回调函数稳定不变

我们的目标是使用`React.memo`没有自定义比较器的“基础”版本。\
选择这条路既可以提高组件的可维护性，也可以提高它对未来变化的鲁棒性。\
但是，为了使 memoization 正常工作，我们需要**重构 onClick 回调以保持其稳定不变**，否则\
执行的相等性检查`React.memo`将阻止 memoization。

在 React 中保持函数身份稳定的传统方法是使用`useCallback`钩子。\
钩子接受一个函数和一个依赖数组，只要依赖不会改变，回调的身份也不会改变。\
让我们重构我们的示例以使用`useCallback`：

我们的第一个尝试是将匿名闭包移动到`() => toggleItem(item)`一个单独的方法内部`useCallback`：

```jsx
const List = ({ data, selectedItems, toggleItem }) => {
  const handleClick = useCallback(() => {
    toggleItem(??????) // 我们如何获得item?
  }, [toggleItem])

  return (
    <ul>
      {data.map((item) => (
        <MemoizedListItem
          key={item.id}
          name={item.name}
          selected={selectedItems.includes(item)}
          onClick={handleClick}
        />
      ))}
    </ul>
  );
};
```

我们现在面临一个问题：以前使用匿名闭包的形式，`item`在`.map`迭代中可以获取到，然后在`toggleItem`函数执行的时候，item 作为参数传递给函数。但是现在这样的情况如何将 item 作为`handleClick`的参数传入呢？

让我们讨论一个可能的解决方案：

### 重构 ListItem 组件

目前，`ListItem`的`onClick`回调不提供有关被选中项目的任何信息。\
如果是这样，我们将能够轻松解决此问题，因此让我们重构`ListItem`和`List`组件以提供此信息。

首先，我们将`ListItem`组件更改为接受完整的`item`对象，并且鉴于`name`prop 现在是多余的，我们将其删除。\
然后，我们为`onClick`事件引入一个处理程序以提供`item`作为参数。这是我们的最终结果：\

```jsx
const ListItem = ({ item, selected, onClick }) => {
  // 运行昂贵的操作来模拟负载,在现实世界的JS应用程序中也可以有其他的相关业务代码
  expensiveOperation(selected);

  return (
    <li
      style={selected ? { textDecoration: "line-through" } : undefined}
      onClick={() => onClick(item)}
    >
      {item.name}
    </li>
  );
};
```

如你所见，`onClick`现在提供当前 item 作为参数。

> 此时有人可能会问，在代码中`li`标签上的`onClick`使用了匿名闭包，我们不应该避免这样做以防止重新渲染吗？\
> 虽然我们*可以*使用`useCallback`在组件内部创建另一个 memoized 回调来处理`ListItem`的单击事件，但在这种情况下不会提供任何性能改进。\
> 我们前面讨论的匿名闭包的问题是`List`它破坏了`React.memo`中的`MemoizedListItem`.  鉴于我们*没有*memo 该`li`元素，因此无论 li 上面的 onClick 回调是不是最新的都不会带来什么很大的性能上的优势。

然后，我们可以重构`List`组件以传递`item`prop，而不是在回调中`name`使用新可用的`item`信息：`handleClick`

```jsx
const List = ({ data, selectedItems, toggleItem }) => {
  const handleClick = useCallback(
    (item) => {
      // We now receive the selected item
      toggleItem(item);
    },
    [toggleItem]
  );

  return (
    <ul>
      {data.map((item) => (
        <MemoizedListItem
          key={item.id}
          item={item} // We pass the full item instead of the name
          selected={selectedItems.includes(item)}
          onClick={handleClick}
        />
      ))}
    </ul>
  );
};
```

好的！让我们继续尝试重构的版本：
![image.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/47199d398fa1457d8a450d3f9ee9e8fc~tplv-k3u1fbpfcp-zoom-1.image)

生效啦，但它仍然很慢！如果我们打开分析器，我们可以看到整个列表仍在渲染中：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f5e7cd59f2bb4e59895f9211d6d4cb0a~tplv-k3u1fbpfcp-watermark.image?)

从 profiler 中可以看出，`onClick`身份仍然在变化！这意味着我们的`handleClick`身份在每次重新渲染时都会改变。

### 另一种常见的问题

在深入研究正确的解决方案之前，让我们讨论一下在这些情况下使用的常见的问题。\
鉴于`useCallback`接受依赖数组，你可能会想指定一个空数组以保持 handleClick 不变：

```jsx
const handleClick = useCallback((item) => {
  toggleItem(item);
}, []);
```

尽管保持不变，**但这种方法也存在我们在前几节中讨论的相同过时错误**。\
如果我们运行它，你会注意到当我们指定自定义比较器时，这些项目会被取消选择：
![image/gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/84fa0b862a7f4c7fa0e61056e2ef6b1a~tplv-k3u1fbpfcp-zoom-1.image)

通常，你应该始终在`useCallback`中指定正确的依赖项，否则，你   会将应用程序暴露于潜在的难以调试的陈旧错误。`useEffect,useMemo`

## [](https://dev.to/federicoterzi/optimizing-lists-in-react-solving-performance-problems-and-anti-patterns-2ph4#solving-the-toggleitem-identity-problem)解决 toggleItem 标识问题

正如我们之前讨论过的，我们的`handleClick`回调的问题在于它的`toggleItem`依赖标识在每次渲染时都会发生变化，从而导致它也重新渲染：

```jsx
const handleClick = useCallback(
  (item) => {
    toggleItem(item);
  },
  [toggleItem]
);
```

我们的第一次尝试是像`handleClick`一样使用 `useCallback`处理：

```jsx
const toggleItem = useCallback(
  (item) => {
    if (!selected.includes(item)) {
      setSelected([...selected, item]);
    } else {
      setSelected(selected.filter((current) => current !== item));
    }
  },
  [selected]
);
```

但这并*不能解决*问题，因为这个回调依赖于外部状态变量`selected`，每次`setSelected`调用都会改变。如果我们希望它保持不变，我们需要一种方法来使`toggleItem`是纯的。幸运的是，我们可以使用[`useState`的功能更新](https://reactjs.org/docs/hooks-reference.html#functional-updates)来实现我们的目标：

```jsx
const toggleItem = useCallback((item) => {
  setSelected((prevSelected) => {
    if (!prevSelected.includes(item)) {
      return [...prevSelected, item];
    } else {
      return prevSelected.filter((current) => current !== item);
    }
  });
}, []);
```

如你所见，我们将之前的逻辑包装在`setSelected`调用中，这反过来又提供了我们需要计算新选定项的先前状态值。

如果我们继续运行重构后的示例，它可以运行而且也很敏捷！我们还可以运行通常的分析器来了解正在发生的事情：

悬停在正在渲染的项目上：\
[![固定反应示例](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a9be74931eb847539f04ac3820271b97~tplv-k3u1fbpfcp-zoom-1.image)](https://res.cloudinary.com/practicaldev/image/fetch/s--FL8_1dPv--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xibodyyoi3ag665qepsu.png)

悬停在其他项目上：\
[![修复反应示例 2](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/59571541aa5a483ab9168c31afcba4f8~tplv-k3u1fbpfcp-zoom-1.image)](https://res.cloudinary.com/practicaldev/image/fetch/s--NpaP2lQs--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xxmzjmkub7b9utuf6mb6.png)

如你所见，在选择一个项目后，我们只渲染当前正在选择的项目，而其他项目正在 memo 中。

### 关于函数状态更新的说明

在我们刚刚讨论的示例中，将我们的`toggleItem`方法转换为函数模式`useState`是相对简单的。\
在现实世界的场景中，事情可能并不那么简单。

例如，你的函数可能依赖于多个状态值：

```jsx
const [selected, setSelected] = useState([]);
const [isEnabled, setEnabled] = useState(false);

const toggleItem = useCallback(
  (item) => {
    // 假如仅在启用时，才允许切换项目，我们该怎么办？
    if (isEnabled) {
      setSelected((prevSelected) => {
        if (!prevSelected.includes(item)) {
          return [...prevSelected, item];
        } else {
          return prevSelected.filter((current) => current !== item);
        }
      });
    }
  },
  [isEnabled]
);
```

每次`isEnabled`值改变时，你的`toggleItem`身份也会改变。\
在这些情况下，你应该将两个子状态合并到同一个`useState`调用中，或者使用[`useReducer`](https://reactjs.org/docs/hooks-reference.html#additional-hooks)更好地将其转换为一个状态。\
鉴于`useReducer`的`dispatch`函数具有稳定的恒等式，你可以将此方法扩展到复杂状态。\
此外，这同样适用于[Redux](https://redux.js.org/)的`dispatch`函数，因此你可以在 Redux 级别移动项目切换逻辑并将我们的`toggleItem`函数转换为：

```jsx
const dispatch = useDispatch();

// 鉴于dispatch是稳定的，所以`toggleItem` 也将是稳定的
const toggleItem = useCallback(
  (item) => {
    dispatch(toggleItemAction(item));
  },
  [dispatch]
);
```

## 虚拟化列表？

在结束本文之前，我想简要说一下*列表虚拟化*，这是一种用于提高长列表性能的常用技术。\
简而言之，列表虚拟化基于仅呈现给定列表中项目的子集（通常是当前可见的项目）并推迟其他项目的想法。\
例如，如果您有一个包含一千个项目的列表，但在任何给定时间只有 10 个可见，那么我们可能只先渲染这 10 个，而其他的可以在需要时*按需渲染*（即滚动后）。

与呈现整个列表相比，列表虚拟化提供了两个主要优势：

- 更快的初始启动时间，因为我们只需要渲染列表的一个子集
- 较低的内存使用率，因为在任何给定时间仅呈现项目的子集

也就是说，列表虚拟化不是我们需要一直使用的灵丹妙药，因为它会增加复杂性并且可能会出现故障。\
就个人而言，如果你只处理数百个 item，我会避免使用虚拟列表，因为我们在本文中讨论的 memo 技术通常足够有效（较旧的移动设备可能需要较低的阈值）。与往常一样，正确的方法取决于特定的用例，因此我强烈建议大家在深入研究更复杂的优化技术之前先分析您的列表。

## 结论

在本文中，我们深入介绍了列表优化。我们从一个有问题的例子开始，逐步解决了大部分的性能问题。\
我们还讨论了开发中应该注意的一些问题，以及解决它们的潜在方法。

总之，列表通常是 React 中性能问题的原因，因为默认情况下每次更改时都会重新渲染所有项目。\
`React.memo`是缓解问题的有效工具，但你可能需要重构应用程序以使 props 的保持不变。

如果大家有兴趣可以在[CodeSandbox](https://codesandbox.io/s/commonreactlistmistakes-solved-forked-do9viy?file=/src/App.js)中找到最终代码。

PS：`useMemo`在我们的例子中还有一个小的优化要添加，你能自己发现吗？
