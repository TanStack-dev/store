---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-04-08T03:15:54.652Z'
title: 快速开始
id: quick-start
---
# 快速入门

这是一个使用 TanStack react-store 的基本 React 应用示例。

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { Store, useStore } from "@tanstack/react-store";

// 你也可以在 React 组件之外实例化存储 (Store)！
export const store = new Store({
  dogs: 0,
  cats: 0,
});

// 只有当 `state[animal]` 发生变化时才会重新渲染。如果存储 (Store) 中无关的属性发生变化，不会触发重新渲染

const Display = ({ animal }) => {
  const count = useStore(store, (state) => state[animal]);
  return <div>{`${animal}: ${count}`}</div>;
};

const updateState = (animal) => {
  store.setState((state) => {
    return {
      ...state,
      [animal]: state[animal] + 1,
    };
  });
};
const Increment = ({ animal }) => (
  <button onClick={() => updateState(animal)}>My Friend Likes {animal}</button>
);

function App() {
  return (
    <div>
      <h1>你的朋友中有多少人喜欢猫或狗？</h1>
      <p>
        点击其中一个按钮来统计你的朋友喜欢猫或狗的数量
      </p>
      <Increment animal="dogs" />
      <Display animal="dogs" />
      <Increment animal="cats" />
      <Display animal="cats" />
    </div>
  );
}

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);

```
