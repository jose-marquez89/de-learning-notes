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
