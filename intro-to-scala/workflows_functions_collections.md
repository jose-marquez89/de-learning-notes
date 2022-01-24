# Workflows, Functions and Collections

## Scripts, Applications and real-world Workflows
- at the command prompt, scala wraps a script in a template and then compiles and executes the program
- scala is a compiled language but there are interpreted language behaviors when you run a script
    - translates source code to something like machine code to create an executable
- the scala interpreter hides compilation from you
- applications are compiled first and then executed

### IDE's 
- IntelliJ IDEA is a good IDE for scala

### Build tools
- "sbt"`
    - simple build tool
    - compiles runs and tests applications
    - you don't have to use for learning but very helpful when you're developing an actual application

### Jupyter
You can use scala in jupyter notebooks with a kernel called [**almond**](https://almond.sh)

## Functions
- functions are **invoked** with a list of arguments to produce a result
- in scala, functions are first class values
Here's a function
```scala
def bust(hand: Int): Boolean = {
    hand > 21;
}
```
Another examples
```scala
def maxHand(handA: Int, handB: Int): Int = {
  if (handA > handB) handA
  else handB
}
```

## Arrays
- **parameterize an array**: configure its types and parameter values
- **initialize elements of an array**: give the array data

### Collections
- There are mutable and immutable collections
- an array is a type of collection
- arrays are mutable

Here is the parameterization and intitialization done all at once
```
scala> val players = Array("Alex", "Chen", "Marta")
```

Here it is split
```scala
// the players variable is of type Array[String]
// it is an array of strings with length 3 where
// length is the number of elements in the array
val players: Array[String] = new Array[String](3)

// adding values
val players(0) = "Alex"
val players(1) = "Chen"
val players(2) = "Marta"
```
_Note: although we defined the array as a `val` (recommended for arrays) and not a `var` (which can be reassigned), it is still possible to change the values if the array because it is mutable. Creating the array using `val` reduces the amount of things you need to keep track of that can change_

#### The 'Any' Supertype
```scala
val mixedTypes = new Array[Any](3)

mixedTypes(0) = "I like turtles"
mixedTypes(1) = 5000
mixedTypes(2) = true
```

Oh by the way, forgot to tell you: _arrays as defined here aren't commonly used in scala because they encourage side effects via their mutability_

## Lists
- immutable
- has methods because it's an object
- there are many list methods, these are only a few
    - `myList.drop()`
    - `myList.mkString(", ")`
    - `myList.length()`
    - `myList.reverse()`

### Lists can still be useful
While immutable, you can still use lists with changing values

The `::` operator is pronounced "cons", popular concept in functional programming languages that you'll use often. There is an append operation that exists in scala but it isn't used because it's not as efficient
```scala
val players = List("Alex", "Chen", "Marta")

// the :: operator prepends an element to the start of an existing list
val newPlayers = "Sindu" :: players

// you can also define players as a var
var players = List("Alex", "Chen", "Marta")

// then prepend in the same var
var players = "Sindu" :: players
```

### Nil
`Nil` is an empty list. A common way to initialize new lists is with `Nil` and `::`
```scala
val players = "Alex" :: "Chen" :: "Marta" :: Nil
```

### Concatenating lists
```scala
// kind of like python extend method on an list, kind of
val playersA = List("Sindu", "Alex")
val playersB = List("Chen", "Marta")
val allPlayers = playersA ::: playersB
```

