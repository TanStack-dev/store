---
source-updated-at: '2025-03-11T17:19:11.000Z'
translation-updated-at: '2025-04-08T03:15:45.381Z'
title: 快速开始
id: quick-start
---
TanStack Store 首先是一个框架无关的 信号 (signals) 实现。

它可以通过我们的任何框架适配器使用，也可以用于原生 JavaScript 或 TypeScript。目前它被用于支撑我们许多库的内部实现。

## 存储 (Store)

首先需要创建一个新的存储 (Store) 实例，它是数据的包装器：

```typescript
import { Store } from '@tanstack/store';

const countStore = new Store(0);

console.log(countStore.state); // 0
countStore.setState(() => 1);
console.log(countStore.state); // 1
```

这个 `Store` 可以用来追踪数据的更新：

```typescript
const unsub = countStore.subscribe(() => {
  console.log('The count is now:', countStore.state);
});

// 之后进行清理
unsub();
```

你甚至可以在数据更新前进行转换：

```typescript
const count = new Store(12, {
  updateFn: (prevValue) => updateValue => {
    return updateValue + prevValue;
  }
});

count.setState(() => 12);
// count.state === 24
```

并实现一种简单的派生状态：

```typescript
let double = 0;
const count = new Store(0, {
  onUpdate: () => {
    double = count.state * 2;
  }
})
```

### 批量更新

可以使用 `batch` 函数对存储 (Store) 进行批量更新：

```typescript
import { batch } from '@tanstack/store';

// countStore.subscribers 只会在最后触发一次，获取最终状态
batch(() => {
  countStore.setState(() => 1);
  countStore.setState(() => 2);
});
```

## 派生 (Derived)

你也可以使用 `Derived` 类创建派生值，这些值会在它们的依赖项变化时懒加载地更新：

```typescript
const count = new Store(0);

const double = new Derived({
  fn: () => count.state * 2,
  // 必须显式列出依赖项
  deps: [count]
});

// 必须挂载派生值以开始监听更新
const unmount = double.mount();

// 之后进行清理
unmount();
```

### 前一个派生值

可以通过传递给 `fn` 函数的 `prevVal` 参数访问派生计算的前一个值：

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

### 依赖值

可以通过传递给 `fn` 函数的 `prevDepVals` 和 `currDepVals` 参数访问派生计算的依赖项值：

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

## 效果 (Effect)

你也可以使用 `Effect` 类来管理跨多个存储 (Store) 和派生值的副作用：

```typescript
const effect = new Effect({
  fn: () => {
    console.log('The count is now:', count.state);
  },
  // `Store` 或 `Derived` 的数组
  deps: [count],
  // 是否立即运行效果，默认为 false
  eager: true
})

// 必须挂载效果以开始监听更新
const unmount = effect.mount();

// 之后进行清理
unmount();
```
