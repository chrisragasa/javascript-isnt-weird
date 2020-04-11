### EXECUTION CONTEXTS AND LEXICAL ENVIRONMENTS

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
