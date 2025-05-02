---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T03:44:13.295Z'
title: 快速開始 (React)
id: quick-start
---
快速入門 (Quick Start)  

這是一個基礎的 React 應用範例，幫助你開始使用 TanStack React Store。  

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { Store, useStore } from "@tanstack/react-store";

// 你也可以在 React 元件之外實例化 store！
export const store = new Store({
  dogs: 0,
  cats: 0,
});

// 只有當 `state[animal]` 變更時才會重新渲染。如果 store 中不相關的屬性變更，不會觸發重新渲染

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
      <h1>你的朋友有多少人喜歡貓或狗？</h1>
      <p>
        按下其中一個按鈕，來計算你的朋友中有多少人喜歡貓或狗
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
