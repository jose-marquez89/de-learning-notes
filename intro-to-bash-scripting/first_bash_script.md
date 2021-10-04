# Your first bash script

### Anatomy
- usually starts with shebang or hash-bang line `#!/usr/bash`

Example: 
```bash
#!/usr/bash
echo "Hello World"
echo "Goodbye World"
```

Pipe 2 sed commands on one file:
```bash
#!/bin/bash

# Create a sed pipe to a new file
cat soccer_scores.csv | sed 's/Cherno/Cherno City/g' | sed 's/Arda/Arda United/g' > soccer_scores_edited.csv
```