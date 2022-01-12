# A scalable language
Scala means staircase in italian, inspired by a spiral staircase where the language started.
- general purpose
- strong static type system
- designed to address criticisms of Java
- scala source is intended to be compiled in Java bytecode
- runs on a JVM

Why scala?
- scalable
- used at companies like netflix, morgan stanley, airbnb, deutsche bank
- designed to be extended by the people programming in it
    - google the cathedral and bazaar
    - lets you add new types, collections and control constructs
- huge library ecosystem
- OOP and functional
- static typing helps avoid bugs
- consice
- high level
- advanced static types

Who uses scala?
- software, data, and machine learning engineers
- data scientists
- industries
    - finance
    - healthcare
    - tech
    - more 

OOP
- every value is an object
- every operation is a method call

```scala
val sumA = 2 + 4

// behind scenes
val sumA = 2.+(4)
```

Scala is functional
- functions are first class values
    - they can be stored in a variable, passed to other functions, return from functions etc...
- operations of a program should map input values to output values rather than change data in place
    - functions should not have side-effects

### Using scala
- You can start the scala interpreter like you do python
- `1 + 3` will produce `res0: Int = 4`
- `res0` is reusable
- `println()` prints to stdout

## Immutable variables and value types