---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T03:44:35.220Z'
title: 快速開始 (Vue)
id: quick-start
---
# 快速開始

這是一個使用 TanStack Vue 儲存庫 (vue-store) 的基本 Vue 應用範例。

**App.vue**
```html
<script setup>
import Increment from './Increment.vue';
import Display from './Display.vue';
</script>

<template>
  <h1>你的朋友中有多少人喜歡貓或狗？</h1>
  <p>按下按鈕來增加喜歡貓或狗的朋友計數器</p>
  <Increment animal="dogs" />
  <Display animal="dogs" />
  <Increment animal="cats" />
  <Display animal="cats" />
</template>
```

**store.js**
```js
import { Store } from '@tanstack/vue-store';

// 你也可以在 Vue 元件外部實例化儲存庫！
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

<!-- 這只會在 `state[props.animal]` 改變時重新渲染。如果儲存庫中不相關的屬性改變，它不會重新渲染 -->
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
  <button @click="updateState(animal)">我的朋友喜歡 {{ animal }}</button>
</template>
```
