# ES5

- [General](#general)
- [Covered by ESLint Plugin](#covered-by-eslint-plugin)
    - [Possible Errors](#possible-errors)
    - [Best Practices](#best-practices)
    - [Strict](#strict)
    - [Variables](#variables)
    - [Stylistic Issues](#stylistic-issues)

## General

#### Use guard clauses

```js
function () {
    if (!foo) {
        return;
    }
    // ...
}
```

## Covered by [ESLint Plugin](https://github.com/akullpp/eslint-plugin-skeleton)

### Possible Errors

#### No comma dangle

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

#### No assignment in conditionals

Exception: In `while` and `do...while`.

```js
// Wrong
if (foo = 0) {
    // ...
}
```

#### No console

Exception: In `node` environment.

```js
// Wrong
console.log();
```

#### No constants in conditionals

```js
// Wrong
if (true) {
    // ...
}
```

#### No control characters in Regular Expressions

ASCII characters ranging from 0 to 31.

```js
// Wrong
var foo = /\\x1f/;
```

#### No debugger

```js
// Wrong
debugger;
```

#### No duplicate arguments

```js
// Wrong
function (foo, bar, foo) {
    // ...
}
```

#### No duplicate keys

```js
// Wrong
var foo = {
    bar: 'baz',
    bar: 'baz'
}
```

#### No duplicate case labels

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

#### No empty character class in Regular Expressions

```js
// Wrong
var foo = /^abc[]/;
```

#### No empty statements

```js
// Wrong
if (foo) {
}
```

#### No assignment to the exception parameter

```js
// Wrong
try {
    // ...
} catch (ex) {
    ex = 'foo';
}
```

#### No extra boolean cast

```js
// Wrong
if (!!foo) {
    // ...
}
```

#### No extra semicolons

```js
// Wrong
var foo;;
```

#### No reassignments of function declarations

```js
// Wrong
function foo() {
    // ...
}
foo = 'bar';
```

#### No function declarations in nested blocks

```js
// Wrong
if (foo) {
    function baz() {
        // ...
    }
}
```

#### No invalid string arguments in RegExp constructor

```js
// Wrong
var foo = new RegExp('[');
```

#### No irregular whitespaces

```js
function foo()\u00A0{
    // ...
}
```

#### No negation of the left hand side in conditionals

```js
// Wrong
if (!foo in bar) {
    // ...
}
```

#### No invokation of capitalized global objects

```js
// Wrong
Math();
```

#### No multiple spaces in Regular Expressions

```js
// Wrong
var foo = /a   b/;

// Right
var foo = /a {3}b/;
```

#### No spares arrays

```js
// Wrong
var foo = [1,,3];
```

#### No unexpected multilines

```js
// Wrong
var foo = 'baz'
(1 || 2).baz();

// Right
var foo = 'baz';
(1 || 2).baz();
```

#### No unreachable code

```js
// Wrong
function foo () {
    var bar = true;
    return bar;
    bar = false;
}
```

#### NaN comparisons

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

#### Valid typeof strings

```js
// Wrong
if (typeof 'foo' === 'strng') {
    // ..
}
```

### Best Practices

#### No usage of variables outside of their block

```js
// Wrong
if (foo) {
    var bar = 1;
} else {
    var bar = 0;
}
baz = bar;

// Right
var bar = 0;

if (foo) {
    bar = 1;
}
baz = bar;
```

#### Always use braces for statements

```js
// Wrong
if (foo) return 1;

// Right
if (foo) {
    return 1;
}
```

#### In switch statements always use a default case

```js
// Right
switch (foo) {
    case 1:
        break;
    case 2:
        break;
    default:
        break;
}
```

#### In multiline property calls the dot should always be on the same line as the property

```js
// Wrong
foo.
    bar().
    bar();

// Right
foo
    .bar()
    .bar();
```

#### If a property isn't a variable, use dot notation

```js
// Wrong
foo['bar'];

// Right
foo.bar;
```

#### Always use type-safe equality operators

```js
// Wrong
if (foo == bar) {
    // ...
}

// Right
if (foo === bar) {
    // ...
}
```

#### Always have a guard clause in `for in` loops to exclude properties inherited from the prototype chain

Should not occur since `guard-for-in` is active.

```js
// Wrong
for (key in foo) {
    bar(key);
}

// Right
for (key in foo) {
    if (foo.hasOwnProperty(key)) {
        bar(key);
    }
}
```

#### Don't use `alert`, `confirm` or `prompt`

#### Don't use `arguments.caller` or `arguments.callee`

#### Don't declare functions in case clauses

```js
// Wrong
switch (foo) {
    case 1:
        function bar() {
            // ...
        }
        break;
    default:
        break;
}
```

#### If an `if` block returns, the `else` block becomes unnecessary

```js
// Wrong
if (foo) {
    return 1;
} else {
    return 2;
}

// Right
if (foo) {
    return 1;
}
return 2;
```

#### Only use labels for iteration or switch statements

```js
// Wrong
foo:
var bar = 1;
```

#### Don't use `eval`

#### Don't extend native objects

```js
// Wrong
Object.prototype.foo = bar;
```

#### Don't use unnecessary bindings

Happens when `this` gets removed by refactoring.

```js
// Wrong
function () {
    foo();
}.bind(bar);
```

#### Don't use fallthroug in switch statements

```js
// Wrong
switch(foo) {
    case 1:
        bar();
    case 2:
        baz();
}

// Right
switch(foo) {
    case 1:
        bar();
        break;
    case 2:
        baz();
        break;
    default:
        break;
}
```

#### Don't use leading or trailing decimal points in numeric literals

```js
// Wrong
var foo = .1;
var bar = 1.;
```

#### Don't use implicit coercion

```js
// Wrong
var foo = !!bar;
var foo = +bar;
var foo = '' + bar;

// Right
var foo = Boolean(foo);
var foo = Number(foo);
var foo = String(foo);
```

#### Don't use functions for implied evaluation

```js
// Wrong
setTimeout('alert("foo");', 100);
```


