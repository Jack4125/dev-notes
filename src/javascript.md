# JavaScript

![Definition of synchronous and asynchronous](/images/syn-asyn.PNG)

[Read more](https://medium.com/better-programming/is-javascript-synchronous-or-asynchronous-what-the-hell-is-a-promise-7aa9dd8f3bfb)

[Regex Cheatsheet](https://www.rexegg.com/regex-quickstart.html)

## General

- 6 Primitives: number, string, boolean, `null`, `undefined`, Symbol
- 1 Special Type: Object
- Primitives are copied directly. Objects are copied _by reference_ (address in memory).

- Array Methods

slice <==> filter, map
splice <==> forEach

- String Method

split <==> just split string, not related to Array methods

## `var` `const` `let`

- `var` is function scoped. When in loops, it would go outside the loop's scope to the functional scope. Most computer languages don't work this way. This is why you should always put everything in IFFEs if you user `var`, because declarations would get attached to "Window" or "Global" objects.

- `let` and `const` are block scoped. They do not attach to global variables. The are

### Type Conversion:

- `String(value)`

  - `String(value)`

    ![Number Conversion](/images/numeric-conversion.PNG)

- `Number(value)`

  - `NaN` is returned if value cannot be converted to number..

- `Boolean(value)`

  ![Boolean conversion rules](/images/boolean-conversion.PNG)

  - `"0"` is `true`

- `Infinity` and `-Infinity` can be used directly, or as a result of dividing number by 0.
  - `NaN`
    - Result of undefined mathematical operation between different data types.
    - Sticky - propagates to the whole result.

### Operators

- `%` (Binary) = Remainder
- `**` (Binary) = Exponent
- `++/--` (Unary)
  - Prefix Form `++variable` - add 1 to variable and return new value.
  - Postfix Form `variable++` - return variable and add 1 to it.
- `?` (Ternary)
  - Do not use as substitute for all `if` statements.
- Logical Operators
  - `!` (Not)
    - `!!` converts value to boolean type.
    - Higher precedence than `&&` and `||`.
  - `||` (Or)
    - If **_ANY_** operands are `true`, returns `true`; otherwise return `false`
    - (From left to right) Returns the first `truthy` value (original value before boolean conversion), or the last operand.
  - `&&` (And)
    - If **_ALL_** operands are `true`, return `true`; otherwise return `false`
    - Returns the first `falsy` value, or the last operand.
  - `&&` executes before `||` due to higher precedence.

### Comparison

- String
  - `'a' > 'A'` is `true`
  - `'az' > 'aa'` is `true`
  - `'aa' > 'a'` is `true`
- Different Types
  - all values are converted to _numbers_ or truthy/falsy _numbers_ when comparing.
  - `null == undefined` is `true`, but `null === undefined` is `false`

### Loops

- `continue` directive moves loop onto next iteration.
- `break` directive ends loop
- `label` makes execution jumps to a specific outer loop.

- `for` loop execution order:
  1. initial
  2. condition
  3. **_body_**
  4. step (step is always executed at the end)
- If `initial` is declared within loop, it is bound within the scope of the loop. But if declared outside, it can be modified by actions performs inside the loop.
- `switch (condition) { case (match): break;}`
  - `match` case matters.
  - Without `break` at the end of each `case`, execution will move on to next `case`.

## Functions

A function expression is very similar to and has almost the same syntax as a function declaration (see function statement for details). The main difference between a function expression and a function declaration is the function name, which can be omitted in function expressions to create anonymous functions. A function expression can be used as an IIFE (Immediately Invoked Function Expression) which runs as soon as it is defined. See also the chapter about functions for more information.

![Constructor Function example](/images/constructor.png)
![Constructor Function example 2](/images/constructor-2.png)

- Function Declaration: `function name() {...}`
  - Created at the beginning of execution flow, so they can be called anywhere.
- Function Expression: `let name = function() {...};`
  - Semicolon at the end!
  - _Expressions_ are created when execution flow reaches it, so it is not available before then.
- Arrow Function: `let name = () => {...}`
- Anonymous Functions: `function() {...}`
- Naming:
  - Start with a verb, since functions should be _actions_.
  - show... get... calc... create... check...
- Function names are special "values", so they can be copied onto another variable.
- Callback Functions: (functional) arguments of a function - these arguments are expected to be "called back" from inside the function.
- Scope:
  - Local variable: A variable declared _inside_ a function is only visible inside that function.
  - Outer variable: A variable declared _outside_ a function can be accessed and modified inside that function.
  - Same scope rules apply for _functions_ declared inside/outside another function.
  - Global variable: Try to avoid global variables, and only use for storing project-level data.
- Default Parameter: `func(param="default value")`
- Functions should `return`. If there is no `return` or nothing is returned, then `undefined` is returned by default.
- [passing both event and parameter](https://github.com/riot/riot/issues/1001)

## Closure

- [Closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/

A [closure](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36) is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). In other words, a closure gives you access to an outer function’s scope from an inner function. This scope consists of any local variables that were in-scope at the time the closure was created.

In JavaScript, closures are created every time a function is created, at function creation time.
To use a closure, define a function inside another function and expose it. To expose a function, return it or pass it to another function.
The inner function will have access to the variables in the outer function scope, even after the outer function has returned.

closures are the primary mechanism used to enable data privacy. When you use closures for data privacy, the enclosed variables are only in scope within the containing (outer) function. You can’t get at the data from an outside scope except through the obj ect’s privileged methods.

[Use closures for privacy](https://itnext.io/javascript-closure-for-privacy-8a40c274192e)

```js
function makeFunc() {
  var name = 'Mozilla';
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();

// When myFunc is invoked, the variable 'name' remains available for use and "Mozilla" is passed to alert.
myFunc();
```

```js
function makeAdder(x) {
  return function (y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
console.log(add5(2)); // 7

var add10 = makeAdder(10);
console.log(add10(2)); // 12

// In essence, makeAdder is a function factory. `add5` and `add10` are both closures.
// They share the same function body definition, but store different lexical environments.
```

## Fetch API

- ReferenceError: fetch is not defined.

  > Fetch API is not defined in Node, only in browsers

- `Body.json()` is asynchronous and returns a Promise object that resolves to a JavaScript object.

- `JSON.parse()` is synchronous can parse a string and change the resulting returned JavaScript object.

- `JSON.stringify()` is synchronous and can turn a JavaScript object into string.

- 'AJAX' works with 'callbacks'; 'fetch' works with 'promises'.

```js
// fetch GET
fetch('https://jsonplaceholder.typicode.com/users')
  .then((objectResponse) => {
    // convert promise object to JSON
    return response.json();
  })
  .then((jsonResponse) => {
    // code to execute with jsonResponse
    return;
  });

// fetch POST
fetch('https://jsonplaceholder.typicode.com/users', {
  method: 'POST',
  headers: {
    'Content-type': 'application/json',
    apiKey: '123456',
  },
  // Can't send JSON through HTTP. Must convert to string
  body: JSON.stringify({ id: '123' }),
})
  .then(...);

// Async GET Syntax
async function getData() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users');
    const jsonResponse = await response.json();
    return jsonResponse;
  } catch (err) {
    console.log(err);
  }
}

fetch('./sample.txt')
  .then((res) => {
    // ERROR: Uncaught (in promise) TypeError: Failed to execute 'text' on 'Response': body stream is locked
    // (https://stackoverflow.com/questions/54082327/why-does-logging-the-result-of-fetch-break-it-body-stream-is-locked)
    return res.text();
  })
  .then((data) => {
    return data;
  });

fetch('./sample.json')
  .then((res) => res.json()) // return the json part of the res (none of the other stuff like header.whatever)
  .then((data) => {
    return data;
  });
```

## Object

- `obj.key` vs `obj[key]`
  - `.` does not allow space in keys. (i.e. `"key one"`)
  - Computed Properties: `[]` can be used to add \_variables into objects.
  - `[]` can also have complex expressions inside.
- `delete obj.key` deletes obj.key.
- Check property existence:
  - `(obj.key === undefined)`
  - `("key" in obj)` - must use quotes around key.
- `for (let key in obj) {...}` loops through "obj" with "key" as each entry.
- Ordering of keys is based on creation order, except those with _Integers Properties_ (a string that can be converted to-and-from and integer without change) are sorted (ascending).
- Objects are copied _by reference_ (address in memory). Modifying object in one place changes it for all places.
- Creating [independent copy](https://javascript.info/object#cloning-and-merging-object-assign) of an object (not just reference) is usually unnecessary, but can be done with `Object.assign()` or "Deep Cloning".

- `for...in` loops through "object's properties (not methods)"

  ```js
  for (let i in myObj) {
    // code
  }
  ```

- get all keys in object

  ```js
  const keys = Object.keys(myObj);
  // 'keys' is an array of property and method names
  ```

- `in` operator checks if a property exists in an object

  ```js
  if ('age' in myObj) {
    // code
  }
  ```

```js
let user = {
  name: 'Jack',
  // _ is "private property" naming convention
  // Should not be modified by outside forces
  _level: 99,
  getMoney() {
    console.log('Received $1000!');
  },
  // Avoid using arrow functions when using 'this' in a method!
  // Arrow functions inherently bind an already defined 'this'
  // value to the function itself that is NOT the calling object.
  // The value of 'this' is the global object, which doesn’t have
  // your self-defined property and therefore returns undefined.
  getAge() {
    console.log(this.age);
  },
  // getter/setter
  get userLevel() {
    // why use getter:
    // Perform action on data while getting it. Return based on BUILT-IN conditions.
    // * Properties cannot share the same name as the getter/setter function.
    if (this._level < 50) {
      return this._level;
    } else {
      return 'Cannot get user level. It is too high for you to see.';
    }
  },
  set userLevel(newLevel) {
    if (typeof newLevel === 'number') {
      this._level = newLevel;
    } else {
      return 'Level must be a number.';
    }
  },
};

user.getMoney();
user.getAge();

// Do not need to be called with a set of parentheses.
// Call getter
user.userLevel;
// Call setter
user.userLevel = 42;
```

## Advanced Objects

```js
// Constructor Function
function ObjectFactory(input) {
  this.input = input;
  this.printInput = function () {
    console.log(this.input);
  };
  return;
}
// 'new' operator creates empty object, and sets 'this' to point to
// the newly created object.
// Without 'new', 'this' refers to 'Window' in browser, or 'Global' in Node.
const another = new ObjectFactory('hello');

// Factory Function
function objectFactory(input) {
  return {
    input,
    printInput: function () {
      console.log(this.input);
    },
  };
}
const object = objectFactory('hello');
object.printInput();

// closure
// private objects
function Circle(radius) {
  this.radius = radius;

  // use variable declaration instead of 'this' to privatize things

  let defaultLocation = { x: 0, y: 1 };
  let computeLocation = function (factor) {
    // ...
  };

  // closure
  this.draw = function () {
    // x and y's scopes are limited to 'draw' function, and are gone when function is done.
    // 'draw' is able to access all local variables, AND ALL VARIABLES OF ITS PARENT FUNCTION
    // like 'defaultLocation' and 'computeLocation'.
    // So we say:
    // 'defaultLocation' and 'computeLocation' are not within the 'scope' of 'draw', but they are
    // within the 'closure' of its parent function 'Circle'
    // 'scope' is temporary; 'closure' stays.
    let x, y;
    console.log(this.radius); // use 'this' when referencing properties
    computeLocation(0.1); // don't need to use 'this' when referencing variables.
    console.log('draw');
  };

  // getter and setter
  Object.defineProperty(this, 'defaultLocation', {
    get: function () {
      return defaultLocation;
    },
    set: function (value) {
      if (!value.x || !value.y) throw new Error('Invalid location');
      defaultLocation = value;
    },
  });
}

let circle = new Circle(10);
// circle.defaultLocation = {x: 5, y: 5};
// Does not work, because defaultLocation is not a property or method
circle.draw();
```

## Class

```js
/* 
Class method and getter/setting syntax is the same as
for objects, except you can not include commas between methods. 
*/
class Animal {
  // constructor is invoked every time a new instance of Dog class is created
  // parameter of constructor is the parameter passed in when creating new instance
  constructor(name) {
    this._name = name;
    this._behavior = 0;
  }

  /* 
  getters/setters:
  (1) securing access to data properties
  (2) adding extra logic (if-else checks) to properties before getting or setting their values.
  */
  get name() {
    return this._name;
  }

  get behavior() {
    return this._behavior;
  }

  incrementBehavior() {
    this._behavior++;
  }

  /*
  Static Methods
  Sometimes you will want a class to have methods that aren’t available 
    in individual instances, but that you can call directly from the class.
  Ex: Take the Date class, for example — you can both create Date instances 
    to represent whatever date you want, and call static methods, like 
    Date.now() which returns the current date, directly from the class. 
    The .now() method is static, so you can call it directly from the class, 
    but not from an instance of the class.
  */
  static generateName() {
    const names = ['Angel', 'Spike', 'Buffy', 'Willow', 'Tara'];
    const randomNumber = Math.floor(Math.random() * 5);
    return names[randomNumber];
  }
}

// Class Cat
class Cat extends Animal {
  constructor(name, usesLitter) {
    /* 
    The super keyword calls the constructor of the parent class. In this 
    case, super(name) passes the name argument of the Cat class to the 
    constructor of the Animal class. When the Animal constructor runs, 
    it sets this._name = name; for new Cat instances. 
    */
    super(name);
    // '_usesLitter' is a new property that is unique to the Cat
    // class, so we set it in the Cat constructor.
    this._usesLitter = usesLitter;
  }

  get usesLitter() {
    return this._usesLitter;
  }
}

// Class Dog
class Dog extends Animal {
  constructor(name) {
    super(name);
  }
}

// The syntax for calling methods and getters on an instance is the same as calling them on an object — append the instance with a period, then the property or method name. For methods, you must also include opening and closing parentheses.

let nikko = new Dog('Nikko'); // Create dog named Nikko

const nikko = new Cat('Nikko', false); // Create cat named Nikko
let bradford = new Dog('Bradford'); // Create dog name Bradford

nikko.incrementBehavior(); // Add 1 to nikko instance's behavior

console.log(nikko.behavior); // Logs 1 to the console
console.log(bradford.behavior); // Logs 0 to the console

// call static method
console.log(Animal.generateName()); // returns a name
```

## [Module](https://v8.dev/features/modules)

```js
// Modules only work in Node, not in browser's script files
module.exports = menu;
module.exports = { menu1, menu2, menu3 };

let menu = require('./module');

// ES6
export default menu;
export { menu1, menu2, menu3 };

import menu from './module';
import { menu1, menu2, menu3 } from './module';
```
