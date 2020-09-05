# JavaScript

JavaScript provides functionality to a website.

## Table of Content

- [Overview](##Overview)
  - [Browser](###Browser)
- [Variables](##Variables)
  - [Numbers](###Numbers)
  - [Strings](###Strings)
  - [Boolean](###Boolean)
  - [Null & Undefined](###Null-&-Undefined)
  - [Objects](###Objects)
  - [Arrays\*](###Arrays)
- [Logic](##Logic)
  - [Operators](###Operators)
  - [Comparison](###Comparison)
  - [Loops](###Loops)
- [Function](##Function)
- [Advanced Concepts](##Advanced-Concepts)
- [References](##References)

## Overview

- JavaScript Engines (V8, SpiderMonkey, Chakra, etc.) execute Javascript by:
  1. Parses the script.
  2. Compiles the script into machine code.
- `"user strict";`
  - Turns on new ES5 features and impose harsher restrictions.
  - Helps with spotting syntax/logic [errors](https://love2dev.com/blog/javascript-strict-mode/).
- Synchronous vs Asynchronous JavaScript:

  ![Definition of synchronous and asynchronous](/images/syn-asyn.PNG)

  [Read more](https://medium.com/better-programming/is-javascript-synchronous-or-asynchronous-what-the-hell-is-a-promise-7aa9dd8f3bfb)

### Browser

- `alert("message")` - Display message.
- `confirm("question msg")` - Display question message, and OK/Cancel buttons.
- `prompt("message", opt[default input value])` - Display message, input field, and OK/Cancel buttons.  
  (IE: use `""` for optional parameter to avoid error)

## Variables

- Naming:
  - Cannot start with a number.
  - Can only include numbers, letters, `_` and `$`.
  - List of [reserved names](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords) that cannot be used.
  - Case matters.
  - `Var`: @ https://javascript.info/var
- Data Types
  - Dynamically-Typed - variables are not bound to any specific data types.
  - 6 Primitives: number, string, boolean, `null`, `undefined`, Symbol
  - 1 Special Type: Object
  - `let name = Symbol("optional description")`
  - See Symbols in [Advanced Concepts](##Advanced-Concepts)
  - Primitives are copied directly. Objects are copied _by reference_ (address in memory).
- Type Conversion:

  - `String(value)`

    - `String(value)` can be used on `null` and `undefined`. But `value.toString()` cannot, as it requires value to [exist](https://stackoverflow.com/questions/3945202/whats-the-difference-between-stringvalue-vs-value-tostring).
    - Numeric conversion rules:

      ![Number Conversion](/images/numeric-conversion.PNG)

  - `Number(value)`
    - Explicit conversion to number is usually required when reading values from string-based sources (like text form).
    - `NaN` is returned if value cannot be converted to number..
  - `Boolean(value)`

    - Boolean conversion (truthy/falsy) rules:

      ![Boolean conversion rules](/images/boolean-conversion.PNG)

    - `"0"` is `true`

  - Object to Primitive Conversion:
    - Object to `boolean` conversion always return `true`.
    - Object to `number` conversion happens when math operation is applied onto objects, like `date2 - date1`.
    - Object to `string` conversion happens when object needs to be outputted, like in `alert(obj)`.
    - [Fine-tuning](https://javascript.info/object-toprimitive#toprimitive)

### Numbers

- Special numeric values:
  - `Infinity` and `-Infinity` can be used directly, or as a result of dividing number by 0.
  - `NaN`
    - Result of undefined mathematical operation between different data types.
    - Sticky - propagates to the whole result.
- more @ https://javascript.info/number

### Strings

- more @ https://javascript.info/string

### Boolean

- Truthy vs Falsy

### Null & Undefined

- `undefined`
  - Meaning: value has been declared but not assigned.
  - Returned when looking up non-existent properties of objects.
- `null`
  - Meaning: nothing is or should be here.
  - `typeof null` returns "Object" (officially recognized error).

### Objects

- Constructor: `let name = new Object();`
- Literal: `let name = {};`
- `obj.key` vs `obj[key]`
  - `.` does not allow space in keys. (i.e. `"key one"`)
  - Computed Properties: `[]` can be used to add \_variables into objects.
  - `[]` can also have complex expressions inside.
  - There are no reserved words for object keys (except `__proto__`)
- `delete obj.key` deletes obj.key.
- Check property existence:
  - `(obj.key === undefined)`
  - `("key" in obj)` - must use quotes around key.
- `for (let key in obj) {...}` loops through "obj" with "key" as each entry.
- Ordering of keys is based on creation order, except those with _Integers Properties_ (a string that can be converted to-and-from and integer without change) are sorted (ascending).
- Objects are copied _by reference_ (address in memory). Modifying object in one place changes it for all places.
- `const name = {...}`'s content can be changed, but not its reference `name`.
- Creating [independent copy](https://javascript.info/object#cloning-and-merging-object-assign) of an object (not just reference) is usually unnecessary, but can be done with `Object.assign()` or "Deep Cloning".

#### Object Methods

- `obj.key = function() {...};`
- Object literal shorthand: `obj = { key(): {...} };`
- Called with `obj.key();`
- Methods are object properties with _functions_ as values.

#### `this` Keyword

- To access properties stored inside the same object, use `this.key`.
- The value of `this` is evaluated at run-time, where it look for the object "before the dot".
- Arrow functions do not have `this`. When `this` is used inside arrow functions, it references the outer non-arrow function/object.
- [see more](https://javascript.info/arrow-functions)

#### Constructor Functions

- Constructors exist to implement reusable object creation code.
- Name begin with capital letter.
- Executed _only_ with `new` keyword.
  ![Constructor Function example](/images/constructor.png)
  ![Constructor Function example 2](/images/constructor-2.png)

### Arrays

methods:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find

#### Methods

https://www.digitalocean.com/community/tutorials/how-to-use-object-methods-in-javascript
https://www.w3schools.com/js/js_object_methods.asp
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Object/values

## Logic

### Operators

- Operators _always_ return values.
- [Precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)
  - Unary operators have higher precedence than binary operators, so Unary operators are applied first.
  - `()` has highest precedence.
- `=` (Unary)
  - When chained, execution runs from right to left.
  - Very low precedence, applied last.
- `+`
  - (Unary) Converts other types into numbers (based on value and truthy/falsy)
  - (Binary) From left to right, add numbers until it runes into the first string operand, then concatenate.
  - To add `"1"` and `"2"`, do `+"1" + +"2"`.
- `++/--` (Unary)
  - Prefix Form `++variable` - add 1 to variable and return new value.
  - Postfix Form `variable++` - return variable and add 1 to it.
- `?` (Ternary)
  - Should be used to _return and assign_ values based on condition.
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
- `%` (Binary) = Remainder
- `**` (Binary) = Exponent

### Comparison

- `!=` means not equal.
- `!==` means not strictly equal.
- String
  - `'a' > 'A'` is `true`
  - `'az' > 'aa'` is `true`
  - `'aa' > 'a'` is `true`
- Different Types
  - all values are converted to _numbers_ or truthy/falsy _numbers_ when comparing.
  - `null == undefined` is `true`
  - But `null === undefined` is `false`
  - See more [null/undefined edge cases](https://javascript.info/comparison#comparison-with-null-and-undefined).

### Loops

- `continue` directive moves loop onto next iteration.
- `break` directive ends loop
- `label` makes execution jumps to a specific outer loop.
- `for (initial; condition; step) { body }`
  - Execution order:
    1. initial
    2. condition
    3. **_body_**
    4. step (step is always executed at the end)
  - If `initial` is declared within loop, it is bound within the scope of the loop. But if declared outside, it can be modified by actions performs inside the loop.
- `switch (condition) { case (match): break;}`
  - `match` case matters.
  - Without `break` at the end of each `case`, execution will move on to next `case`.

## Functions

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
- [Closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures):
  ![Closure](/images/closure.png)
  - A "closure" is like an "area" around an "inner-function" where time (it's outer lexical scope) is frozen. This way, this inner function will still have access to all the outer function's variables (even after the outer function has finished running), because the variables have been frozen inside the "closure" of the inner function.

### Modules

https://v8.dev/features/modules

## Advanced Concepts

- [Symbols](https://javascript.info/symbol#summary)
- [Bitwise Operators](https://javascript.info/operators#bitwise-operators)
- [Garbage Collection](https://javascript.info/garbage-collection)
- [Linters](https://javascript.info/coding-style#automated-linters)
- [Regular Expressions](https://javascript.info/regular-expressions)
- [Regex Cheatsheet](https://www.rexegg.com/regex-quickstart.html)
