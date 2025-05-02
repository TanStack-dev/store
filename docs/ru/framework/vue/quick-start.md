---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:26:31.063Z'
title: Быстрый старт
id: quick-start
---
Базовый пример приложения на Vue для начала работы с TanStack vue-store.

**App.vue**
```html
<script setup>
import Increment from './Increment.vue';
import Display from './Display.vue';
</script>

<template>
  <h1>Сколько ваших друзей любят кошек или собак?</h1>
  <p>Нажмите одну из кнопок, чтобы увеличить счетчик друзей, которые любят кошек или собак</p>
  <Increment animal="dogs" />
  <Display animal="dogs" />
  <Increment animal="cats" />
  <Display animal="cats" />
</template>
```

**store.js**
```js
import { Store } from '@tanstack/vue-store';

// Вы можете создать экземпляр хранилища (Store) и вне компонентов Vue!
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

<!-- Этот компонент будет перерендериваться только при изменении `state[props.animal]`. Если изменится несвязанное свойство хранилища, перерендера не произойдет -->
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
  <button @click="updateState(animal)">Мой друг любит {{ animal }}</button>
</template>
```
