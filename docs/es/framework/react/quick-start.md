---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:23:41.565Z'
title: Inicio rápido
id: quick-start
---
# Inicio Rápido

Ejemplo básico de una aplicación React para comenzar con TanStack react-store.

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { Store, useStore } from "@tanstack/react-store";

// ¡También puede instanciar el store fuera de los componentes de React!
export const store = new Store({
  dogs: 0,
  cats: 0,
});

// Esto solo se volverá a renderizar cuando `state[animal]` cambie. Si cambia una propiedad no relacionada del store, no se volverá a renderizar

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
      <h1>¿A cuántos de tus amigos les gustan los gatos o los perros?</h1>
      <p>
        Presione uno de los botones para agregar un contador de cuántos de sus amigos
        prefieren gatos o perros
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
