---
title: "CS571-S23 学习笔记(五) : React 2"
date: 2023-08-28T00:00:01+08:00
description: "CS571 S23 旨在向CS本科生介绍UX开发的精彩世界，这是我在这门课的学习笔记。本节关键词：框架（Framework）,库（Library）
,useRef"
categories: [前端学习]
tags: [JavaScript, CS571, 前端, React]
draft: false
---

## 框架和库的区别

### 框架（Framework）

框架是一个完整的解决方案，它提供了一整套工具、库和规范，用于简化应用程序的开发过程。框架通常具有一定的结构 和约束，开发者需要按照框架的规则进行开发。框架通常涵盖了应用程序的架构、数据流管理、路由、状态管理、UI 组件等核心功能。流行的 JavaScript 框架有 React、Angular 和 Vue.js。

### 库（Library）

库是一组可复用的代码集合，用于实现特定的功能，开发者可以根据需要选择性地使用库中的功能。库通常提供了一些函数、类或模块，以便开发者可以在自己的代码中调用它们。库通常不限制应用程序的整体结构，开发者可以根据自己的需求进行自由组织。常见的 JavaScript 库有 jQuery、Lodash 和 Moment.js。

简而言之，框架更像是一个完整的开发工具包，提供了一种开发应用程序的方式，而库则是提供了一些特定功能的代码集合，可以根据需要选择性地使用。使用框架可以加快开发速度，但需要遵循框架的规则和约束；而使用库可以灵活地选择需要的功能，但需要自己组织代码结构。

## useRef

在 React 中，`useRef` 是一个用于创建可变引用的 Hook。它可以用来引用组件中的 DOM 元素、保存可变值以及在组件生命周期之间共享数据。

使用 `useRef`，你可以在函数组件中创建一个引用，并且该引用在组件重新渲染时保持不变。这使得你可以在函数组件中访问和操作 DOM 元素。

下面是一个使用 `useRef` 引用 DOM 元素的示例：

```jsx
import React, { useRef, useEffect } from "react";

function MyComponent() {
  const myRef = useRef(null);

  useEffect(() => {
    // 在组件挂载后，访问和操作 DOM 元素
    console.log(myRef.current); // 输出 DOM 元素
    myRef.current.focus(); // 聚焦到 DOM 元素
  }, []);

  return (
    <div>
      <input type="text" ref={myRef} />
    </div>
  );
}
```

在上面的代码中，我们使用 `useRef(null)` 创建了一个引用 `myRef`，并将其赋值给 `<input>` 元素的 `ref` 属性。然后，通过 `myRef.current`，我们可以访问到该 DOM 元素，并对其执行操作。在上述示例中，我们在组件挂载后使用 `useEffect` 钩子来访问和操作 DOM 元素。

除了用于引用 DOM 元素，`useRef` 也可以用于存储任意可变值，并在组件重新渲染时保持不变。这意味着，当你修改 `useRef` 的 `.current` 属性时，组件不会重新渲染。

下面是一个使用 `useRef` 存储可变值的示例：

```jsx
import React, { useRef, useEffect } from "react";

function MyComponent() {
  const countRef = useRef(0);

  useEffect(() => {
    console.log(countRef.current); // 输出当前计数值
  });

  const incrementCount = () => {
    countRef.current += 1;
  };

  return (
    <div>
      <button onClick={incrementCount}>Increment</button>
    </div>
  );
}
```

在上面的代码中，我们使用 `useRef(0)` 创建了一个引用 `countRef`，并将其初始化为 0。在 `incrementCount` 函数中，我们通过修改 `countRef.current` 的值来递增计数。每次组件重新渲染时，我们都可以通过 `countRef.current` 获取当前计数值，而不会触发重新渲染。
