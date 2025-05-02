---
source-updated-at: '2025-03-11T17:19:11.000Z'
translation-updated-at: '2025-05-02T03:52:35.351Z'
title: クイックスタート
id: quick-start
---
TanStack Storeは、まず第一にフレームワークに依存しないシグナル実装です。

これは私たちのフレームワークアダプターのいずれかと一緒に使用できますが、Vanilla JavaScriptやTypeScriptでも使用できます。現在、多くのライブラリ内部の状態管理を支えるために使用されています。

## ストア

まず、データをラップする新しいストアインスタンスを作成します:

```typescript
import { Store } from '@tanstack/store';

const countStore = new Store(0);

console.log(countStore.state); // 0
countStore.setState(() => 1);
console.log(countStore.state); // 1
```

この`Store`は、データの更新を追跡するために使用できます:

```typescript
const unsub = countStore.subscribe(() => {
  console.log('The count is now:', countStore.state);
});

// 後でクリーンアップする場合
unsub();
```

データを更新する前に変換することも可能です:

```typescript
const count = new Store(12, {
  updateFn: (prevValue) => updateValue => {
    return updateValue + prevValue;
  }
});

count.setState(() => 12);
// count.state === 24
```

そして派生状態の基本的な形式を実装できます:

```typescript
let double = 0;
const count = new Store(0, {
  onUpdate: () => {
    double = count.state * 2;
  }
})
```

### バッチ更新

`batch`関数を使用してストアへの更新を一括処理できます:

```typescript
import { batch } from '@tanstack/store';

// countStore.subscribersは最終状態で一度だけトリガーされます
batch(() => {
  countStore.setState(() => 1);
  countStore.setState(() => 2);
});
```

## 派生値

`Derived`クラスを使用して、依存関係が変更されたときに遅延更新される派生値を作成することもできます:

```typescript
const count = new Store(0);

const double = new Derived({
  fn: () => count.state * 2,
  // 依存関係を明示的にリストする必要があります
  deps: [count]
});

// 更新の監視を開始するには派生値をマウントする必要があります
const unmount = double.mount();

// 後でクリーンアップする場合
unmount();
```

### 前回の派生値

`fn`関数に渡される`prevVal`引数を使用して、派生計算の前回の値にアクセスできます:

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

### 依存関係の値

`fn`関数に渡される`prevDepVals`と`currDepVals`引数を使用して、派生計算の依存関係の値にアクセスできます:

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

## エフェクト

`Effect`クラスを使用して、複数のストアや派生値にわたる副作用を管理することもできます:

```typescript
const effect = new Effect({
  fn: () => {
    console.log('The count is now:', count.state);
  },
  // `Store`または`Derived`の配列
  deps: [count],
  // エフェクトを即時実行するかどうか、デフォルトはfalse
  eager: true
})

// 更新の監視を開始するにはエフェクトをマウントする必要があります
const unmount = effect.mount();

// 後でクリーンアップする場合
unmount();
```
