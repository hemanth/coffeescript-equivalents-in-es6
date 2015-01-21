# CoffeeScript equivalents in ECMAScript 6
> Inspiration from [es6-equivalents-in-es5](https://github.com/addyosmani/es6-equivalents-in-es5)

*Please note this document is very much a work in progress. Contributions are welcome.*


## Arrow Functions

CoffeeScript:

```coffee
square = (x) -> x * x
```

ES6 equivalent:

```js
let square = (x) => x * x;
// More of an abuse, arrow functions are useful for lexical scope.
```

## Splats

CoffeeScript:

```coffee
race = (winner, runners...) ->
    console.log winner, runners
```

ES6 equivalent:

```js
let race = (winner, ...runners) =>
    console.log(winner, runners);
```

## Array Comprehensions

CoffeeScript:

```coffee
numbers = [ 1, 2, 3 ];
(Math.sqrt num for num in numbers)
```

ES7 equivalent:

___Proposed in ES6 but deferred to ES7.___

```js
var numbers = [ 1, 2, 3 ];
[for (num of numbers) Math.sqrt(num)];
```

## Default Params

CoffeeScript:

```coffee
log = (message, level='log') -> console[level](message)
```

ES6 equivalent:

```js
let log = (message, level = 'log') => console[level](message);
```

## String interpolation / Template strings

CoffeeScript:

```coffee
container = 'mug'
liquid = 'hot chocolate'
console.log "Filling the #{container} with #{liquid}..."
```

ES6 equivalent:

```js
let container = 'mug'
   ,liquid = 'hot chocolate';
console.log(`Filling the ${container} with ${liquid}...`);
```

