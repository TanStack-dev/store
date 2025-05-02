---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:24:39.421Z'
title: Schnellstart
id: quick-start
---
# Schnellstart

Das grundlegende Svelte-App-Beispiel, um mit dem TanStack Svelte-Store (Svelte-Store) zu beginnen.

**store.ts**
```ts
import { Store } from '@tanstack/svelte-store';

// Sie können den Store auch außerhalb von Svelte-Dateien instanziieren!
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


<h1>Wie viele deiner Freunde mögen Katzen oder Hunde?</h1>
<p>Drücke einen der Buttons, um einen Zähler hinzuzufügen, der zeigt, wie viele deiner Freunde Katzen oder Hunde mögen</p>
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
    
<!-- Dies wird nur neu gerendert, wenn sich `state[animal]` ändert. Wenn sich eine nicht verwandte Store-Eigenschaft ändert, erfolgt kein Re-Rendering -->
<div>{ animal }: { count.current }</div>
```

**Increment.svelte**
```html
<script lang="ts">
    import { updateState } from './store';
    
    const { animal }: { animal: 'cats' | 'dogs' } = $props()
</script>
    
<button onclick={() => updateState(animal)}>Mein Freund mag { animal }</button>
```
