---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:25:32.548Z'
title: Démarrage rapide
id: quick-start
---
# Démarrage rapide

Un exemple d'application React basique pour commencer avec TanStack react-store.

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { Store, useStore } from "@tanstack/react-store";

// Vous pouvez aussi instancier le store en dehors des composants React !
export const store = new Store({
  dogs: 0,
  cats: 0,
});

// Ce composant ne se re-rendra que lorsque `state[animal]` change. Si une propriété non liée du store change, il ne se re-rendra pas

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
  <button onClick={() => updateState(animal)}>Mes amis aiment les {animal}</button>
);

function App() {
  return (
    <div>
      <h1>Combien de vos amis aiment les chats ou les chiens ?</h1>
      <p>
        Appuyez sur l'un des boutons pour ajouter un compteur du nombre de vos amis
        qui aiment les chats ou les chiens
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
