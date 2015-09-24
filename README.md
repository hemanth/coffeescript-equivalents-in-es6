# CoffeeScript equivalents in ECMAScript 6 [(now ECMAScript 2015)](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
> Inspiration from [es6-equivalents-in-es5](https://github.com/addyosmani/es6-equivalents-in-es5)

*Note: a few of these examples are taken from CoffeeScript's own website.*

## Topics

- [Statements as Expressions](#statements-as-expressions)
- [Arrow Functions](#arrow-functions)
- [Splats](#splats)
- [Array Comprehensions](#array-comprehensions)
- [Default Params](#default-params)
- [String interpolation / Template strings](#string-interpolation--template-strings)
- [Lexical Scope and Variable safety.](#lexical-scope-and-variable-safety)
- [Function binding](#function-binding)
- [Classes, Inheritance, and Super](#classes-inheritance-and-super)
- [Destructuring Assignment](#destructuring-assignment)
- [Array Slicing](#array-slicing)
- [Array Splicing](#array-splicing)
- [Ranges](#ranges)
- [Chained Comprehensions](#chained-comprehensions)
- [Block Regular Expressions](#block-regular-expressions)
- [Operators and Aliases](#operators-and-aliases)
- [Generators](#generators)
- [Loops and Iteration](#loops-and-iteration)
- [Switch Statements](#switch-statements)

## Statements as Expressions

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#expressions)

```coffee
grade = (student) ->
  unless student?
    throw new Error 'student is required'
  else if student.excellentWork
    'A+'
  else if student.okayStuff
    if student.triedHard then 'B' else 'B-'
  else
    'C'

eldest = if 24 > 21 then 'Liz' else 'Ike'
```

ES6 equivalent:

```js
// There is no exact equivalent, but there are ternary expressions for common
// cases like above.
function grade(student) {
  if (student == null) {
    throw new Error('student is required')
  } else if (student.excellentWork) {
    return 'A+';
  } else if (student.okayStuff) {
    return student.triedHard ? 'B' : 'B-';
  } else {
    return 'C';
  }
}

let eldest = 24 > 21 ? 'Liz' : 'Ike';
```

*Note: you might want to watch [discussion on do-expressions](https://www.google.com/search?q=es7+"do+expressions") if you are interested in this feature. Note that the code below may not reflect the proposal in its current state.*

```js
// With current do-expression proposal, stage 0
let grade = student => do {
  if (student == null) {
    throw new Error('student is required')
  } else if (student.excellentWork) {
    'A+';
  } else if (student.okayStuff) {
    student.triedHard ? 'B' : 'B-';
  } else {
    'C';
  }
}
```

## Arrow Functions

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#literals)

```coffee
readConfig = (file, parse) ->
  new Promise (resolve) ->
      fs.readFile file, 'utf8', (err, data) =>
        if err?
          reject err
        else
          resolve parse data
```

ES6 equivalent: [![es6-doc](./js.png)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

```js
function readConfig(file, parse) {
  return new Promise(resolve =>
    fs.readFile(file, 'utf8', (err, data) =>
      err != null ?
        reject(err) :
        resolve(parse(data)));
}
```

## Splats

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#splats)

```coffee
race = (winner, runners...) ->
    console.log winner, runners

race 'Bob', runnerList...
```

ES6 equivalent: [![es6-doc](./js.png)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)

```js
let race = (winner, ...runners) =>
    console.log(winner, runners);

race('Bob', ...runners);
```

## Array Comprehensions

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#loops)

```coffee
numbers = [ 1, 4, 9 ];
(Math.sqrt num for num in numbers)
# -> [1, 2, 3]
```

ES6 equivalent:

```js
let numbers = [ 1, 4, 9 ];
numbers.map(num => Math.sqrt(num));
// -> [1, 2, 3]
```

ES7 equivalent: [![es6-doc](./js.png)](http://babeljs.io/docs/learn-es2015/#comprehensions)

```js
let numbers = [ 1, 4, 9 ];
[for (num of numbers) Math.sqrt(num)];
// -> [1, 2, 3]
```

## Default Params

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#literals)

```coffee
log = (message, level='log') -> console[level](message)
```

ES6 equivalent: [![es6-doc](./js.png)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)

```js
let log = (message, level = 'log') => console[level](message);
```

## String interpolation / Template strings

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#strings)

```coffee
container = 'mug'
liquid = 'hot chocolate'
console.log "Filling the #{container} with #{liquid}..."
# -> "Filling the mug with hot chocolate..."
```

ES6 equivalent: [![es6-doc](./js.png)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/template_strings)

```js
let container = 'mug'
   ,liquid = 'hot chocolate';
console.log(`Filling the ${container} with ${liquid}...`);
// -> "Filling the mug with hot chocolate..."
```

## Lexical Scope and Variable safety.

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#lexical-scope)

```coffee
inner = 10
value = 20

# literally an IIFE
do (inner = 5, value = 10) ->
  console.log inner # 5

console.log inner # 10
```

ES6 equivalent: [![es6-doc](./js.png)](http://babeljs.io/docs/learn-es2015/#let-const) ([`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), [`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const))

```js
let inner = 10;
const value = 20;

// A simple block
{
  let inner = 5;
  
  // not an error, despite being a constant in the outer scope
  const value = 10;
  console.log(inner); // 5
}

console.log(inner); // 10
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
let hero = {
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

ES6 equivalent: [![es6-doc](./js.png)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/class)

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

## Array Slicing

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#slices)

```coffee
part1 = list[0..3]  # with end
part2 = list[0...3] # without end
part3 = list[..3]   # start defaults to 0
part4 = list[3..]   # end defaults to length

clone = list[..] # Clone the array
```

```js
let part1 = list.slice(0, 3);
let part2 = list.slice(0, 4);
let part3 = list.slice(0, 3);
let part4 = list.slice(3);

// More than one way to clone an array
let clone;
clone = list.slice();
clone = list.concat();
```

## Array Splicing

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#slices)

```coffee
numbers[3..6] = [-3, -4, -5, -6]
numbers[3..6] = list
```

ES6 equivalent:

```js
numbers.splice(3, 4, -3, -4, -5, -6)
[].splice.apply(numbers, [3, 4].concat(list))
```

## Ranges

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#loops)

```coffee
numbers = [0...5]
positives = [1...10]
```

ES6 equivalent:

```js
let numbers = Array(5).map((_, i) => i);
let positives = Array(10).slice(1).map((_, i) => i);

// Or, you can make a custom range function (which is faster)
function range(start, end) {
  if (end == null) [start, end] = [0, start];
  const ret = [];
  while (start < end) ret.push(start++);
  return ret;
}
```

## Chained Comparisons

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#comparisons)

```coffee
cholesterol = 127

healthy = 60 < cholesterol < 200
```

ES6 equivalent:

```js
let cholestrol = 127;

let healthy = 60 < cholestrol && cholestrol < 200;
```

## Block Regular Expressions

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#regexes)

```coffee
OPERATOR = /// ^ (
  ?: [-=]>             # function
   | [-+*/%<>&|^!?=]=  # compound assign / compare
   | >>>=?             # zero-fill right shift
   | ([-+:])\1         # doubles
   | ([&|<>])\2=?      # logic / shift
   | \?\.              # soak access
   | \.{2,3}           # range or splat
) ///
```

ES6 equivalent:

```js
// There isn't really any. This doesn't get the engine-related caching that
// regex literals often get.
let OPERATOR = new RegExp('^(' +
  '?:[-=]>' +             // function
   '|[-+*/%<>&|^!?=]=' +  // compound assign / compare
   '|>>>=?' +             // zero-fill right shift
   '|([-+:])\\1' +        // doubles
   '|([&|<>])\\2=?' +     // logic / shift
   '|\\?\\.' +            // soak access
   '|\\.{2,3}' +          // range or splat
')')
```

## Operators and Aliases

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#operators)

- True: `true`, `yes`, `on`
- False: `false`, `no`, `off`
- And: `a && b`, `a and b`
- Or: `a || b`, `a or b`
- Not: `!a`, `not a`
- Equality: `a == b`, `a is b`
- Inequality: `a != b`, `a isnt b`
- Current Instance: `@`, `this`
- Instance Property: `@prop`, `this.prop`
- Contains Property: `prop of object`
- Contains Entry: `a in b`
- Exponentiation: `x ** y`
- Floor Division: `x // y`
- Always-positive Modulo: `x %% y`

ES6 Equivalents: [![es6-doc](./js.png)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)

- True: `true`
- False: `false`
- And: `a && b`
- Or: `a || b`
- Not: `!a`
- Equality: `a === b`
- Inequality: `a !== b`
- Current Instance: `this`
- Instance Property: `this.prop`
- Contains Property: `prop in object`
- Contains Entry: `a.indexOf(b) >= 0`
  - For strings (and arrays in ES7) only: `a.includes(b)`
  - For iterables in ES6 (such as some DOM nodes): `Array.from(a).indexOf(b) >= 0`
- Exponentiation: `Math.exp(x, y)`
  - ES7: `x ** y`
- Always-positive Modulo: `(a % b + b) % b`

## Generators

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#fat-arrow)

```coffee
lazyRange = (n) ->
  for i til n
    yield i

identity = (iter) ->
  yield from iter

range = lazyRange 10
range = identity range
```

ES6 equivalent: [![es6-doc](./js.png)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)

```js
function *lazyRange(n) {
  for (let i = 0; i < n; i++) {
    yield i;
  }
}

function* identity(iter) {
  yield* iter
}

let range = lazyRange(10);
range = identity(range);
```

## Loops and Iteration

CoffeeScript: [![cs-doc](./cs.png)](http://coffeescript.org/#loops)

```coffee
isOkay = (entry) ->
  # ...

# numbers 0-9
console.log i for i in [0..10]

# evens 0-8
console.log i for i in [0..10] by 2

node = getFirst()
node = node.next while isOkay node

console.log i for i in list

console.log i for i in list when isOkay i

# own object properties
for own prop, value of object
  console.log prop
  console.log object[prop]
```

ES6 equivalent: [![es6-doc](./js.png)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)

```js
function isOkay(entry) {
  // ...
}

// Standard `for` loops
for (let i = 0; i < 10; i++) {
  console.log(i);
}

for (let i = 0; i < 10; i += 2) {
  console.log(i);
}

let node = getFirst();
while (isOkay(node)) {
  node = node.next;
}

// `for ... of` loops
for (let i of list) {
  console.log(i);
}

for (let i of list) {
  if (isOkay(i)) {
    console.log(i);
  }
}

for (let prop of Object.keys(object)) {
  console.log(prop);
  console.log(object[prop]);
}

// Array methods (last three)
Array.from(list).forEach(i => console.log(i))

Array.from(list).filter(isOkay).forEach(i => console.log(i));

Object.keys(object).forEach(prop => {
  console.log(prop);
  console.log(object[prop]);
});
```

## Switch Statements

```coffee
switch day
  when "Mon" then go work
  when "Tue" then go relax
  when "Thu" then go iceFishing
  when "Fri", "Sat"
    if day is bingoDay
      go bingo
      go dancing
  when "Sun" then go church
  else go work
```

ES6 Equivalent: [![es6-doc](./js.png)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch)

```js
switch (day) {
case "Mon": go("work"); break;
case "Mon": go("work"); break;
case "Tue": go("relax"); break;
case "Thu": go("iceFishing"); break;
case "Fri": case "Sat":
  if (day === "bingoDay") {
    go("bingo");
    go("dancing");
  }
  break;
case "Sun": go("church"); break;
default: go("work");
}
```

## Contributors

- [hemanth](http://www.h3manth.com) (creator)
- [stoeffel](https://github.com/stoeffel)
- [impinball](https://github.com/impinball)
