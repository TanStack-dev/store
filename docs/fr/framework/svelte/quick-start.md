---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:25:35.667Z'
title: Démarrage rapide
id: quick-start
---
# Démarrage rapide (Quick Start)

Un exemple d'application Svelte de base pour commencer avec TanStack svelte-store.

**store.ts**
```ts
import { Store } from '@tanstack/svelte-store';

// Vous pouvez instancier le store en dehors des fichiers Svelte aussi !
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


<h1>Combien de vos amis aiment les chats ou les chiens ?</h1>
<p>Appuyez sur l'un des boutons pour ajouter un compteur du nombre de vos amis qui aiment les chats ou les chiens</p>
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
    
<!-- Ce composant ne se re-rendra que lorsque `state[animal]` changera. Si une propriété non liée du store change, il ne se re-rendra pas -->
<div>{ animal }: { count.current }</div>
```

**Increment.svelte**
```html
<script lang="ts">
    import { updateState } from './store';
    
    const { animal }: { animal: 'cats' | 'dogs' } = $props()
</script>
    
<button onclick={() => updateState(animal)}>Mon ami aime les { animal }</button>
```
