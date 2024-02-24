
## Arguments

- '$#' and '$@' are special variables that are used for command line arguments which are passed to a script.
- '$#'  // Counts the number of arguments passed to the script 
- '$@'  // List of the number of arguments passed to the script 


```
#!/bin/bash
echo "Number of arguments: $#"
echo "All arguments: $@"
# Loop through and print each argument
for arg in "$@"; do
    echo "Argument: $arg"
done
```


## Process

> Find processs and kill
```
#find process
ps -p $(lsof -t -i:8080)

#kill
kill -9 <PID from last command>

```
