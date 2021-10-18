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
- `-eq`: equal to
- `-ne`: not equal to
- `-lt`: less than
- `-le`: less than or equal to
- `-gt`: greater than
- `-ge`: greater than or equal to

Using square brackets:
```bash
x=10
if [ $x -gt 5 ]; then
    echo "$x is greater than 5"
fi
```
File related flags you can use:
- `-e` file exists
- `-s` file exists and has size greater than zero
- `-r` file exists and is readable
- `-w` file exists and is writable

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
For loop syntax
```bash
for x in 1 2 3
do
    echo $x
done
```

Creating a "range" (brace expansion)
Syntax: `{START..STOP..INCREMENT}` defaults to one if not specified
```bash
# prints 1, 3, 5
for x in {1..5..2}
do 
    echo $x
done
```
Three expression syntax
```bash
for ((x=2;x<=4;x+=2))
do
    echo $x
done
```

### Glob expansions
Example:
```bash
for book in books/*
do
    echo $book
done
```

### Shell within a shell
Example
```bash
for book in $(ls books/ | grep -i 'air')
do
    echo $book
done
```

### WHILE loops
Simple example
```bash
x=1
while [ $x -le 3 ];
do
    echo $x
    ((x+=1))
done
```

## CASE statements
Basic Format
```bash
case 'STRING' in
    PATTERN1)
    COMMAND1;;
    PATTERN2)
    COMMAND2;;
    *) # this is common practice though not required
    DEFAULT COMMAND;
esac
```
Example
```bash
case $(cat $1) in
    *sydney*)
    mv $1 sydney/ ;;
    *melbourne*|*brisbane*)
    rm $1 ;;
    *canberra*)
    mv $1 "IMPORTANT_$1" ;;
    *)
    echo "No cities found" ;;
esac
```