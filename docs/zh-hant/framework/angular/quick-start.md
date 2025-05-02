---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T03:44:29.764Z'
title: 快速開始 (Angular)
id: quick-start
---
# 快速入門

這是使用 TanStack angular-store 的基本 Angular 應用範例。

**app.component.ts**
```angular-ts
import { Component } from '@angular/core'
import { DisplayComponent } from './display.component'
import { IncrementComponent } from './increment.component'

@Component({
selector: 'app-root',
imports: [DisplayComponent, IncrementComponent],
template: `
<h1>How many of your friends like cats or dogs?</h1>
<p>
  Press one of the buttons to add a counter of how many of your friends like
  cats or dogs
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

// 你也可以在 Angular 元件之外實例化 store！
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
        <!-- 只有當 animal 變化時才會重新渲染。如果 store 中不相關的屬性變化，不會觸發重新渲染 -->
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
        <button (click)="updateState(animal())">My Friend Likes {{ animal() }}</button>
    `,
    standalone: true
})
export class Increment {
    animal = input.required<string>();
    updateState = updateState;
}
```
