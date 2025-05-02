---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-04-08T03:17:02.048Z'
title: 快速开始
id: quick-start
---
快速入门

这是一个使用 TanStack angular-store 的基础 Angular 应用示例。

**app.component.ts**
```angular-ts
import { Component } from '@angular/core'
import { DisplayComponent } from './display.component'
import { IncrementComponent } from './increment.component'

@Component({
selector: 'app-root',
imports: [DisplayComponent, IncrementComponent],
template: `
<h1>你的朋友中有多少人喜欢猫或狗？</h1>
<p>
  点击按钮来统计你的朋友中喜欢猫或狗的人数
</p>
<app-increment animal="dogs" />
<app-display animal="dogs" />
<app-increment animal="cats" />
<app-display animal="cats" />
`,
})
export class AppComponent {}

```

**store.ts**
```typescript
import { Store } from '@tanstack/angular-store';

// 你也可以在 Angular 组件之外实例化存储 (Store)！
export const store = new Store({
  dogs: 0,
  cats: 0,
});

export function updateState(animal: 'dogs' | 'cats') {
  store.setState((state) => {
    return {
      ...state,
      [animal]: state[animal] + 1,
    };
  });
}
```

**display.component.ts**
```angular-ts
import { injectStore } from '@tanstack/angular-store';
import { store } from './store';

@Component({
    selector: 'app-display',
    template: `
        <!-- 只有当 animal 变化时才会重新渲染。如果存储 (Store) 中无关的属性变化，不会触发重新渲染 -->
        <div>{{ animal() }}: {{ count() }}</div>
    `,
    standalone: true
})
export class Display {
    animal = input.required<string>();
    count = injectStore(store, (state) => state[this.animal()]);
}
```

**increment.component.ts**
```angular-ts
import { injectStore } from '@tanstack/angular-store';
import { store, updateState } from './store';

@Component({
    selector: 'app-increment',
    template: `
        <button (click)="updateState(animal())">我的朋友喜欢 {{ animal() }}</button>
    `,
    standalone: true
})
export class Increment {
    animal = input.required<string>();
    updateState = updateState;
}
```
