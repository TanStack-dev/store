---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T03:51:58.727Z'
title: クイックスタート
id: quick-start
---
# クイックスタート

TanStack Solid-storeを使い始めるための基本的なSolidアプリの例です。

```jsx
import { Store, useStore } from '@tanstack/solid-store';

// Solidコンポーネントの外でもストアをインスタンス化できます！
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
    <h1>あなたの友達は猫派？それとも犬派？</h1>
    <p>
      ボタンを押して、猫または犬が好きな友達の数をカウントしましょう
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
