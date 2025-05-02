---
source-updated-at: '2025-03-11T17:19:11.000Z'
translation-updated-at: '2025-05-02T04:26:48.663Z'
title: Быстрый старт
id: quick-start
---
# Быстрый старт

TanStack Store — это, в первую очередь, реализация сигналов (signals), не зависящая от фреймворков.

Он может использоваться с любыми адаптерами для фреймворков, но также работает с ванильными JavaScript или TypeScript. В настоящее время он используется внутри многих библиотек TanStack.

## Хранилище (Store)

Начните с создания нового экземпляра хранилища, которое является обёрткой для ваших данных:

```typescript
import { Store } from '@tanstack/store';

const countStore = new Store(0);

console.log(countStore.state); // 0
countStore.setState(() => 1);
console.log(countStore.state); // 1
```

Это `Store` можно использовать для отслеживания изменений данных:

```typescript
const unsub = countStore.subscribe(() => {
  console.log('The count is now:', countStore.state);
});

// Позже, для очистки
unsub();
```

Вы также можете преобразовывать данные перед их обновлением:

```typescript
const count = new Store(12, {
  updateFn: (prevValue) => updateValue => {
    return updateValue + prevValue;
  }
});

count.setState(() => 12);
// count.state === 24
```

И реализовать простую форму производного состояния (derived state):

```typescript
let double = 0;
const count = new Store(0, {
  onUpdate: () => {
    double = count.state * 2;
  }
})
```

### Пакетные обновления (Batch Updates)

Вы можете выполнять пакетные обновления хранилища с помощью функции `batch`:

```typescript
import { batch } from '@tanstack/store';

// Подписчики countStore получат уведомление только один раз в конце с финальным состоянием
batch(() => {
  countStore.setState(() => 1);
  countStore.setState(() => 2);
});
```

## Производные значения (Derived)

Вы также можете использовать класс `Derived` для создания производных значений, которые лениво обновляются при изменении их зависимостей:

```typescript
const count = new Store(0);

const double = new Derived({
  fn: () => count.state * 2,
  // Зависимости должны быть явно указаны
  deps: [count]
});

// Необходимо смонтировать производное значение для начала отслеживания обновлений
const unmount = double.mount();

// Позже, для очистки
unmount();
```

### Предыдущее производное значение

Вы можете получить доступ к предыдущему значению производного вычисления, используя аргумент `prevVal` в функции `fn`:

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

### Значения зависимостей

Вы можете получить доступ к значениям зависимостей производного вычисления, используя аргументы `prevDepVals` и `currDepVals` в функции `fn`:

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

## Эффекты (Effects)

Вы также можете использовать класс `Effect` для управления побочными эффектами между несколькими хранилищами и производными значениями:

```typescript
const effect = new Effect({
  fn: () => {
    console.log('The count is now:', count.state);
  },
  // Массив `Store` или `Derived`
  deps: [count],
  // Запускать эффект немедленно, по умолчанию false
  eager: true
})

// Необходимо смонтировать эффект для начала отслеживания обновлений
const unmount = effect.mount();

// Позже, для очистки
unmount();
```
