# Control statements in bash scripting

## IF statements
Example flow
```bash
if [ CONDITION ]; then
    # SOME CODE
else
    # SOME CODE
fi

# example with real code
x="Queen"
if [ $x == "King" ]; then
    echo "$x is a King"
else
    echo "$x is not a King"
fi
```

### Arithmetic statements
Example
```bash
# using double parentheses
if (($x > 5)); then
    echo "$x is more than 5"
fi
```
You can also use the following equivalents
-`-eq`: equal to
-`-ne`: not equal to
-`-lt`: less than
-`-le`: less than or equal to
-`-gt`: greater than
-`-ge`: greater than or equal to

Using square brackets:
```bash
x=10
if [ $x -gt 5 ]; then
    echo "$x is greater than 5"
fi
```
File related flags you can use:
-`-e` file exists
-`-s` file exists and has size greater than zero
-`-r` file exists and is readable
-`-w` file exists and is writable

You can find more flags [here](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)

### using AND and OR
-`&&` AND
-`||` OR

Examples:
```bash
# both examples will have the same output
x=10
if [ $x -gt 5 ] && [ $x -lt 11 ]; then
    echo "$x is greater than 5 and less than 11"
fi

x=10
if [[ $x -gt 5  &&  $x -lt 11 ]]; then
    echo "$x is greater than 5 and less than 11"
fi
```

### IF and command line programs
Examples
```bash
# both return the same result
if grep -q Hello words.txt; then
    echo "Hello is inside"
fi

if $(grep -q Hello words.txt); then
    echo "Hello is inside"
fi
```

## FOR loops and WHILE statements
TODO