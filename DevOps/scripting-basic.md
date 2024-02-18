
## Arguments

$# and $@ are special variables that are used for command line arguments which are passed to a script.
$#  // Counts the number of arguments passed to the script 
$@  // List of the number of arguments passed to the script 

```
#!/bin/bash
echo "Number of arguments: $#"
echo "All arguments: $@"
# Loop through and print each argument
for arg in "$@"; do
    echo "Argument: $arg"
done
```

##