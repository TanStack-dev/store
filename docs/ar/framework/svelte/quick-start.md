---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:27:35.409Z'
title: بدء سريع
id: quick-start
---
# بدء سريع

مثال تطبيق أساسي باستخدام Svelte للبدء مع مخزن Svelte الخاص بـ TanStack.

**store.ts**
```ts
import { Store } from '@tanstack/svelte-store';

// يمكنك إنشاء المخزن خارج ملفات Svelte أيضًا!
export const store = new Store({
  dogs: 0,
  cats: 0,
});

export function updateState(animal: 'cats' | 'dogs') {
  store.setState((state) => {
    return {
      ...state,
      [animal]: state[animal] + 1,
    };
  });
}
```

**App.svelte**
```html
<script lang="ts">
	import Increment from "./Increment.svelte";
	import Display from "./Display.svelte";
</script>


<h1>كم عدد أصدقائك الذين يحبون القطط أو الكلاب؟</h1>
<p>اضغط على أحد الأزرار لإضافة عداد لعدد أصدقائك الذين يحبون القطط أو الكلاب</p>
<Increment animal="dogs" />
<Display animal="dogs" />
<Increment animal="cats" />
<Display animal="cats" />
```


**Display.svelte**
```html
<script lang="ts">
    import { useStore } from '@tanstack/svelte-store';
    import { store } from './store';
    
    const {animal}: { animal: 'cats' | 'dogs' } = $props()
    const count = useStore(store, (state) => state[animal]);
</script>
    
<!-- سيتم إعادة التصيير فقط عند تغيير `state[animal]`. إذا تغيرت خاصية غير مرتبطة في المخزن، لن يتم إعادة التصيير -->
<div>{ animal }: { count.current }</div>
```

**Increment.svelte**
```html
<script lang="ts">
    import { updateState } from './store';
    
    const { animal }: { animal: 'cats' | 'dogs' } = $props()
</script>
    
<button onclick={() => updateState(animal)}>صديقي يحب { animal }</button>
```
