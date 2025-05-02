---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T03:52:06.778Z'
title: クイックスタート
id: quick-start
---
# クイックスタート

TanStack Angularストアを使い始めるための基本的なAngularアプリ例です。

**app.component.ts**
```angular-ts
import { Component } from '@angular/core'
import { DisplayComponent } from './display.component'
import { IncrementComponent } from './increment.component'

@Component({
selector: 'app-root',
imports: [DisplayComponent, IncrementComponent],
template: `
<h1>あなたの友達は猫派？それとも犬派？</h1>
<p>
  ボタンを押して、犬または猫が好きな友達の数をカウントしましょう
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

// Angularコンポーネント外でもストアをインスタンス化できます！
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
        <!-- animalが変更された時のみ再レンダリングされます。関連のないストアプロパティが変更されても再レンダリングされません -->
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
        <button (click)="updateState(animal())">私の友達は{{ animal() }}が好き</button>
    `,
    standalone: true
})
export class Increment {
    animal = input.required<string>();
    updateState = updateState;
}
```
