---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:24:31.969Z'
title: Schnellstart
id: quick-start
---
# Schnellstart

Das grundlegende Solid-App-Beispiel, um mit dem TanStack Solid-Store zu beginnen.

```jsx
import { Store, useStore } from '@tanstack/solid-store';

// Sie können den Store auch außerhalb von Solid-Komponenten instanziieren!
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
    <h1>Wie viele deiner Freunde mögen Katzen oder Hunde?</h1>
    <p>
      Drücke einen der Buttons, um einen Zähler für die Anzahl deiner Freunde hinzuzufügen,
      die Katzen oder Hunde mögen
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
