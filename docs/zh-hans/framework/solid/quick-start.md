---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-04-08T03:16:44.593Z'
title: 快速开始
id: quick-start
---
# 快速开始

这是一个基础的 Solid 应用示例，用于开始使用 TanStack Solid-store。

```jsx
import { Store, useStore } from '@tanstack/solid-store';

// 你也可以在 Solid 组件之外实例化存储 (Store)！
export const store = new Store({
  cats: 0,
  dogs: 0
})

export const Display = (props) => {
  const count = useStore(store, (state) => state[props.animals]);
  return (
    <span>
      {props.animals}: {count()}
      </span>
    );
}

export const Button = (props) => {
  return (
    <button
      onClick={() => {
        store.setState((state) => {
          return {
            ...state,
            [props.animals]: state[props.animals] + 1
          }
        })
      }}
    >
      增加
    </button>
  )
}

const App = () => {
  return (
    <div>
    <h1>你的朋友们有多少喜欢猫或狗？</h1>
    <p>
      点击其中一个按钮来增加计数器，统计有多少朋友喜欢猫或狗
      </p>
      <Button animals="dogs" />
      <Display animals="dogs" />
      <Button animals="cats" />
      <Display animals="cats" />
  </div>
  );
};

export default App;
```
