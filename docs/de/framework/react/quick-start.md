---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:24:34.807Z'
title: Schnellstart
id: quick-start
---
# Schnellstart

Das grundlegende React-App-Beispiel, um mit dem TanStack React-Store (React-Store) zu beginnen.

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { Store, useStore } from "@tanstack/react-store";

// Sie können den Store auch außerhalb von React-Komponenten instanziieren!
export const store = new Store({
  dogs: 0,
  cats: 0,
});

// Dies wird nur neu gerendert, wenn sich `state[animal]` ändert. Wenn sich eine nicht verknüpfte Store-Eigenschaft ändert, erfolgt kein Re-Rendering.

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
      <h1>Wie viele deiner Freunde mögen Katzen oder Hunde?</h1>
      <p>
        Drücke einen der Buttons, um einen Zähler hinzuzufügen, der zeigt, wie viele deiner Freunde Katzen oder Hunde mögen.
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
