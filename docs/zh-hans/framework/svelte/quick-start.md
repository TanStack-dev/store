---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-04-08T03:17:22.356Z'
title: 快速开始
id: quick-start
---
快速入门

使用 TanStack svelte-store 的基础 Svelte 应用示例。

**store.ts**
```ts
import { Store } from '@tanstack/svelte-store';

// 你也可以在 Svelte 文件之外实例化存储 (Store)！
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


<h1>你的朋友中有多少人喜欢猫或狗？</h1>
<p>点击按钮来统计喜欢猫或狗的朋友数量</p>
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
    
<!-- 只有当 `state[animal]` 变化时才会重新渲染。如果存储 (Store) 中无关的属性变化，不会触发重新渲染 -->
<div>{ animal }: { count.current }</div>
```

**Increment.svelte**
```html
<script lang="ts">
    import { updateState } from './store';
    
    const { animal }: { animal: 'cats' | 'dogs' } = $props()
</script>
    
<button onclick={() => updateState(animal)}>我的朋友喜欢 { animal }</button>
```
