# super-iterable

[![NPM](https://nodei.co/npm/super-iterable.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/super-iterable/)

[![Build Status](https://travis-ci.org/seanchas116/super-iterable.svg?branch=master)](https://travis-ci.org/seanchas116/super-iterable)

super-iterable is a JavaScript library providing ES6 Iterable utilities.

## Features

* Utility methods like `map` and `filter` for ES6 Iterables
* Lazy evaluation (it's nature of Iterables)
* TypeScript `.d.ts` included (only for ES6 output mode)

super-iterable itself is written in ES6 and TypeScript.

## Examples

### Fizzbuzz

```js
import _ from "super-iterable";

function fizzbuzz(n) {
  if (n % 15 === 0) {
    return "fizzbuzz";
  } else if (n % 3 === 0) {
    return "fizz";
  } else if (n % 5 === 0) {
    return "buzz";
  } else {
    return n.toString();
  }
}

for (const out of _.times(50).map(fizzbuzz)) {
  console.log(out);
}
```

### Fibonacci sequence

```js
const _ = require("super-iterable");

const fibs = _(function* () {
  let a = 0;
  let b = 1;
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
});
for (const fib of fibs.take(50)) {
  console.log(fib);
}
```

## Install

```
npm install --save super-iterable
```

## How to use

```js
// example.js

const _ = require("super-iterable");
// or you can do this in Babel
import _ from "super-iterable";

// your code...
```

### Babel + Node

If you are using Babel + Node, you have to override `ignore` option to import `super-iterable` module as ES6 (Babel ignores JavaScript files inside `node_modules` by default).

Example:

```
babel-node --ignore false example.js
```

### TypeScript + ES6

Since super-iterable provides TypeScript type definitions by default, it is also nice to use it in TypeScript.

TypeScript >= 1.8 (nightly) is recommended, since it has started support CommonJS when targeting ES6.

```ts
// example.ts
import _ = require("super-iterable");

// your code...
```

## API

#### `_(iterable) => SuperIterable`

Wraps an Iterable into a `SuperIterable`.

```js
// arrays
_([1,2,3]) //=> SuperIterable(1,2,3)

// sets
const set = new Set([1,2,3]);
_(set); //=> SuperIterable(1,2,3)

// maps
const map = new Map([1, "one"], [2, "two"]);
_(map); //=> SuperIterable([1, "one"], [2, "two"])
```

#### `_(generatorFunc) => SuperIterable`

Creates a `SuperIterable` from a generator function.

```js
_(function *() {
  yield 1;
  yield 2;
  yield 3;
}); //=> SuperIterable(1,2,3)
```

#### `_.times(count)`

Creates a `SuperIterable` that enumerates 0 .. `count`.

```js
_.times(3)
  .toArray(); //=> [0,1,2]
```

#### `_.count(begin, step = 1)`

Creates a `SuperIterable` that enumerates `begin`, `begin + step`, `begin + 2 *step` ... .

```js
_.count(1, 2)
  .take(3)
  .toArray(); //=> [1,3,5]
```

### `SuperIterable`

`SuperIterable` is a convenient wrapper for Iterable objects.
`SuperIterable` itself is also an Iterable.

#### `SuperIterable#map(func)`

Transforms values with `func` function.

```js
_([1,2,3])
  .map(x => x * 2)
  .toArray(); //=> [2,4,6]
```

#### `SuperIterable#filter(pred)`

Filters values with `pred` function.

```js
_([1,2,3,4])
  .filter(x => x % 2 == 0)
  .toArray(); //=> [2,4]
```

#### `SuperIterable#flatMap(func)`

Transforms values into Iterables with `func` function and concatenates them.

```js
_([1,2,3])
  .flatMap(x => [x, x * 2])
  .toArray(); //=> [1,2,2,4,3,6]
```

#### `SuperIterable#entries(func)`

Make pairs from the indices and the values.

```js
_(["foo", "bar", "baz"])
  .entries(); //=> [[1, "foo"], [2, "bar"], [3, "baz"]]
```

#### `SuperIterable#reduce(func, initValue)`

Accumulates each value.

```js
_([1,2,3,4])
  .reduce((acc, x) => acc * x, 1); //=> 24
```

#### `SuperIterable#take(count)`

Takes the first `count` values.

```js
_(["foo", "bar", "baz"])
  .take(2); //=> ["foo", "bar"]
```

#### `SuperIterable#takeWhile(pred)`

Takes the elements while `pred(element)` is true.

```js
_([2,4,6,5,4])
  .takeWhile(x => x % 2 == 0); //=> [2,4,6]
```

#### `SuperIterable#drop(count)`

Drops the first `count` values.

```js
_([1,2,3,1,2])
  .drop(2); //=> [3,1,2]
```

#### `SuperIterable#dropWhile(pred)`

Drops the elements while `pred(element)` is true.

```js
_([2,4,6,5,4])
  .dropWhile(x => x % 2 == 0); //=> [5,4]
```

#### `SuperIterable#count()`

Counts the elements.

```js
_([1,2,3,4])
  .count(); //=> 4
```

#### `SuperIterable#concat(...otherIterables)`

Concatenates Iterables.

```js
_([1,2,3,4])
  .concat([5,6,7], new Set([8,9])); //=> [1,2,3,4,5,6,7,8,9]
```

#### `SuperIterable#every(pred)`

Returns true if every elements fulfills `pred` condition, otherwise false.

```js
_([2,4,6])
  .every(x => x % 2 == 0); //=> true
```

```js
_([1,2,3])
  .every(x => x % 2 == 0); //=> false
```

#### `SuperIterable#some(pred)`

Returns true if at least one element fulfills `pred` condition, otherwise false.

```js
_([1,2,3])
  .some(x => x % 2 == 0); //=> true
```

```js
_([1,3,5])
  .some(x => x % 2 == 0); //=> false
```

#### `SuperIterable#find(pred)`

Returns the element that fulfills `pred` condition, otherwise undefined.

```js
_([1,4,3])
  .find(x => x % 2 == 0); //=> 4
```

```js
_([1,5,3])
  .find(x => x % 2 == 0); //=> undefined
```

#### `SuperIterable#findIndex(predicate)`

Returns the index of the element that fulfills `pred` condition, otherwise -1.

```js
_([1,4,3])
  .find(x => x % 2 == 0); //=> 1
```

```js
_([1,5,3])
  .find(x => x % 2 == 0); //=> -1
```

#### `SuperIterable#indexOf(elem)`

Returns the index of the element, or returns -1 if it is not found.

```js
_([1,4,3])
  .indexOf(4); // => 1
```

```js
_([1,5,3])
  .indexOf(4); // => -1
```

#### `SuperIterable#includes(elem)`

Returns whether the `SuperIterable` includes the element or not.

```js
_([1,4,3])
  .includes(4); // => true
```

```js
_([1,5,3])
  .includes(4); // => false
```

#### `SuperIterable#keys()`

Returns the "keys" (the first values of element pairs).

```js
_([[1,2],[3,4],[5,6]])
  .keys(); //=> [1,3,5]
```

#### `SuperIterable#values()`

Returns the "values" (the second values of element pairs).

```js
_([[1,2],[3,4],[5,6]])
  .keys(); //=> [2,4,6]
```

#### `SuperIterable#toArray()`

Converts the `SuperIterable` into an Array.

```js
_([1,2,3,4])
  .toArray(); //=> [1,2,3,4]
```
