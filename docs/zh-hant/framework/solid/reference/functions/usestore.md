---
source-updated-at: '2025-03-04T11:38:12.000Z'
translation-updated-at: '2025-05-02T03:43:54.271Z'
id: useStore
title: useStore
---

<!-- DO NOT EDIT: this page is autogenerated from the type comments -->

# Function: useStore()

## Call Signature

```ts
function useStore<TState, TSelected>(store, selector?): Accessor<TSelected>
```

Defined in: [index.tsx:13](https://github.com/TanStack/store/blob/main/packages/solid-store/src/index.tsx#L13)

### Type Parameters

• **TState**

• **TSelected** = `NoInfer`\<`TState`\>

### Parameters

#### store

`Store`\<`TState`, `any`\>

#### selector?

(`state`) => `TSelected`

### Returns

`Accessor`\<`TSelected`\>

## Call Signature

```ts
function useStore<TState, TSelected>(store, selector?): Accessor<TSelected>
```

Defined in: [index.tsx:17](https://github.com/TanStack/store/blob/main/packages/solid-store/src/index.tsx#L17)

### Type Parameters

• **TState**

• **TSelected** = `NoInfer`\<`TState`\>

### Parameters

#### store

`Derived`\<`TState`, `any`\>

#### selector?

(`state`) => `TSelected`

### Returns

`Accessor`\<`TSelected`\>
