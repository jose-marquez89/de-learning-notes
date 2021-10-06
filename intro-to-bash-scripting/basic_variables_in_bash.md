# Basic variables in bash

### Assigning and referencing variables
```bash
var1="Moon"
echo $var1

# another example
firstname='Cynthia'
lastname='Liu'
echo "Hi there" $firstname $lastname
```
_Note: bash won't let you put spaces before and after `=` sign for variables

Double and single quotes do _not_ mean the same thing:
- 'sometext': shell interprets this literally
- "sometext": shell interprets literally _except_ using `$` and backticks
- `sometext`: shell runs the command and captures STDOUT back into a variable

Example
```bash
# below outputs $now_var because it interprets things literally
now_var='NOW'
now_var_singlequote='$now_var'
echo $now_var_singlequote

# below outputs NOW
now_var_doublequote="$now_var"
echo $now_var_doublequote
```

#### Using backticks
The `date` command prints the date to the terminal
```bash

# below outputs "The date is Mon 2 Dec 2019 13:13:32 AEDT"
rightnow_doublequote="The date is `date`."
echo $rightnow_doublequote

# equivalent to backtick notation - results are the same
rightnow_doublequote="The date is `date`."
rightnow_parentheses="The date is $(date)."
echo $rightnow_doublequote
echo $rightnow_parentheses
```
Parentheses is used more in modern applications


