---
source-updated-at: '2025-03-11T17:19:11.000Z'
translation-updated-at: '2025-05-02T03:44:33.542Z'
title: 快速開始
id: quick-start
---
TanStack Store 首先是一個框架無關 (framework-agnostic) 的訊號 (signals) 實現。

它可以與我們提供的任何框架適配器 (framework adapters) 一起使用，也可以在純 JavaScript 或 TypeScript 中使用。目前它被用於驅動我們許多函式庫的內部功能。

## 儲存庫 (Store)

首先需要建立一個新的 store 實例，這是對資料的封裝：

```typescript
import { Store } from '@tanstack/store';

const countStore = new Store(0);

console.log(countStore.state); // 0
countStore.setState(() => 1);
console.log(countStore.state); // 1
```

這個 `Store` 可以用來追蹤資料的更新：

```typescript
const unsub = countStore.subscribe(() => {
  console.log('The count is now:', countStore.state);
});

// 之後進行清理
unsub();
```

你甚至可以在資料更新前進行轉換：

```typescript
const count = new Store(12, {
  updateFn: (prevValue) => updateValue => {
    return updateValue + prevValue;
  }
});

count.setState(() => 12);
// count.state === 24
```

並實現一種基礎形式的衍生狀態 (derived state)：

```typescript
let double = 0;
const count = new Store(0, {
  onUpdate: () => {
    double = count.state * 2;
  }
})
```

### 批次更新 (Batch Updates)

你可以使用 `batch` 函數對 store 進行批次更新：

```typescript
import { batch } from '@tanstack/store';

// countStore.subscribers 只會在最後觸發一次，並顯示最終狀態
batch(() => {
  countStore.setState(() => 1);
  countStore.setState(() => 2);
});
```

## 衍生值 (Derived)

你也可以使用 `Derived` 類別來建立衍生值，這些值會在其依賴項變更時惰性更新：

```typescript
const count = new Store(0);

const double = new Derived({
  fn: () => count.state * 2,
  // 必須明確列出依賴項
  deps: [count]
});

// 必須掛載 (mount) 衍生值以開始監聽更新
const unmount = double.mount();

// 之後進行清理
unmount();
```

### 先前的衍生值 (Previous Derived Value)

你可以透過傳遞給 `fn` 函數的 `prevVal` 參數來存取衍生計算的前一個值：

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

### 依賴值 (Dependency Values)

你可以透過傳遞給 `fn` 函數的 `prevDepVals` 和 `currDepVals` 參數來存取衍生計算的依賴項值：

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

## 副作用 (Effects)

你也可以使用 `Effect` 類別來管理多個 stores 和衍生值的副作用：

```typescript
const effect = new Effect({
  fn: () => {
    console.log('The count is now:', count.state);
  },
  // `Store` 或 `Derived` 的陣列
  deps: [count],
  // 副作用是否立即執行，預設為 false
  eager: true
})

// 必須掛載 (mount) 副作用以開始監聽更新
const unmount = effect.mount();

// 之後進行清理
unmount();
```
