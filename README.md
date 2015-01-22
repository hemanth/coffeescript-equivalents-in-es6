# CoffeeScript equivalents in ECMAScript 6
> Inspiration from [es6-equivalents-in-es5](https://github.com/addyosmani/es6-equivalents-in-es5)

*Please note this document is very much a work in progress. Contributions are welcome.*

## Topics

- [Arrow Functions](#arrow-functions)
- [Splats](#splats)
- [Array Comprehensions](#array-comprehensions)
- [Default Params](#default-params)
- [String interpolation / Template strings](#string-interpolation--template-strings)
- [Lexical Scope and Variable safety.](#lexical-scope-and-variable-safety)
- [Function binding](#function-binding)
- [Classes, Inheritance, and Super](#classes-inheritance-and-super)

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
numbers = [ 1, 4, 9 ];
(Math.sqrt num for num in numbers)
# -> [1, 2, 3]
```

ES7 equivalent:

___Proposed in ES6 but deferred to ES7.___

```js
var numbers = [ 1, 4, 9 ];
[for (num of numbers) Math.sqrt(num)];
// -> [1, 2, 3]
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
# -> Filling the mug with hot chocolate..."
```

ES6 equivalent:

```js
let container = 'mug'
   ,liquid = 'hot chocolate';
console.log(`Filling the ${container} with ${liquid}...`);
// -> Filling the mug with hot chocolate..."
```

## Lexical Scope and Variable safety.

CoffeeScript:

```coffee
outer = 1
changeNumbers = ->
  inner = -1
  outer = 10
inner = changeNumbers()
# inner will be 10.
```

ES6 equivalent:

```js
let outer = 1;
let changeNumbers = () => {
  let inner = -1;
  return outer = 10;
}
let inner = changeNumbers();
// inner will be 10
```

## Function binding

CoffeeScript:

```coffee
hero =
  name: "Batman"
  alterEgo: "Bruce Wayne"
  enemies: ['Two-Face', 'Bane']
  printEnemies: ->
    @enemies.forEach((enemy) =>
      console.log @name + " fights " + enemy);

hero.printEnemies()
# -> "Batman fights Two-Face"
# -> "Batman fights Bane"
```

ES6 equivalent:

```js
var hero = {
  name: "Batman",
  alterEgo: "Bruce Wayne",
  enemies: ['Two-Face', 'Bane'],
  printEnemies() {
    this.enemies.forEach(enemy =>
      console.log(this.name + " fights " + enemy));
  }
};

hero.printEnemies();
// -> "Batman fights Two-Face"
// -> "Batman fights Bane"
```

## Classes, Inheritance, and Super

CoffeeScript:

```coffee
class Person
  constructor: (@name) ->
    @movement = "walks"

  move: (meters) ->
    console.log "#{@name} #{@movement} #{meters}m."

class Hero extends Person
  constructor: (@name, @movement) ->

  move: ->
    super 500

clark = new Person "Clark Kent"
superman = new Hero "Superman", "flies"

clark.move(100)
# -> Clark Kent walks 100m.
superman.move()
# -> Superman flies 500m.
```

ES6 equivalent:

```js
class Person {
  constructor(name) {
    this.name = name;
    this.movement = "walks";
  }

  move(meters) {
    console.log(`${this.name} ${this.movement} ${meters}m.`);
  }
}

class Hero extends Person {
  constructor(name, movement) {
    this.name = name;
    this.movement = movement;
  }

  move() {
    super.move(500);
  }
}

let clark = new Person("Clark Kent");
let superman = new Hero("Superman", "flies");

clark.move(100);
// -> Clark Kent walks 100m.
superman.move();
// -> Superman flies 500m.
```
