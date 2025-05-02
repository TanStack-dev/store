---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T03:44:17.034Z'
title: 快速開始 (Svelte)
id: quick-start
---
# 快速開始

這是一個基本的 Svelte 應用範例，幫助你開始使用 TanStack svelte-store。

**store.ts**
```ts
import { Store } from '@tanstack/svelte-store';

// 你也可以在 Svelte 檔案外部實例化 store！
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


<h1>你的朋友有多少人喜歡貓或狗？</h1>
<p>按下按鈕來統計你的朋友有多少人喜歡貓或狗</p>
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
    
<!-- 這只會在 `state[animal]` 變更時重新渲染。如果 store 中不相關的屬性變更，不會觸發重新渲染 -->
<div>{ animal }: { count.current }</div>
```

**Increment.svelte**
```html
<script lang="ts">
    import { updateState } from './store';
    
    const { animal }: { animal: 'cats' | 'dogs' } = $props()
</script>
    
<button onclick={() => updateState(animal)}>我的朋友喜歡 { animal }</button>
```
