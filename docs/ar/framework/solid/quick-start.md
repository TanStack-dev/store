---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:27:30.509Z'
title: بدء سريع
id: quick-start
---
# بدء سريع  

مثال تطبيق أساسي باستخدام إطار العمل (Solid) للبدء مع مكتبة (TanStack Solid-store).  

```jsx
import { Store, useStore } from '@tanstack/solid-store';

// يمكنك إنشاء المتجر خارج مكونات (Solid) أيضًا!
export const store = new Store({
  cats: 0,
  dogs: 0
})

export const Display = (props) => {
  const count = useStore(store, (state) => state[props.animals]);
  return (
    <span>
      {props.animals}: {count()}
      </span>
    );
}

export const Button = (props) => {
  return (
    <button
      onClick={() => {
        store.setState((state) => {
          return {
            ...state,
            [props.animals]: state[props.animals] + 1
          }
        })
      }}
    >
      Increment
    </button>
  )
}

const App = () => {
  return (
    <div>
    <h1>كم عدد أصدقائك الذين يحبون القطط أو الكلاب؟</h1>
    <p>
      اضغط على أحد الأزرار لإضافة عداد لعدد أصدقائك الذين يحبون القطط أو الكلاب
      </p>
      <Button animals="dogs" />
      <Display animals="dogs" />
      <Button animals="cats" />
      <Display animals="cats" />
  </div>
  );
};

export default App;
```
