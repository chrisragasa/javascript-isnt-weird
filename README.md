---
A guide to understanding how JavaScript works under the hood.
---

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

- in the example, `function b` and `function a` are both lexically sitting on top of the global environment
- "scope chain" is the process at which the engine goes through the execution stack to look for a called variable
