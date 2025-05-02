---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T03:52:21.547Z'
title: クイックスタート
id: quick-start
---
# クイックスタート

TanStack Storeはフレームワークに依存しないデータストアで、React、Solid、Vue、Angular、Svelteなどの主要フレームワーク向けのアダプターを提供しています。TanStack Storeは主に、フレームワークに依存しないTanStackライブラリの内部状態管理に使用されますが、スタンドアロンライブラリとしても利用可能です。

以下はTanStack react-storeを使い始めるための基本的なReactアプリの例です。

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { Store, useStore } from "@tanstack/react-store";

// Reactコンポーネントの外でもストアをインスタンス化できます！
export const store = new Store({
  dogs: 0,
  cats: 0,
});

// これは`state[animal]`が変更された時のみ再レンダリングされます。関係ないストアプロパティが変更されても再レンダリングされません

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
      <h1>How many of your friends like cats or dogs?</h1>
      <p>
        Press one of the buttons to add a counter of how many of your friends
        like cats or dogs
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
