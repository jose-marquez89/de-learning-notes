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
TODO
