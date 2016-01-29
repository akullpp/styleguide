# JavaScript (ES5)

## Abgedeckt durch ESLint

### Kein baumelndes Komma

```js
// Falsch
var foo = [
    'bar',
    'baz',
];

// Richtig
var foo = [
    'bar',
    'baz'
];
```

# Keine Zuweisungen in Konditionalen

Ausnahme: In `while` und `do...while`.

```js
// Falsch
if (foo = 0) {}
```

# Keine Ausgabe auf Konsole

Ausnahme: In `node`-Umgebung. In Angular ist `$log` sparsam zu benutzen.

```js
// Falsch
console.log();
```

# Keine Konstanten in Konditionalen

```js
// Falsch
if (true) {}
```

# Keine Kontrollzeichen in Regulären Ausdrücken

ASCII Zeichen 0-31

```js
// Falsch
var foo = /\\x1f/;
```
