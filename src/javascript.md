---
title: "JavaScript"
tags: "JavaScript"
---

# JavaScript

## `var` `const` `let`

- `var` is function scoped.

- `let` and `const` are block scoped.

- `var` in loops would go outside the loop's scope to the functional scope. Most computer languages don't work this way. This is why you should always put everything in IFFEs if you user `var`,
  because declarations would get attached to "Window" or "Global" objects.
- `let` and `const` do not attach to global variables.

## Closure

A closure is the combination of a function and the lexical environment within which that function was declared. This environment consists of any local variables that were in-scope at the time the closure was created. In this case, myFunc is a reference to the instance of the function displayName created when makeFunc is run.
The instance of displayName maintains a reference to its lexical environment, within which the variable name exists. For this reason, when myFunc is invoked, the variable name remains available for use and "Mozilla" is passed to alert.

```js
function makeFunc() {
  var name = "Mozilla"
  function displayName() {
    alert(name)
  }
  return displayName
}

var myFunc = makeFunc()
myFunc()
```

closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). In other words, a closure gives you access to an outer function’s scope from an inner function.
In JavaScript, closures are created every time a function is created, at function creation time.
To use a closure, define a function inside another function and expose it. To expose a function, return it or pass it to another function. The inner function will have access to the variables in the outer function scope, even after the outer function has returned.
closures are the primary mechanism used to enable data privacy. When you use closures for data privacy, the enclosed variables are only in scope within the containing (outer) function. You can’t get at the data from an outside scope except through the object’s privileged methods.

```js
function makeAdder(x) {
  return function (y) {
    return x + y
  }
}

var add5 = makeAdder(5)
var add10 = makeAdder(10)

console.log(add5(2)) // 7
console.log(add10(2)) // 12
```

In essence, makeAdder is a function factory — it creates functions which can add a specific value to their argument. In the above example we use our function factory to create two new functions — one that adds 5 to its argument, and one that adds 10.
add5 and add10 are both closures. They share the same function body definition, but store different lexical environments. In add5's lexical environment, x is 5, while in the lexical environment for add10, x is 10.

### Fetch API

- ReferenceError: fetch is not defined.
  Fetch API is not defined in Node, only in browsers

* Body.json() is asynchronous and returns a Promise object that resolves to a JavaScript object.
  JSON.parse() is synchronous can parse a string and change the resulting returned JavaScript object.
  JSON.stringify() is synchronous and can turn a JavaScript object into string.

* 'AJAX' works with 'callbacks'; 'fetch' works with 'promises'.

  - Use JSON.parse() to parse the response.

  - for AJAX. Use json() to parse the response for fetch.

```js
// fetch GET
fetch("https://jsonplaceholder.typicode.com/users")
  .then(objectResponse => {
    // convert promise object to JSON
    return response.json()
  })
  .then(jsonResponse => {
    // code to execute with jsonResponse
    return
  })

// fetch POST
fetch("https://jsonplaceholder.typicode.com/users", {
  method: "POST",
  headers: {
    "Content-type": "application/json",
    apiKey: "123456",
  },
  // Can't send JSON through HTTP. Must convert to string
  body: JSON.stringify({ id: "123" }),
})
  .then(response => {
    return response.json()
  })
  .then(jsonResponse => {
    return
  })

// Async GET Syntax
async function getData() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users")
    const jsonResponse = await response.json()
    return jsonResponse
  } catch (err) {
    console.log(err)
  }
}

fetch("./sample.txt")
  .then(res => {
    //console.log(res);
    //console.log(res.text());
    // ERROR: Uncaught (in promise) TypeError: Failed to execute 'text' on 'Response': body stream is locked
    // (https://stackoverflow.com/questions/54082327/why-does-logging-the-result-of-fetch-break-it-body-stream-is-locked)
    return res.text()
  })
  .then(data => {
    return data
  })

fetch("./sample.json")
  .then(res => res.json()) // return the json part of the res (none of the other stuff like header.whatever)
  .then(data => {
    return data
  })
```

### Object

```js
let user = {
  name: "Jack",
  age: 28,
  // _ is "private property" naming convention
  // Should not be modified by outside forces
  _level: 99,
  jobs: {
    first: "Google",
    second: "Yahoo",
  },
  getMoney() {
    console.log("Received $1000!")
  },
  // Avoid using arrow functions when using 'this' in a method!
  // Arrow functions inherently bind an already defined 'this'
  // value to the function itself that is NOT the calling object.
  // The value of 'this' is the global object, which doesn’t have
  // your self-defined property and therefore returns undefined.
  getAge() {
    console.log(this.age)
  },
  // getter/setter
  get userLevel() {
    // why use getter:
    // Perform action on data while getting it. Return based on BUILT-IN conditions.
    // * Properties cannot share the same name as the getter/setter function.
    if (this._level < 50) {
      return this._level
    } else {
      return "Cannot get user level. It is too high for you to see."
    }
  },
  set userLevel(newLevel) {
    if (typeof newLevel === "number") {
      this._level = newLevel
    } else {
      return "Level must be a number."
    }
  },
}

user.getMoney()
user.getAge()

// Call getter
// Setter methods like age do not need to be called with a set of parentheses.
user.userLevel

// Call setter
user.userLevel = 42

// for...in loops through "object's properties (not methods)""
for (let i in user.jobs) {
  console.log(i)
  console.log(i.company)
}

// get all keys in object
const keys = Object.keys(user)
// 'keys' is an array of property and method names

// 'in' operator checks if a property exists in an object
if ("age" in user) {
  // code
}

// Constructor Function
function ObjectFactory(input) {
  this.input = input
  this.printInput = function () {
    console.log(this.input)
  }
  return
}
// 'new' operator creates empty object, and sets 'this' to point to
// the newly created object.
// Without 'new', 'this' refers to 'Window' in browser, or 'Global' in Node.
const another = new ObjectFactory("hello")

// Factory Function
function objectFactory(input) {
  return {
    input,
    printInput: function () {
      console.log(this.input)
    },
  }
}
const object = objectFactory("hello")
object.printInput()

// closure
// private objects
function Circle(radius) {
  this.radius = radius

  // use variable declaration instead of 'this' to privatize things
  //
  let defaultLocation = { x: 0, y: 1 }
  let computeLocation = function (factor) {
    // ...
  }

  // closure
  this.draw = function () {
    // x and y's scopes are limited to 'draw' function, and are gone when function is done.
    // 'draw' is able to access all local variables, AND ALL VARIABLES OF ITS PARENT FUNCTION
    // like 'defaultLocation' and 'computeLocation'.
    // So we say:
    // 'defaultLocation' and 'computeLocation' are not within the 'scope' of 'draw', but they are
    // within the 'closure' of its parent function 'Circle'
    // 'scope' is temporary; 'closure' stays.
    let x, y
    console.log(this.radius) // use 'this' when referencing properties
    computeLocation(0.1) // don't need to use 'this' when referencing variables.
    console.log("draw")
  }

  // getter and setter
  Object.defineProperty(this, "defaultLocation", {
    get: function () {
      return defaultLocation
    },
    set: function (value) {
      if (!value.x || !value.y) throw new Error("Invalid location")
      defaultLocation = value
    },
  })
}

let circle = new Circle(10)
// circle.defaultLocation = {x: 5, y: 5};
// Does not work, because defaultLocation is not a property or method
circle.draw()
```

### Module

```js
// Modules only work in Node, not in browsers
let menu = {}

// Export
module.exports = menu
module.exports = { menu1, menu2, menu3 }
// ES6
export default menu
export { menu1, menu2, menu3 }

// Import
let menu = require("./module")
// ES6
import menu from "./module"
import { menu1, menu2, menu3 } from "./module"
```

### Class

```js
/* 
Class method and getter syntax is the same as
for objects, except you can not include commas between methods. 
*/
class Animal {
  // constructor is invoked every time a new instance of Dog class is created
  constructor(name) {
    this._name = name
    this._behavior = 0
  }

  get name() {
    return this._name
  }

  get behavior() {
    return this._behavior
  }

  incrementBehavior() {
    this._behavior++
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
    const names = ["Angel", "Spike", "Buffy", "Willow", "Tara"]
    const randomNumber = Math.floor(Math.random() * 5)
    return names[randomNumber]
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
    super(name)
    // '_usesLitter' is a new property that is unique to the Cat
    // class, so we set it in the Cat constructor.
    this._usesLitter = usesLitter
  }

  get usesLitter() {
    return this._usesLitter
  }
}

// Class Dog
class Dog extends Animal {
  constructor(name) {
    super(name)
  }
}

// The syntax for calling methods and getters on an instance is the same as calling them on an object — append the instance with a period, then the property or method name. For methods, you must also include opening and closing parentheses.

let nikko = new Dog("Nikko") // Create dog named Nikko

const nikko = new Cat("Nikko", false) // Create cat named Nikko
let bradford = new Dog("Bradford") // Create dog name Bradford

nikko.incrementBehavior() // Add 1 to nikko instance's behavior

console.log(nikko.behavior) // Logs 1 to the console
console.log(bradford.behavior) // Logs 0 to the console

// call static method
console.log(Animal.generateName()) // returns a name
```
