# Type system, Control Structure, Style
- **Type**: restricts the possible values to which a variable can refer,
or an expression can produce, at runtime
- **Compile Time**: When source code is compiled into machine code
- **Run time**: when the program is executing commands (after compilation, if compiled)

## Pros and Cons of Static typing
**Pros**
- increased performance at runtime
    - for scala, being compiled also makes a difference at runtime
- the properties of your program are verified
    - you can prove the abscence of type related bugs
- safe refactoring
- documentation in the form of type annotations

**Cons**
- it takes time to check types
    - delay before execution
- code is verbose
- the language is not flexible

_Note: Scala has type inference that allows code to be less verbose_

### Promoting flexibility
How is the inflexibility in the language addressed? These are advanced topics but here are two ways:
- pattern matching
- innovative ways of writing and composing types

## If/Else
```scala
val hand = 24

if (hand > 21) {
    println("This hand busts")
}
```
In a function
```scala
def maxHand(handA: Int, handB: Int): Int = {
    if (handA > handB) handA
    else handB
}
```

### if-else if-else
```scala
val handA = 26
val handB = 20

// find and print the best hand
if (bust(handA) && bust(handB)) println(0)
if else (bust(handA)) println(handB)
if else (bust(handB)) println(handA)
if else (handA > handB) println(handA)
else println(hand)
```

_Note: you can assign an if-else statement to a variable in one line if it is readable (short)_

## While and the imperative style
Although imperative is not the preferred style for scala, here's how you can do a while loop
```scala
var i = 0
val numRepetitions = 3

while (i < numRepetitions) {
    println("Hip hip hooray!")
    i = i + 1 // you can also do i += 1 but i++ doesn't work
}
```
Example 2
```scala
var i = 0
var hands = Array(17, 24, 21)

while (i < hands.length) {
    println(bust(hands(i)))
    i = i + 1
}
```

## Foreach and the functional style
Scala is a hybrid language that can do imperative style but nudges us towards functional programming
- imperative
    - one command at a time
    - iterate with loops
    - mutating a shared state
- functional
    - functions are first class values
    - operations of a program should map input values to output values rather than change data in place

Example of side effects
```scala
while (i < hands.length) {
    // side effect because we're producing more than a single output
    println(hand(i) > 21) 
    i = i + 1
    // side effect because the function is mutating a variable outside of its scope
}
```

```scala
var hands = Array(17, 24, 21)

// apply the bust function to each element of the array
hands.foreach(bust)
```

### Signs of style
- imperative
    - `var` variables
    - side effects
    - `Unit` types
- functional
    - `val` variables
    - no side effects
    - Non-unit types
        - `Int`
        - `Boolean`
        - `Double`

### Function literals
If a function you use in `.foreach()` has more than one argument, you can use [function literals](https://docs.scala-lang.org/overviews/scala-book/for-loops.html).