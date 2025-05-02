---
source-updated-at: '2025-03-11T17:19:11.000Z'
translation-updated-at: '2025-05-02T04:27:54.657Z'
title: بدء سريع
id: quick-start
---
# بدء سريع

TanStack Store هو في المقام الأول تنفيذ للإشارات (signals) غير مرتبط بإطار عمل محدد.

يمكن استخدامه مع أي من أدوات التكامل الخاصة بأطر العمل لدينا، ولكنه يمكن أيضًا استخدامه في JavaScript أو TypeScript العادي. يتم استخدامه حاليًا لتشغيل العديد من الوظائف الداخلية لمكتباتنا.

## المتجر (Store)

ستبدأ بإنشاء مثيل جديد للمتجر، وهو غلاف حول بياناتك:

```typescript
import { Store } from '@tanstack/store';

const countStore = new Store(0);

console.log(countStore.state); // 0
countStore.setState(() => 1);
console.log(countStore.state); // 1
```

يمكن بعد ذلك استخدام هذا `المتجر (Store)` لتتبع التحديثات على بياناتك:

```typescript
const unsub = countStore.subscribe(() => {
  console.log('The count is now:', countStore.state);
});

// لاحقًا، للتنظيف
unsub();
```

يمكنك حتى تحويل البيانات قبل تحديثها:

```typescript
const count = new Store(12, {
  updateFn: (prevValue) => updateValue => {
    return updateValue + prevValue;
  }
});

count.setState(() => 12);
// count.state === 24
```

وتنفيذ شكل بدائي من الحالة المشتقة (derived state):

```typescript
let double = 0;
const count = new Store(0, {
  onUpdate: () => {
    double = count.state * 2;
  }
})
```

### تحديثات الدُفعات (Batch Updates)

يمكنك تجميع التحديثات على المتجر باستخدام الدالة `batch`:

```typescript
import { batch } from '@tanstack/store';

// countStore.subscribers سيتم تشغيلها مرة واحدة فقط في النهاية بالحالة النهائية
batch(() => {
  countStore.setState(() => 1);
  countStore.setState(() => 2);
});
```

## المشتقات (Derived)

يمكنك أيضًا استخدام فئة `Derived` لإنشاء قيم مشتقة يتم تحديثها بكسل (lazily) عند تغيير تبعياتها:

```typescript
const count = new Store(0);

const double = new Derived({
  fn: () => count.state * 2,
  // يجب سرد التبعيات بشكل صريح
  deps: [count]
});

// يجب تركيب القيمة المشتقة لبدء الاستماع للتحديثات
const unmount = double.mount();

// لاحقًا، للتنظيف
unmount();
```

### القيمة المشتقة السابقة (Previous Derived Value)

يمكنك الوصول إلى القيمة السابقة للحساب المشتق باستخدام الوسيطة `prevVal` الممررة إلى الدالة `fn`:

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

### قيم التبعيات (Dependency Values)

يمكنك الوصول إلى قيم تبعيات الحساب المشتق باستخدام الوسيطتين `prevDepVals` و `currDepVals` الممررتين إلى الدالة `fn`:

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

## التأثيرات (Effects)

يمكنك أيضًا استخدام فئة `Effect` لإدارة التأثيرات الجانبية عبر متاجر وقيم مشتقة متعددة:

```typescript
const effect = new Effect({
  fn: () => {
    console.log('The count is now:', count.state);
  },
  // مصفوفة من `Store`s أو `Derived`s
  deps: [count],
  // هل يجب تشغيل التأثير فورًا، القيمة الافتراضية هي false
  eager: true
})

// يجب تركيب التأثير لبدء الاستماع للتحديثات
const unmount = effect.mount();

// لاحقًا، للتنظيف
unmount();
```
