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

### Array types in bash

Numerically Indexed
```bash
# declare without adding elements
declare -a my_first_array

# create and add elements at the same time
# note the absence of commas
my_first_array=(1 2 3)
```

### Array properties

Get all array elements
```bash
# output is 1 3 5 2
my_array=(1 3 5 2)
echo ${my_array[@]}

# get the length of the array from the array itself
echo ${#my_array[@]}
```

Get individual elements
```bash
# gets 300
my_first_array=(15 20 300 42)
echo ${my_first_array[2]}
```
_Note: bash uses zero indexing unlike R and **like** python_

Change array elements
```bash
my_first_array=(15 20 300)
my_first_array[0]=999
echo ${my_first_array[0]}
```
Slicing syntax: `array[@]:N:M` where `N` is the starting index and `M` is the number of elements to return
```bash
# output is 42 32
my_first_array=(15 20 300 42 23)
echo ${my_first_array[@]:3:2}
```
Appending to arrays: `array+=(elements)`
```bash
# output is 300 42 23 10
my_array=(300 42 23)
my_array+=(10)
echo ${my_array[@]}
```

### Associative arrays
This is the second type of array in bash (only available in Bash 4 and on). Basically this is dictionary

Creating an associative array
```bash
# declare with -A
declare -A city_details
# add elements
city_details=([city_name]="New York" [population]=140000000)
# index into the array with key
echo ${city_details[city_name]}
```

Creating associative arrays all in one line
```bash
declare -A city_details=([city_name]="New York" [population]=140000000)

# access all keys with '!'
echo ${!city_details[@]}
```