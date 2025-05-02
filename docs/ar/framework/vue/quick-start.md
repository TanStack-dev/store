---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:27:34.044Z'
title: بدء سريع
id: quick-start
---
مثال تطبيق Vue الأساسي للبدء مع TanStack vue-store.

**App.vue**
```html
<script setup>
import Increment from './Increment.vue';
import Display from './Display.vue';
</script>

<template>
  <h1>كم من أصدقائك يحبون القطط أو الكلاب؟</h1>
  <p>اضغط على أحد الأزرار لإضافة عداد لعدد أصدقائك الذين يحبون القطط أو الكلاب</p>
  <Increment animal="dogs" />
  <Display animal="dogs" />
  <Increment animal="cats" />
  <Display animal="cats" />
</template>
```

**store.js**
```js
import { Store } from '@tanstack/vue-store';

// يمكنك إنشاء المتخزن خارج مكونات Vue أيضًا!
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

<!-- هذا سيعيد التصيير فقط عند تغيير `state[props.animal]`. إذا تغيرت خاصية غير مرتبطة في المتخزن، لن يعيد التصيير -->
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
  <button @click="updateState(animal)">صديقي يحب {{ animal }}</button>
</template>
```
