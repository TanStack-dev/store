---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T03:52:03.674Z'
title: クイックスタート
id: quick-start
---
# クイックスタート

TanStack Svelte-Storeを使い始めるための基本的なSvelteアプリの例です。

**store.ts**
```ts
import { Store } from '@tanstack/svelte-store';

// Svelteファイルの外でもストアをインスタンス化できます！
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


<h1>あなたの友達は猫派？それとも犬派？</h1>
<p>ボタンを押して、友達が猫派か犬派かをカウントしましょう</p>
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
    
<!-- `state[animal]`が変更されたときのみ再レンダリングされます。関連のないストアプロパティが変更されても再レンダリングされません -->
<div>{ animal }: { count.current }</div>
```

**Increment.svelte**
```html
<script lang="ts">
    import { updateState } from './store';
    
    const { animal }: { animal: 'cats' | 'dogs' } = $props()
</script>
    
<button onclick={() => updateState(animal)}>私の友達は{ animal }が好き</button>
```
