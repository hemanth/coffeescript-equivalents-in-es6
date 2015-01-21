# CoffeeScript equivalents in ECMAScript 6 
> Inspiration from [es6-equivalents-in-es5](https://github.com/addyosmani/es6-equivalents-in-es5)

*Please note this document is very much a work in progress. Contributions are welcome.*


## Arrow Functions

```coffee
square = (x) -> x * x
```

```js
let square = (x) => x * x;
// More of an abuse, arrow functions are useful for lexical scope.
```

## Splats

```coffee
race = (winner, runners...) ->
  console.log winner, runners
```

```js
let race = (winner, ...runners) =>
    console.log(winner, runners);
```

## Array Comprehensions

```coffee
numbers = [ 1, 2, 3 ];
(Math.sqrt num for num in numbers)
```

```js
var numbers = [ 1, 2, 3 ];
[for (num of numbers) Math.sqrt(num)];
```

## Default Params 

```coffee
log = (message, level='log') -> console[level](message)
```

```js
let log = (message, level = 'log') => console[level](message);
```
