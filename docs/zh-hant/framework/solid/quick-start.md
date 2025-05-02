---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T03:44:20.688Z'
title: 快速開始 (Solid)
id: quick-start
---
# 快速開始

這是使用 TanStack Solid-store 的基本 Solid 應用範例。

```jsx
import { Store, useStore } from '@tanstack/solid-store';

// 你也可以在 Solid 元件之外實例化 store！
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
      Increment
    </button>
  )
}

const App = () => {
  return (
    <div>
    <h1>你的朋友有多少人喜歡貓或狗？</h1>
    <p>
      按下任一按鈕來增加計數器，記錄你朋友喜歡貓或狗的數量
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
