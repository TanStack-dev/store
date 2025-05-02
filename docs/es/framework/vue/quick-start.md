---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:23:43.190Z'
title: Inicio rápido
id: quick-start
---
# Inicio Rápido

Ejemplo básico de una aplicación Vue para comenzar con TanStack vue-store.

**App.vue**
```html
<script setup>
import Increment from './Increment.vue';
import Display from './Display.vue';
</script>

<template>
  <h1>¿A cuántos de tus amigos les gustan los gatos o los perros?</h1>
  <p>Presiona uno de los botones para agregar un contador de cuántos de tus amigos prefieren gatos o perros</p>
  <Increment animal="dogs" />
  <Display animal="dogs" />
  <Increment animal="cats" />
  <Display animal="cats" />
</template>
```

**store.js**
```js
import { Store } from '@tanstack/vue-store';

// ¡También puede instanciar el store fuera de los componentes de Vue!
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

<!-- Esto solo se volverá a renderizar cuando `state[props.animal]` cambie. Si una propiedad no relacionada del store cambia, no se volverá a renderizar -->
<template>
  <div>{{ animal }}: {{ count }}</div>
</template>
```

**Increment.vue**
```html
<script setup>
import { store, updateState } from './store';

const props = defineProps({ animal: String });
</script>

<template>
  <button @click="updateState(animal)">A mi amigo le gustan los {{ animal }}</button>
</template>
```
