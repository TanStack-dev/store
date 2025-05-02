---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:25:30.740Z'
title: Démarrage rapide
id: quick-start
---
# Démarrage rapide

Exemple d'application Solid de base pour commencer avec TanStack Solid-store.

```jsx
import { Store, useStore } from '@tanstack/solid-store';

// Vous pouvez instancier le store en dehors des composants Solid aussi !
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
      Incrémenter
    </button>
  )
}

const App = () => {
  return (
    <div>
    <h1>Combien de vos amis aiment les chats ou les chiens ?</h1>
    <p>
      Appuyez sur l'un des boutons pour ajouter un compteur du nombre de vos amis
      qui aiment les chats ou les chiens
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
