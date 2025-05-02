---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:26:27.213Z'
title: Быстрый старт
id: quick-start
---
# Быстрый старт

Базовый пример React-приложения для начала работы с TanStack react-store.

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { Store, useStore } from "@tanstack/react-store";

// Вы можете создать хранилище (store) и вне React-компонентов!
export const store = new Store({
  dogs: 0,
  cats: 0,
});

// Этот компонент будет перерендериваться только при изменении `state[animal]`. 
// Если изменится несвязанное свойство хранилища, перерендера не произойдет

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
