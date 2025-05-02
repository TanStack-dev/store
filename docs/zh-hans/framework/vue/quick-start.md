---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-04-08T03:16:32.686Z'
title: 快速开始
id: quick-start
---
TanStack vue-store 快速入门的基础 Vue 应用示例。

**App.vue**
```html
<script setup>
import Increment from './Increment.vue';
import Display from './Display.vue';
</script>

<template>
  <h1>你的朋友中有多少人喜欢猫或狗？</h1>
  <p>点击按钮来统计喜欢猫或狗的朋友数量</p>
  <Increment animal="dogs" />
  <Display animal="dogs" />
  <Increment animal="cats" />
  <Display animal="cats" />
</template>
```

**store.js**
```js
import { Store } from '@tanstack/vue-store';

// 你也可以在 Vue 组件之外实例化存储 (Store)！
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

<!-- 只有当 `state[props.animal]` 变化时才会重新渲染。如果存储 (Store) 中无关的属性变化，不会触发重新渲染 -->
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
  <button @click="updateState(animal)">我的朋友喜欢 {{ animal }}</button>
</template>
```
