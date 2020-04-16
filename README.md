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
