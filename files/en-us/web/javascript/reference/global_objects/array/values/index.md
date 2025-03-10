---
title: Array.prototype.values()
slug: Web/JavaScript/Reference/Global_Objects/Array/values
tags:
  - Array
  - Beginner
  - ECMAScript 2015
  - Iterator
  - JavaScript
  - Method
  - Prototype
  - Reference
  - Polyfill
browser-compat: javascript.builtins.Array.values
---

{{JSRef}}

The **`values()`** method returns a new _array [iterator](/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterator_protocol)_ object that iterates the value of each index in the array.

{{EmbedInteractiveExample("pages/js/array-values.html")}}

## Syntax

```js
values()
```

### Return value

A new iterable iterator object.

## Description

`Array.prototype.values()` is the default implementation of [`Array.prototype[@@iterator]()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/@@iterator).

```js
Array.prototype.values === Array.prototype[Symbol.iterator]; // true
```

## Examples

### Iteration using for...of loop

Because `values()` returns an iterable iterator, you can use a [`for...of`](/en-US/docs/Web/JavaScript/Reference/Statements/for...of) loop to iterate it.

```js
const arr = ["a", "b", "c", "d", "e"];
const iterator = arr.values();

for (const letter of iterator) {
  console.log(letter);
} // "a" "b" "c" "d" "e"
```

### Iteration using next()

Because the return value is also an iterator, you can directly call its `next()` method.

```js
const arr = ["a", "b", "c", "d", "e"];
const iterator = arr.values();
iterator.next(); // { value: "a", done: false }
iterator.next(); // { value: "b", done: false }
iterator.next(); // { value: "c", done: false }
iterator.next(); // { value: "d", done: false }
iterator.next(); // { value: "e", done: false }
iterator.next(); // { value: undefined, done: true }
console.log(iterator.next().value); // undefined
```

### Reusing the iterable

> **Warning:** The array iterator object should be a one-time use object. Do not reuse it.

The iterable returned from `values()` is not reusable. When `next().done = true` or `currentIndex > length`, [the `for...of` loop ends](/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#interactions_between_the_language_and_iteration_protocols), and further iterating it has no effect.

```js
const arr = ["a", "b", "c", "d", "e"];
const values = arr.values();
for (const letter of values) {
  console.log(letter);
}
// "a" "b" "c" "d" "e"
for (const letter of values) {
  console.log(letter);
}
// undefined
```

If you use a [`break`](/en-US/docs/Web/JavaScript/Reference/Statements/break) statement to end the iteration early, the iterator can resume from the current position when continuing to iterate it.

```js
const arr = ["a", "b", "c", "d", "e"];
const values = arr.values();
for (const letter of values) {
  console.log(letter);
  if (letter === "b") {
    break;
  }
}
// "a" "b"

for (const letter of values) {
  console.log(letter);
}
// "c" "d" "e"
```

### Mutations during iteration

There are no values stored in the array iterator object returned from `values()`; instead, it stores the address of the array used in its creation, and reads the currently visited index on each iteration. Therefore, its iteration output depends on the value stored in that index at the time of stepping. If the values in the array changed, the array iterator object's values change too.

```js
const arr = ["a", "b", "c", "d", "e"];
const iterator = arr.values();
console.log(iterator); // Array Iterator { }
console.log(iterator.next().value); // "a"
arr[1] = "n";
console.log(iterator.next().value); // "n"
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [Polyfill of `Array.prototype.values` in `core-js`](https://github.com/zloirock/core-js#ecmascript-array)
- {{jsxref("Array.prototype.keys()")}}
- {{jsxref("Array.prototype.entries()")}}
- {{jsxref("Array.prototype.forEach()")}}
- {{jsxref("Array.prototype.every()")}}
- {{jsxref("Array.prototype.some()")}}
- [A polyfill](https://github.com/behnammodi/polyfill/blob/master/array.polyfill.js)
