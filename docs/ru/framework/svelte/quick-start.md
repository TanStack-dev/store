---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:26:31.810Z'
title: Быстрый старт
id: quick-start
---
# Быстрый старт

Базовый пример приложения на Svelte для начала работы с TanStack svelte-store.

**store.ts**
```ts
import { Store } from '@tanstack/svelte-store';

// Вы можете инициализировать хранилище (store) и вне файлов Svelte!
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


<h1>Сколько ваших друзей любят кошек или собак?</h1>
<p>Нажмите на одну из кнопок, чтобы увеличить счетчик друзей, предпочитающих кошек или собак</p>
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
    
<!-- Этот компонент будет перерендериваться только при изменении `state[animal]`. Если изменится несвязанное свойство хранилища (store), перерендеринга не произойдет -->
<div>{ animal }: { count.current }</div>
```

**Increment.svelte**
```html
<script lang="ts">
    import { updateState } from './store';
    
    const { animal }: { animal: 'cats' | 'dogs' } = $props()
</script>
    
<button onclick={() => updateState(animal)}>Мой друг любит { animal }</button>
```
