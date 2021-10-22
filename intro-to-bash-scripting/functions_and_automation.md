# Functions and Automation

## Basic Functions in Bash
Basic syntax, keep in mind that `return` is different than in other languages
```bash
function_name () {
    # function code
    return # something
}

# alternative way to write functions
function function_name {
    # function code
    return # something
}
```

Example
```bash
temp_f=30
function convert_temp () {
    temp_c=$(echo "scale=2; ($temp - 32) * 5 / 9" | bc)
    echo $temp_c
}
```

## Arguments, Return values and scope
Passing arguments into functions is similar to how you do 
this for a script, so you have access to ARGV properties:
- access each argument with `$1`, `$2` etc
- `$@` and `$*` gets all arguments in ARGV
- `$#` gives the length of arguments


### Scope
Variables are global by default, this is not like R and Python
```bash
# default global scope (prints LOTR.txt)
function print_filename {
    first_filename=$1
}
print_filename "LOTR.txt" "model.txt"
echo $first_filename
```

### Restricting scope in functions
The reason the code below prints a blank line is because when `first_filename` was assigned the **global** `$1` which is blank when you run with `bash script.sh` 
```bash
# prints a blank
function print_filename {
    local first_filename=$1
}
print_filename "LOTR.txt" "model.txt"
echo $first_filename
```

### Return values
The `return` option in bash doesn't return data, it's only meant to return success (0) or failure (1-255).
It's captured by the global variable `$?`

How do you get data out? Two approaches:
- assign to a global variable
- echo back what you want and capture using shell-within-a-shell

### Return errors
```bash
function function_2 {
    echlo # typo/error
}
function_2 # call the function
echo $? # print the return value
```

### Returning correctly
```bash
function convert_temp {
    echo $(echo "scale=2; ($1 - 32) * 5 / 9" | bc)
}
converted=$(convert_temp 30)
echo "30F in Celsius is $converted C"
```