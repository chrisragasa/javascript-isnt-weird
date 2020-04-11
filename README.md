### EXECUTION CONTEXTS AND LEXICAL ENVIRONEMNTS

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
