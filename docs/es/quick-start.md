---
source-updated-at: '2025-03-11T17:19:11.000Z'
translation-updated-at: '2025-05-02T04:24:01.621Z'
title: Inicio rápido
id: quick-start
---
# Inicio Rápido  

TanStack Store es, ante todo, una implementación de señales (signals) independiente del framework.  

Se puede utilizar con cualquiera de nuestros adaptadores para frameworks, pero también se puede usar en JavaScript o TypeScript estándar. Actualmente, se utiliza para potenciar muchos de los componentes internos de nuestras bibliotecas.  

## Store  

Para comenzar, creará una nueva instancia de Store, que es un envoltorio alrededor de sus datos:  

```typescript
import { Store } from '@tanstack/store';

const countStore = new Store(0);

console.log(countStore.state); // 0
countStore.setState(() => 1);
console.log(countStore.state); // 1
```  

Este `Store` puede usarse para rastrear actualizaciones de sus datos:  

```typescript
const unsub = countStore.subscribe(() => {
  console.log('The count is now:', countStore.state);
});

// Más tarde, para limpiar
unsub();
```  

Incluso puede transformar los datos antes de que se actualicen:  

```typescript
const count = new Store(12, {
  updateFn: (prevValue) => updateValue => {
    return updateValue + prevValue;
  }
});

count.setState(() => 12);
// count.state === 24
```  

Y puede implementar una forma primitiva de estado derivado:  

```typescript
let double = 0;
const count = new Store(0, {
  onUpdate: () => {
    double = count.state * 2;
  }
})
```  

### Actualizaciones en Lote  

Puede agrupar actualizaciones en un Store usando la función `batch`:  

```typescript
import { batch } from '@tanstack/store';

// Los suscriptores de countStore solo se activarán una vez al final con el estado final
batch(() => {
  countStore.setState(() => 1);
  countStore.setState(() => 2);
});
```  

## Derived  

También puede usar la clase `Derived` para crear valores derivados que se actualizan de forma diferida cuando sus dependencias cambian:  

```typescript
const count = new Store(0);

const double = new Derived({
  fn: () => count.state * 2,
  // Debe listar explícitamente las dependencias
  deps: [count]
});

// Debe montar el valor derivado para comenzar a escuchar actualizaciones
const unmount = double.mount();

// Más tarde, para limpiar
unmount();
```  

### Valor Derivado Anterior  

Puede acceder al valor anterior de un cálculo derivado usando el argumento `prevVal` pasado a la función `fn`:  

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

### Valores de Dependencias  

Puede acceder a los valores de las dependencias de un cálculo derivado usando los argumentos `prevDepVals` y `currDepVals` pasados a la función `fn`:  

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

## Effects  

También puede usar la clase `Effect` para manejar efectos secundarios en múltiples Stores y valores derivados:  

```typescript
const effect = new Effect({
  fn: () => {
    console.log('The count is now:', count.state);
  },
  // Arreglo de `Store`s o `Derived`s
  deps: [count],
  // Si el efecto debe ejecutarse inmediatamente, el valor predeterminado es false
  eager: true
})

// Debe montar el efecto para comenzar a escuchar actualizaciones
const unmount = effect.mount();

// Más tarde, para limpiar
unmount();
```
