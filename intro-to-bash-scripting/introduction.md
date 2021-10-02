# Bash Scripting Intro
- stands for Bourne Again Shell

### Shell commands refresher
- `grep` filters input based on regex
- `cat` concatenates file line by line
- `tail` and `head` give only the last `-n` lines
- `wc` does a word or line count (with flags `-w` and `-l`)

### Regex
- You can review and test regular expression at `regex101.com`

You can use regex with grep
```bash
grep '[pc]' fruits.txt 
```

### Piping
You can do something like `sort | uniq -c`. You sort first because `uniq` uses adjacent lines for comparison.

```bash
cat new_fruits.txt | sort | uniq -c | head -n 3
```
Output ->
```
14 apple
13 banana
12 carrot
```