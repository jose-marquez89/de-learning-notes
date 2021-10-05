# Standard Streams and Arguments

### Streams
There are 3 streams:
- STDIN: stream of data into the program
- STDOUT: streaming data _out_ of the program
- STDERR: error stream

If you see the following, it's a redirect for one of the streams to be deleted. 2 would be STDERR and 1 would be STDOUT (these seem to be zero indexed)
```
2> /dev/null
1> /dev/null
```
Example of this sort of thing:
```bash
# redirect STDOUT to new file
cat sports.txt 1> new_sports.txt
```

When you use a pipe, you are redirecting the STDOUT of a command and using it as STDIN for the next command in the pipeline.

### STDIN vs ARGV
- ARGV is the array of all the arguments given to the program
- access arguments with `$`. `$1` is the first argument
- `$@` and `$*` give all the arguments in ARGV
- `$#` gives the number (length) of arguments