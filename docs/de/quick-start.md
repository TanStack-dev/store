---
source-updated-at: '2025-03-11T17:19:11.000Z'
translation-updated-at: '2025-05-02T04:24:59.023Z'
title: Schnellstart
id: quick-start
---
# Schnellstart

TanStack Store ist in erster Linie eine Framework-unabhängige Signals-Implementierung (Signals Implementation).

Es kann mit jedem unserer Framework-Adapter verwendet werden, aber auch in reinem JavaScript oder TypeScript. Derzeit wird es genutzt, um viele interne Funktionen unserer Bibliotheken zu betreiben.

## Store

Sie beginnen mit der Erstellung einer neuen Store-Instanz, die ein Wrapper um Ihre Daten ist:

```typescript
import { Store } from '@tanstack/store';

const countStore = new Store(0);

console.log(countStore.state); // 0
countStore.setState(() => 1);
console.log(countStore.state); // 1
```

Dieser `Store` kann dann verwendet werden, um Änderungen an Ihren Daten zu verfolgen:

```typescript
const unsub = countStore.subscribe(() => {
  console.log('Der Zähler ist jetzt:', countStore.state);
});

// Später, um aufzuräumen
unsub();
```

Sie können die Daten sogar transformieren, bevor sie aktualisiert werden:

```typescript
const count = new Store(12, {
  updateFn: (prevValue) => updateValue => {
    return updateValue + prevValue;
  }
});

count.setState(() => 12);
// count.state === 24
```

Und eine primitive Form von abgeleitetem Zustand (Derived State) implementieren:

```typescript
let double = 0;
const count = new Store(0, {
  onUpdate: () => {
    double = count.state * 2;
  }
})
```

### Batch-Updates

Sie können mehrere Updates für einen Store mit der `batch`-Funktion bündeln:

```typescript
import { batch } from '@tanstack/store';

// countStore.subscribers wird nur einmal am Ende mit dem finalen Zustand ausgelöst
batch(() => {
  countStore.setState(() => 1);
  countStore.setState(() => 2);
});
```

## Abgeleitete Werte (Derived)

Sie können auch die `Derived`-Klasse verwenden, um abgeleitete Werte zu erstellen, die träge aktualisiert werden, wenn ihre Abhängigkeiten sich ändern:

```typescript
const count = new Store(0);

const double = new Derived({
  fn: () => count.state * 2,
  // Abhängigkeiten müssen explizit aufgelistet werden
  deps: [count]
});

// Der abgeleitete Wert muss gemountet werden, um auf Updates zu hören
const unmount = double.mount();

// Später, um aufzuräumen
unmount();
```

### Vorheriger abgeleiteter Wert

Sie können auf den vorherigen Wert einer abgeleiteten Berechnung zugreifen, indem Sie das `prevVal`-Argument verwenden, das an die `fn`-Funktion übergeben wird:

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

### Abhängigkeitswerte

Sie können auf die Werte der Abhängigkeiten einer abgeleiteten Berechnung zugreifen, indem Sie die Argumente `prevDepVals` und `currDepVals` verwenden, die an die `fn`-Funktion übergeben werden:

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

## Effekte

Sie können auch die `Effect`-Klasse verwenden, um Nebenwirkungen über mehrere Stores und abgeleitete Werte hinweg zu verwalten:

```typescript
const effect = new Effect({
  fn: () => {
    console.log('Der Zähler ist jetzt:', count.state);
  },
  // Array von `Store`s oder `Derived`s
  deps: [count],
  // Soll der Effekt sofort ausgeführt werden? Standard ist false
  eager: true
})

// Der Effekt muss gemountet werden, um auf Updates zu hören
const unmount = effect.mount();

// Später, um aufzuräumen
unmount();
```
