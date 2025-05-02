---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:26:25.935Z'
title: Быстрый старт
id: quick-start
---
# Быстрый старт

Базовый пример приложения на Solid для начала работы с TanStack Solid-store.

```jsx
import { Store, useStore } from '@tanstack/solid-store';

// Хранилище (Store) можно создать и вне компонентов Solid!
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
    <h1>Сколько ваших друзей любят кошек или собак?</h1>
    <p>
      Нажмите на одну из кнопок, чтобы увеличить счётчик друзей,
      которые любят кошек или собак
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
