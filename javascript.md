# JavaScript (ES5)

- [General](#general)
- [Covered by ESLint Plugin](#covered-by-eslint-plugin)

## General

## Covered by [ESLint Plugin](https://github.com/akullpp/eslint-plugin-skeleton)

### No comma dangle

```js
// Wrong
var foo = [
    'bar',
    'baz',
];

// Right
var foo = [
    'bar',
    'baz'
];
```

### No assignment in conditionals

Exception: In `while` and `do...while`.

```js
// Wrong
if (foo = 0) {
    // ...
}
```

### No console

Exception: In `node` environment.

```js
// Wrong
console.log();
```

### No constants in conditionals

```js
// Wrong
if (true) {
    // ...
}
```

### No control characters in Regular Expressions

ASCII characters ranging from 0 to 31.

```js
// Wrong
var foo = /\\x1f/;
```

### No debugger

```js
// Wrong
debugger;
```

### No duplicate arguments

```js
// Wrong
function (foo, bar, foo) {
    // ...
}
```

### No duplicate keys

```js
// Wrong
var foo = {
    bar: 'baz',
    bar: 'baz'
}
```

### No duplicate case labels

```js
// Wrong
switch (foo) {
    case 1:
        break;
    case 2:
        break;
    case 1:
        break;
    default:
        break;
}
```

### No empty character class in Regular Expressions

```js
// Wrong
var foo = /^abc[]/;
```

### No empty statements

```js
// Wrong
if (foo) {
}
```

### No assignment to the exception parameter

```js
// Wrong
try {
    // ...
} catch (ex) {
    ex = 'foo';
}
```

### No extra boolean cast

```js
// Wrong
if (!!foo) {
    // ...
}
```

### No extra semicolons

```js
// Wrong
var foo;;
```

### No reassignments of function declarations

```js
// Wrong
function foo() {
    // ...
}
foo = 'bar';
```

### No function declarations in nested blocks

```js
// Wrong
if (foo) {
    function baz() {
        // ...
    }
}
```

### No invalid string arguments in RegExp constructor

```js
// Wrong
var foo = new RegExp('[');
```

### No irregular whitespaces

```js
function foo()\u00A0{
    // ...
}
```

### No negation of the left hand side in conditionals

```js
// Wrong
if (!foo in bar) {
    // ...
}
```

### No invokation of capitalized global objects

```js
// Wrong
Math();
```

### No multiple spaces in Regular Expressions

```js
// Wrong
var foo = /a   b/;

// Right
var foo = /a {3}b/;
```

### No spares arrays

```js
// Wrong
var foo = [1,,3];
```

### No unexpected multilines

```js
// Wrong
var foo = 'baz'
(1 || 2).baz();

// Right
var foo = 'baz';
(1 || 2).baz();
```

### No unreachable code

```js
// Wrong
function foo () {
    var bar = true;
    return bar;
    bar = false;
}
```

### NaN comparisons

```js
// Wrong
if (foo == NaN) {
    // ...
}

// Right
if (isNaN(foo)) {
    // ...
}
```

### Valid typeof strings

```js
// Wrong
if (typeof 'foo' === 'strng') {
    // ..
}
```
