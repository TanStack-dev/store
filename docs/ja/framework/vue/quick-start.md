---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T03:52:15.938Z'
title: クイックスタート
id: quick-start
---
# クイックスタート

TanStack Storeの基本的なVueアプリケーション例です。TanStack vue-storeを使い始めるためのサンプルです。

**App.vue**
```html
<script setup>
import Increment from './Increment.vue';
import Display from './Display.vue';
</script>

<template>
  <h1>あなたの友達は猫派？それとも犬派？</h1>
  <p>ボタンを押して、友達が猫または犬を好きな人数をカウントしましょう</p>
  <Increment animal="dogs" />
  <Display animal="dogs" />
  <Increment animal="cats" />
  <Display animal="cats" />
</template>
```

**store.js**
```js
import { Store } from '@tanstack/vue-store';

// Vueコンポーネントの外でもストアをインスタンス化できます！
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

<!-- このコンポーネントは`state[props.animal]`が変更された時のみ再レンダリングされます。関連のないストアプロパティが変更されても再レンダリングされません -->
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
  <button @click="updateState(animal)">私の友達は{{ animal }}が好き</button>
</template>
```
