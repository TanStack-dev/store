---
source-updated-at: '2025-03-04T11:38:12.000Z'
translation-updated-at: '2025-05-06T20:16:00.596Z'
id: __storeToDerived
title: __storeToDerived
---

<!-- DO NOT EDIT: this page is autogenerated from the type comments -->

# Variable: \_\_storeToDerived

```ts
const __storeToDerived: WeakMap<Store<unknown, (cb) => unknown>, Set<Derived<unknown, readonly any[]>>>;
```

Defined in: [scheduler.ts:19](https://github.com/TanStack/store/blob/main/packages/store/src/scheduler.ts#L19)

This is here to solve the pyramid dependency problem where:
      A
     / \
    B   C
     \ /
      D

Where we deeply traverse this tree, how do we avoid D being recomputed twice; once when B is updated, once when C is.

To solve this, we create linkedDeps that allows us to sync avoid writes to the state until all of the deps have been
resolved.

This is a record of stores, because derived stores are not able to write values to, but stores are
