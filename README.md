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
- [Destructuring Assignment](#destructuring-assignment)

## Arrow Functions

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#literals)

```coffee
square = (x) -> x * x
```

ES6 equivalent: [![es6-doc](./js.png)](http://tc39wiki.calculist.org/es6/arrow-functions/)

```js
let square = (x) => x * x;
// More of an abuse, arrow functions are useful for lexical scope.
```

## Splats

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#splats)

```coffee
race = (winner, runners...) ->
    console.log winner, runners
```

ES6 equivalent: [![es6-doc](./js.png)](http://tc39wiki.calculist.org/es6/rest-parameters/)

```js
let race = (winner, ...runners) =>
    console.log(winner, runners);
```

## Array Comprehensions

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#loops)

```coffee
numbers = [ 1, 4, 9 ];
(Math.sqrt num for num in numbers)
# -> [1, 2, 3]
```

ES7 equivalent: [![es6-doc](./js.png)](http://tc39wiki.calculist.org/es6/array-comprehensions/)

___Proposed in ES6 but deferred to ES7.___

```js
var numbers = [ 1, 4, 9 ];
[for (num of numbers) Math.sqrt(num)];
// -> [1, 2, 3]
```

## Default Params

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#literals)

```coffee
log = (message, level='log') -> console[level](message)
```

ES6 equivalent: [![es6-doc](./js.png)](http://tc39wiki.calculist.org/es6/default-parameter-values/)

```js
let log = (message, level = 'log') => console[level](message);
```

## String interpolation / Template strings

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#strings)

```coffee
container = 'mug'
liquid = 'hot chocolate'
console.log "Filling the #{container} with #{liquid}..."
# -> Filling the mug with hot chocolate..."
```

ES6 equivalent: [![es6-doc](./js.png)](http://tc39wiki.calculist.org/es6/template-strings/)

```js
let container = 'mug'
   ,liquid = 'hot chocolate';
console.log(`Filling the ${container} with ${liquid}...`);
// -> Filling the mug with hot chocolate..."
```

## Lexical Scope and Variable safety.

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#lexical-scope)

```coffee
outer = 1
changeNumbers = ->
  inner = -1
  outer = 10
inner = changeNumbers()
# inner will be 10.
```

ES6 equivalent: [![es6-doc](./js.png)](http://tc39wiki.calculist.org/es6/block-scoping/)

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

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#fat-arrow)

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

ES6 equivalent: [![es6-doc](./js.png)](http://tc39wiki.calculist.org/es6/arrow-functions/)

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

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#classes)

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

ES6 equivalent: [![es6-doc](./js.png)](http://tc39wiki.calculist.org/es6/classes/)

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

## Destructuring Assignment

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#destructuring)

```coffee
hero =
  name: "Spider-Man"
  alterEgo: "Peter Benjamin Parker"
  enemies: ["Electro", "Doctor Octopus"]

{name, alterEgo} = hero
# name = "Spider-Man"
# alterEgo = "Peter Benjamin Parker"

[head, tail...] = [1, 2, 3, 4, 5]
# head = 1
# tail = [2, 3, 4, 5]
```

ES6 equivalent: [![es6-doc](./js.png)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

```js
let hero = {
  name: "Spider-Man",
  alterEgo: "Peter Benjamin Parker",
  enemies: ["Electro", "Doctor Octopus"]
};

let {name, alterEgo} = hero;
// name = "Spider-Man"
// alterEgo = "Peter Benjamin Parker"

let [head, ...tail] = [1, 2, 3, 4, 5];
// head = 1
// tail = [2, 3, 4, 5]
```


## Contributors

- [hemanth](http://www.h3manth.com) (creator)
- [stoeffel](https://github.com/stoeffel)
