---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:26:33.744Z'
title: Быстрый старт
id: quick-start
---
# Быстрый старт

Базовый пример приложения на Angular для начала работы с TanStack angular-store.

**app.component.ts**
```angular-ts
import { Component } from '@angular/core'
import { DisplayComponent } from './display.component'
import { IncrementComponent } from './increment.component'

@Component({
selector: 'app-root',
imports: [DisplayComponent, IncrementComponent],
template: `
<h1>Сколько ваших друзей любят кошек или собак?</h1>
<p>
  Нажмите одну из кнопок, чтобы увеличить счетчик друзей, которые любят
  кошек или собак
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

// Вы можете инициализировать хранилище (store) вне Angular-компонентов!
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
        <!-- Этот компонент будет перерендериваться только при изменении animal. Если изменится несвязанное свойство хранилища (store), перерендера не произойдет -->
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
        <button (click)="updateState(animal())">Мой друг любит {{ animal() }}</button>
    `,
    standalone: true
})
export class Increment {
    animal = input.required<string>();
    updateState = updateState;
}
```
