---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:25:34.782Z'
title: Démarrage rapide
id: quick-start
---
# Démarrage rapide

Un exemple d'application Vue de base pour commencer avec TanStack vue-store.

**App.vue**
```html
<script setup>
import Increment from './Increment.vue';
import Display from './Display.vue';
</script>

<template>
  <h1>Combien de vos amis aiment les chats ou les chiens ?</h1>
  <p>Appuyez sur l'un des boutons pour ajouter un compteur du nombre de vos amis qui aiment les chats ou les chiens</p>
  <Increment animal="dogs" />
  <Display animal="dogs" />
  <Increment animal="cats" />
  <Display animal="cats" />
</template>
```

**store.js**
```js
import { Store } from '@tanstack/vue-store';

// Vous pouvez instancier le store en dehors des composants Vue aussi !
export const store = new Store({
  dogs: 0,
  cats: 0,
});

export function updateState(animal) {
  store.setState((state) => {
    return {
      ...state,
      [animal]: state[animal] + 1,
    };
  });
}
```

**Display.vue**
```html
<script setup>
import { useStore } from '@tanstack/vue-store';
import { store } from './store';

const props = defineProps({ animal: String });
const count = useStore(store, (state) => state[props.animal]);
</script>

<!-- Ce composant ne se re-rendra que lorsque `state[props.animal]` change. Si une propriété non liée du store change, il ne se re-rendra pas -->
<template>
  <div>{{ animal }} : {{ count }}</div>
</template>
```

**Increment.vue**
```html
<script setup>
import { store, updateState } from './store';

const props = defineProps({ animal: String });
</script>

<template>
  <button @click="updateState(animal)">Mon ami aime les {{ animal }}</button>
</template>
```
