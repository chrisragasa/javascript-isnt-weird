# Execution Contexts and Lexical Environments

syntax parser:
a program that reads your code and determines what it does and if its grammar is valid.

lexical environment:
where something sits physically in the code you write.

execution context:
a wrapper to help manage the code that is running. there are lots of lexical environments. which one is currently running is manageed via execution context. it can contain things beyond what you've written in your code.

name/value pair:
a name which maps to a unique value. the name may be defined more than once, but only can have one value in any given execution context. that value may be more name/value pairs.

object:
a collection of name value pairs -- the simplest definition when talking about javascript.
an "object" in javascript is no more complex than a collection of name/value pairs -- not the same
as how objects are represented in other languages (very simple!)

### b3-global-environment

the global environment and the global object

- global execution context: creates a global object and 'this' , automatically created by the js engine
- if you run a blank js file and inspect the browser console, notice how 'this' and 'window' variables are
  automatically created
- at the global level: global object (window) = 'this'
- GLOBAL = "not inside a function"
- when you create variables (assuming that variable is not written inside a function) and functions, these variables and functions are attached to the global object

### b4-hoisting

people explain hoisting as a phenomenon where functions and variables are "hoisted" to the top of the file.

however, what's actually happening is the "creation phase" of the execution context... during this phase, the following things are created:

- global object
- 'this'
- outer environment
- memory space for variables and functions, aka "hoisting"

Functions are placed into this memory space in it's whole entirety.
However, with variables, hoisting is a little different.
The engine sets all values for variables as 'undefined'.
It's not a good idea to rely on hoisting.

### b5-undefined

`undefined` is a special value in javascript

if `a` is never declared in the file

- Uncaught reference error = `a` was never stored in memory

if `a` is declared, but not set

- `a` will be set to undefined

never set a variable equal to `undefined`!!! for example, `a = undefined`.
this will be hard to debug!

### b6-execution

the phase that happens after the "creation" phase where code is executed line by line.

### single threaded, synchronous execution

- single threaded: one command executed at a time
- synchronous: one at a time and _in order_

"javascript is actually single threaded and synchronous in it's behavior!"

### function invocation and execution stack

- _invocation_: running a function... in javascript, by using parenthesis()

- everytime a function is invoked, it creates a new "execution context" and puts in on top of the "execution stack"

- each execution context goes through the "creation" and "execution" phase
- this concept is very important to have in your mental model with thinking about function invocation

### b9-variable-environments

- variable enironment: where the variables live and how they relate to each other in memory
- notice how in the example, different values are printed for `myVar`

### b10-scope-chain

- every execution context has a reference to its outer environment. however, the outer environment isn't necessarily 1 level down on the execution stack.

- when you ask for a variable when running a line of code inside any particular execution context and the javascript engine can't find that variable, it automatically searches for that variable at the execution context of the outer reference -- where the outer reference points depends on where the function sits lexically

- "scope chain" is the process at which the engine goes through the execution stack to look for a called variable

### scope, es6, and `let`

- _scope_: where a variable is available in your code (and if it's truly the same variable, or a new copy)

`let`: allows the javascript engine to use block scoping

```js
if (a > b) {
  let c = true;
}
```

- with `let` you can't use a variable unless it is actually declared

### asynchronous callbacks

- _asynchronous_: more than one at a time
- there are asynchronous events in javascript (click events, http requests, etc.)
- the javascript engine doesn't exist by itself: it has hooks where it can talk to other engines that live inside the browser (rendering engine, HTTP Request, etc.)
- the Javascript engine contains: the "Global Execution Context" and the "Event Queue" (click event or an HTTP Request)
- the Javascript engine looks at the "Event Queue" only when the execution stack is empty -- for example, there might be a `clickHandler()` for a click event, but this won't get called until everything on the execution stack is completed: `Global Execution Context`, `a()`, `b()`, etc.
- so in essence, JavaScript still runs "synchronously", but the browser "asychronously" puts events into the event queue for JavaScript to handle when its execution stack is empty.
- another way to think of this phenomenon, is that javascript code runs normally, but whenever an event happens while the javascript code is running, the js engine puts that event in the "Event Queue". when the execution stack is empty, the event queue is then handled. this is why if you run the code in `b11-asynchronous-callbacks`, logging of the click event happens at the very end if you click the browser during the 3 sec runtime of the `waitThreeSeconds()` function

# Types and Operators

_dynamic typing_: you don't tell the engine what type of data a variable holds, it figures it out while your code is running

Static Typing:

```
bool isNew = 'hello'; // an error
```

Dynamic Typing:

```
var isNew = true; // no errors
isNew = 'yup!'; // no errors
isNew = 1; // no errors
```

### Primitive Types

**primitive type**: a type of data that represents a single value; that is, not an object

- `undefined`: represents lack of existence (you shouldn't set a value to this)
- `null`: represents lack of existence (you can set a variable to this)
- `boolean`: `true` or `false`
- `number`: _floating point_ number (there's always some decimals). Unlike other programming languages, there's only one `number` type... and it can make the math weird.
- `string`: a sequence of characters (both '' and "" can be used)
- `symbol`: used in ES6

### Operators

**operator**: a special function that is syntactically (written) differently; generally, operators take two parameters and return one result

"operators are functions"

### Operator Precendence and Associativity

[Operator precendence - MDN web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

**operator precendence**: which operator function gets called first... functions are called in order of precendence

**operator associativity**: what order operator functions get called in: left-to-right or right-to-left when functions have the same precedence

### Coercion

**coercion**: converting a value from one type to another. This happens quite a bit in Javascript because it is dynamically typed.

in the code below, javascript engine coerces the number 1 into a string and concatenates it to the 2

```js
var a = 1 + "2";
console.log(a);
// 12
```

_This is an extremely important concept for debugging purposes._

### Comparison Operators

[Equality comparisons and sameness
](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)

```js
console.log(3 < 2 < 1);
// true
```

coersion

```js
Number(undefined);
// NaN

Number(null);
// 0
```

Equality

- sometimes hard to debug because of dynamic typing and coercion

```js
3 == 3; // true
"3" == 3; // true
false == 0; // true

// although null coerces to 0, null does not == 0
null == 0; // false

0 != false; // false
```

Strict Equality

- easier to debug because it makes sure that the values in comparison are both the same data type

```js
3 === 3; // true
"3" === "3"; // true
"3" === 3; // false

0 !== false; // true
```

**In general, 99% of the time use `===` when making equality comparisons.**

### Existence and Booleans

coersion can be used to check the existence of things

### Default Values

- function parameters are set to `undefined` if they are not declared in the function

```js
name = name || "<Your name here>";
// name = '<Your name here>'
// name is set to the value that can be coerced to true
```

### Framework Aside: Default Values

Javascript stacks code from all js files and pastes them together.

```html
<script src="lib1.js"></script>
<script src="lib2.js"></script>
<script src="app.js"></script>
```

```js
// lib1.js
var libraryName = "Lib 1";

// lib2.js
var libraryName = "Lib 2";

// app.js
console.log(libraryName);
// "Lib 2"
```

In the code above, `libraryName` becomes a global variable...
In order to handle scenarios where code collides with one another, you can use a method like this:

```js
window.libraryName = window.libraryName || "Lib 2";
```

# Objects and Functions

### Objects and the Dot

- while in other langauges, objects and functions are 2 distinct things to talk about, in JS, _they are very much related_
- Object: Primitive "property", Object "property", Function "method"

```js
var person = new Object();
person["firstname"] = "Tony"; // create name value pair in the person object
person["lastname"] = "Alicea";

var firstNameProperty = "firstname";

console.log(person[firstNameProperty]); // Tony
console.log(person.firstname); // Tony
console.log(person.lastname); // Tony
console.log(person.firstNameProperty); // Tony

// preferred method is using the dot operator
person.address = new Object();
person.address.street = "111 Main St.";
person.address.city = "New York";
```

### Objects and Object Literals

- this is the preferred method to making objects

```js
var Tony = {
  firstname: "Tony",
  lastname: "Alicea",
  address: { street: "111 Main St.", city: "New York", state: "NY" },
};

function greet(person) {
  console.log("Hi" + person.firstname);
}

greet(Tony);

// wherever you want, you can create objects on the fly
greet({ firstname: "Mary", lastname: "Doe" }); // in JS you can make an object as an argument and pass it to a parameter
```

### Faking Namespaces

**namespace**: a container for variables and functions -- typically to keep variables and functions with the same name separate

Instead of:

```
var greet = 'Hello!';
var greet = 'Hola!';
```

In js, you deal with collisions by using objects:

```
var english = {};
var spanish = {};

english.greet = 'Hello!';
spanish.greet = 'Hola!';
```

### JSON and Object Literals

object literal (attributes could also be strings, but they don't have to)

```
var objectLiteral = {
  firstname: 'Mary',
  isAProgrammer: true
}
```

JSON -- attributes _must be strings_. can be thought of as a subset of object literal.

```
  "firstname": "Mary",
  "isAProgrammer": true
```

JavaScript has `JSON.stringify` and `JSON.parse` to convert between the two (string vs object)

### Functions are Objects

**first class functions**: everything you can do with other types you can do with functions; assign them to variables, pass them around, create them on the fly

function = a special type of object

- since it is an object, we can attach a primitive, object, and a function to a function
- NAME = optional... can be anonymous
- CODE = the code we write gets placed into a special property called the code property of the object. this is "invocable" ()... "run this code, of this function"

we need to think of a function AS AN OBJECT with additional properties!

### Function Statements and Function Expressions

**expression**: a unit of code that results in a value

```js
a = 3;
// 3

1 + 2;
// 3
```

`if` is a statement... it doesn't return anything

`expression` results in a value... it returns a value

In JS, there are function statements and function expressions.

```js
greet();

// FUNCTION STATEMENT
// When this is ran during the execution phase, JS recognizes it, but doesn't actually do anything.
funtion greet() {
  console.log('hi');
}

// FUNCTION EXPRESSION
// When this is ran during the execution phase, an object is CREATED, which results in a value -- the value being the function object.
greetAnonymous = function() {
  console.log('hi');
}

anonymousGreet();
```

Remember, with the concept of hoisting, that if we shift the order of anonymousGreet(), we will get undefined (variables are set to undefined during the execution phase).

```js
greet();


funtion greet() {
  console.log('hi');
}

anonymousGreet();
// we will get an error here: undefined is not a function
// remember, greetAnonymous is set to `undefined` during the execution phase
// here, we are trying to invoke greetAnonymous, but it is still the `undefined` primitive type, but it isn't a function YET!
// function expressions are not hoisted!!!

greetAnonymous = function() {
  console.log('hi');
}
```

You can pass a function expression as a parameter to a function.

```js
function log(a) {
  console.log(a);
}

log({
  greeting: "hi",
});

// remember, a function is an object. we can create functions on the fly. they work like object literals!
log(function () {
  console.log("hello");
});
```

We can take the above block even further. What happens if we want to invoke the function, instead of just printing out the function?

```js
function log(a) {
  a();
}

log(function () {
  console.log("hello");
});
```

These concepts introduce a new class of programming: functional programming!

### By Value vs By Reference

In JS, Primitives are copied by value and Objects (including functions) are copied by reference.

```js
// by value (primitives)
var a = 3;
var b;

b = a;
a = 2;

console.log(a); // 2
console.log(b); // 3

// by reference (all objects (including functions))
var c = { greeting: 'hi' };
var d;

d = c;
c.greeting = 'hello'; // mutate

console.log(c); // Object { greeting: 'hello' }
console.log(d); // Object { greeting: 'hello' }

// by reference (even as parameters)
function changeGreeting(obj) {
  obj.greeting = 'Hola'; // mutate
}

changeGreeting(d);
console.log(c); // Object { greeting: 'Hola' }
console.log(d); // Object { greeting: 'Hola' }

// equals operator sets up new memory space (new address)
c = { greeting: 'howdy' };
console.log(c); // Object { greeting: 'howdy' }
console.log(d); // Object { greeting: 'Hola' }
```

### Objects, Functions, and 'this'

```js
console.log(this); // Window

```

If simply invoking a function, `this` is going to refer to the global object.
```js
function a() {
  console.log(this);
}

var b = function() {
  console.log(this);
}

a(); // Window object
b(); // Window object
```

You can mutate the object that contains you by using the `this` keyword.
```js
var c = {
  name: 'The c object',
  log: function() {
    this.name = 'Updated c object';
    console.log(this);
  }
}

c.log() // Object c

// since log was defined inside the object c, `this` refers to the c object, instead of the global Window object
```

Whenever a function is a method of an object, the `this` keyword refers to the object. 
However, other internal functions refer to `this` as the global object. You can use the concept of setting a variable = `self`, to avoid this problem. See below:
```js
var c = {
  name: 'The c object',
  log: function() {
    var self = this; // set by reference, pointing to the whole object `c` - this pattern is used often

    self.name = 'Updated c object';
    console.log(self);

    var setname = function(newname) {
      self.name = newname;
    }

    setname('Updated again! The c object');
    console.log(self);
  }
}

c.log();
```

### Arrays - Collections of Anything

```js
var arr = [1,2,3];

// since js is dynamically typed, you can mix and match data types in an array
var arr = [
  1,
  false,
  {
      name: 'Tony',
      address: '111 Main St.'
  },
  function(name) {
    var greeting = 'Hello ';
    console.log(greeting + name);
  },
  "hello"
]

console.log(arr); // [1, false, Object, function, "hello"]

arr[3](arr[2].name); // 'Hello Tony'
```

### 'arguments' and spread

Holds all the values that you pass to a function.

```js
function greet(firstname, lastname, lanugage) {
  console.log(firstname);
  console.log(lastname);
  console.log(language);
}

greet(); // is js, you can run this function even though you aren't passing any arguments
// hoisting sets each of these values equal to Undefined

greet('John'); // prints John Undefined Undefined
// js assumes since only 1 argument passed, it is the first parameter (firstname)
```

You can set default values for parameters like this code example below:

```js
function greet(language) {

  language = language || 'es';

  console.log(language)
}

greet(); // es
greet('eg'); // eg
```

There is a keyword `arguments` that is a list of all the values of the parameters passed.
```js
function greet(firstname, lastname, lanugage) {

  if (arguments.length === 0) {
    console.log('Missing parameters');
    return;
  }

  console.log(firstname);
  console.log(lastname);
  console.log(language);
  console.log(arguments); // prints a list of the arguments passed

}


greet();
```

You can use the concept of a spread `...` parameter, where any additional arguments will get wrapped up into a list.
```js
function greet(firstname, lastname, lanugage, ...other) {

  if (arguments.length === 0) {
    console.log('Missing parameters');
    return;
  }

  console.log(firstname);
  console.log(lastname);
  console.log(language);
  console.log(arguments); // prints a list of the arguments passed

}


greet('john','doe','en', '1', '2', '3');
```

### Function Overloading

```js
function greet(firstname, lastname, language) {

  if (language === 'en') {
    console.log('Hello ' + firstname + ' ' + lastname);
  }

  if (language === 'es') {
    console.log('Hello ' + firstname + ' ' + lastname);
  }

}

function greetEnglish(firstname, lastname) {
  greet(firstname, lastname, 'en');
}

function greetSpanish(firstname, lastname) {
  greet(firstname, lastname, 'es');
}

greetEnglish('John', 'Doe');
greetSpanish('John', 'Doe');
```

### DANGEROUS: Automatic Semicolon Insertion
Semicolons are optional in Javascript. Therefore, in places where the syntax parser expects a semicolon, the js engine will automatically insert a semicolon.

Remember to always put your own semicolons! Forgetting to do so can introduce huge problems.

```js
function getPerson() {
  return 
  {
    firstname: 'Tony'
  }
}

console.log(getPerson()); // undefined
// the js engine automatically inserted a semicolon after the `return`


// instead, do the following
function getPerson() {
  return {
    firstname: 'Tony'
  }
}

```
