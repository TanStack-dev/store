---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:23:39.734Z'
title: Inicio rápido
id: quick-start
---
# Inicio Rápido

Ejemplo básico de una aplicación en Solid para comenzar con TanStack Solid-store.

```jsx
import { Store, useStore } from '@tanstack/solid-store';

// ¡Puede instanciar el almacén (store) fuera de los componentes de Solid también!
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
      Incrementar
    </button>
  )
}

const App = () => {
  return (
    <div>
    <h1>¿A cuántos de tus amigos les gustan los gatos o los perros?</h1>
    <p>
      Presione uno de los botones para agregar un contador de cuántos de sus amigos
      prefieren gatos o perros
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
