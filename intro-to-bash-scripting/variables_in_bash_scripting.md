# Variables in bash scripting

## Numeric Variables in Bash 
- use `expr` in bash for numeric expressions
- you can't just enter numeric expressions in the bash shell
- doesn't natively support floating point numbers
- you can use `bc` to add decimals (floats) together
- if you don't want to open the program you can just pipe things into `bc`

```bash
echo "1 + 2.75" | bc

# bc has a scale argument for decimal places
# output - 3.333
echo "scale=3; 10 / 3" | bc
```
Assigning numeric variables
```bash
# using double quotes for the number will work but it will produce a string value!!!
dog_name='Roger'
dog_age=6
echo "My dog's name is $dog_name and he is $dog_age years old."
```
Double bracket notation
```
expr 5 + 7
echo $((5 + 7))
```
Shell within a shell
```bash
model1=87.65
model2=89.20
echo "The total score is $(echo "$model1 + $model2" | bc)"
echo "The average score is $(echo "($model1 + $model2) / 2" | bc)"
```

## Arrays in Bash
**TODO**