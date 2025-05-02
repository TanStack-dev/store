---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:23:45.707Z'
title: Inicio rápido
id: quick-start
---
# Inicio Rápido

Ejemplo básico de una aplicación en Svelte para comenzar con TanStack svelte-store.

**store.ts**
```ts
import { Store } from '@tanstack/svelte-store';

// ¡Puede instanciar el almacén (store) fuera de los archivos de Svelte también!
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


<h1>¿A cuántos de tus amigos les gustan los gatos o los perros?</h1>
<p>Presione uno de los botones para agregar un contador de cuántos de tus amigos prefieren gatos o perros</p>
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
    
<!-- Esto solo se volverá a renderizar cuando `state[animal]` cambie. Si una propiedad no relacionada del almacén (store) cambia, no se volverá a renderizar -->
<div>{ animal }: { count.current }</div>
```

**Increment.svelte**
```html
<script lang="ts">
    import { updateState } from './store';
    
    const { animal }: { animal: 'cats' | 'dogs' } = $props()
</script>
    
<button onclick={() => updateState(animal)}>A mi amigo le gustan los { animal }</button>
```
