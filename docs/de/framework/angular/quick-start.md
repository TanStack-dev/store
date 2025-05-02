---
source-updated-at: '2025-03-11T17:22:18.000Z'
translation-updated-at: '2025-05-02T04:24:39.599Z'
title: Schnellstart
id: quick-start
---
# Schnellstart (Quick Start)

Ein grundlegendes Angular-App-Beispiel, um mit dem TanStack Angular-Store zu beginnen.

**app.component.ts**
```angular-ts
import { Component } from '@angular/core'
import { DisplayComponent } from './display.component'
import { IncrementComponent } from './increment.component'

@Component({
selector: 'app-root',
imports: [DisplayComponent, IncrementComponent],
template: `
<h1>Wie viele deiner Freunde mögen Katzen oder Hunde?</h1>
<p>
  Drücke einen der Buttons, um einen Zähler für die Anzahl deiner Freunde zu erhöhen, die
  Katzen oder Hunde mögen
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

// Sie können den Store auch außerhalb von Angular-Komponenten instanziieren!
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
        <!-- Diese Komponente wird nur neu gerendert, wenn sich animal ändert. Bei Änderungen an nicht verwandten Store-Eigenschaften erfolgt kein Re-Rendering -->
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
        <button (click)="updateState(animal())">Mein Freund mag {{ animal() }}</button>
    `,
    standalone: true
})
export class Increment {
    animal = input.required<string>();
    updateState = updateState;
}
```
