---
source-updated-at: '2025-03-11T17:19:11.000Z'
translation-updated-at: '2025-05-02T04:25:55.537Z'
title: Démarrage rapide
id: quick-start
---
# Démarrage rapide

TanStack Store est avant tout une implémentation de signaux (signals) indépendante du framework.

Il peut être utilisé avec n'importe lequel de nos adaptateurs de framework, mais aussi avec du JavaScript ou TypeScript vanilla. Il est actuellement utilisé pour alimenter les fonctionnalités internes de nombreuses bibliothèques.

## Store

Vous commencerez par créer une nouvelle instance de store, qui est un wrapper autour de vos données :

```typescript
import { Store } from '@tanstack/store';

const countStore = new Store(0);

console.log(countStore.state); // 0
countStore.setState(() => 1);
console.log(countStore.state); // 1
```

Ce `Store` peut ensuite être utilisé pour suivre les mises à jour de vos données :

```typescript
const unsub = countStore.subscribe(() => {
  console.log('The count is now:', countStore.state);
});

// Plus tard, pour nettoyer
unsub();
```

Vous pouvez même transformer les données avant leur mise à jour :

```typescript
const count = new Store(12, {
  updateFn: (prevValue) => updateValue => {
    return updateValue + prevValue;
  }
});

count.setState(() => 12);
// count.state === 24
```

Et implémenter une forme primitive d'état dérivé (derived state) :

```typescript
let double = 0;
const count = new Store(0, {
  onUpdate: () => {
    double = count.state * 2;
  }
})
```

### Mises à jour par lot (Batch Updates)

Vous pouvez regrouper les mises à jour d'un store en utilisant la fonction `batch` :

```typescript
import { batch } from '@tanstack/store';

// countStore.subscribers ne se déclenchera qu'une seule fois à la fin avec l'état final
batch(() => {
  countStore.setState(() => 1);
  countStore.setState(() => 2);
});
```

## Valeurs dérivées (Derived)

Vous pouvez également utiliser la classe `Derived` pour créer des valeurs dérivées qui se mettent à jour de manière paresseuse lorsque leurs dépendances changent :

```typescript
const count = new Store(0);

const double = new Derived({
  fn: () => count.state * 2,
  // Doit explicitement lister les dépendances
  deps: [count]
});

// Doit monter (mount) la valeur dérivée pour commencer à écouter les mises à jour
const unmount = double.mount();

// Plus tard, pour nettoyer
unmount();
```

### Valeur dérivée précédente

Vous pouvez accéder à la valeur précédente d'un calcul dérivé en utilisant l'argument `prevVal` passé à la fonction `fn` :

```typescript
const count = new Store(1);

const double = new Derived({
  fn: ({ prevVal }) => {
    return count.state + (prevVal ?? 0);
  },
  deps: [count]
});

double.mount();
double.state; // 1
count.setState(() => 2);
double.state; // 3
```

### Valeurs des dépendances

Vous pouvez accéder aux valeurs des dépendances d'un calcul dérivé en utilisant les arguments `prevDepVals` et `currDepVals` passés à la fonction `fn` :

```typescript
const count = new Store(1);

const double = new Derived({
  fn: ({ prevDepVals, currDepVals }) => {
    return (prevDepVals[0] ?? 0) + currDepVals[0];
  },
  deps: [count]
});

double.mount();
double.state; // 1
count.setState(() => 2);
double.state; // 3
```

## Effets (Effects)

Vous pouvez également utiliser la classe `Effect` pour gérer les effets secondaires sur plusieurs stores et valeurs dérivées :

```typescript
const effect = new Effect({
  fn: () => {
    console.log('The count is now:', count.state);
  },
  // Tableau de `Store`s ou `Derived`s
  deps: [count],
  // L'effet doit-il s'exécuter immédiatement, par défaut false
  eager: true
})

// Doit monter (mount) l'effet pour commencer à écouter les mises à jour
const unmount = effect.mount();

// Plus tard, pour nettoyer
unmount();
```
