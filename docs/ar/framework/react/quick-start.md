---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:27:31.953Z'
title: بدء سريع
id: quick-start
---
# بدء سريع

مثال تطبيق React الأساسي للبدء مع TanStack react-store.

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import { Store, useStore } from "@tanstack/react-store";

// يمكنك إنشاء المتجر خارج مكونات React أيضًا!
export const store = new Store({
  dogs: 0,
  cats: 0,
});

// هذا سيعيد التصيير فقط عند تغيير `state[animal]`. إذا تغيرت خاصية غير مرتبطة في المتجر، لن يعيد التصيير

const Display = ({ animal }) => {
  const count = useStore(store, (state) => state[animal]);
  return <div>{`${animal}: ${count}`}</div>;
};

const updateState = (animal) => {
  store.setState((state) => {
    return {
      ...state,
      [animal]: state[animal] + 1,
    };
  });
};
const Increment = ({ animal }) => (
  <button onClick={() => updateState(animal)}>My Friend Likes {animal}</button>
);

function App() {
  return (
    <div>
      <h1>كم عدد أصدقائك الذين يحبون القطط أو الكلاب؟</h1>
      <p>
        اضغط على أحد الأزرار لإضافة عداد لعدد أصدقائك الذين يحبون القطط أو الكلاب
      </p>
      <Increment animal="dogs" />
      <Display animal="dogs" />
      <Increment animal="cats" />
      <Display animal="cats" />
    </div>
  );
}

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);

```
