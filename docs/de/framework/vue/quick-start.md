---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:24:36.970Z'
title: Schnellstart
id: quick-start
---
# Schnellstart

Das grundlegende Vue-App-Beispiel, um mit dem TanStack Vue-Store (Vue-Store) zu beginnen.

**App.vue**
```html
<script setup>
import Increment from './Increment.vue';
import Display from './Display.vue';
</script>

<template>
  <h1>Wie viele deiner Freunde mögen Katzen oder Hunde?</h1>
  <p>Drücke einen der Buttons, um einen Zähler für die Anzahl deiner Freunde hinzuzufügen, die Katzen oder Hunde mögen</p>
  <Increment animal="dogs" />
  <Display animal="dogs" />
  <Increment animal="cats" />
  <Display animal="cats" />
</template>
```

**store.js**
```js
import { Store } from '@tanstack/vue-store';

// Du kannst den Store auch außerhalb von Vue-Komponenten instanziieren!
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

<!-- Dies wird nur neu gerendert, wenn sich `state[props.animal]` ändert. Bei Änderungen einer nicht verknüpften Store-Eigenschaft erfolgt kein Re-Rendering -->
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
  <button @click="updateState(animal)">Mein Freund mag {{ animal }}</button>
</template>
```
