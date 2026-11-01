
## Arguments

```
# $# and $@ are special variables that are used for command line arguments which are passed to a script.
# '$#'  // Counts the number of arguments passed to the script 
# '$@'  // List of the number of arguments passed to the script 

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
ps -ef | grep -i <process_name>
# find process using port
ps -p $(lsof -t -i:8080)

#kill
kill -9 <PID from last command>
```


## Basic Scripting
```
# Unix flavour name
uname -a

# word count
wc -l <file_name>  # count lines
wc -w <file_name>  # count words

# check if file exists
if [ -f "filename" ]; then
    echo "File exists."
else
    echo "File does not exist."
fi


# check if directory exists
if [ -d "directory_name" ]; then
    echo "Directory exists."
fi

# find file 
find /path/to/search -name "filename"

# diff
diff file1 file2  # compare two files
```


# chmod
- read (r) = 4 ,  write (w) = 2, execute (x) = 1
- [owner][group][others]  =  [rwx][rwx][rwx] = [4+2+1][4+2+1][4+2+1] = 7 7 7

| Permission | Value |
|------------|-------|
| --- | 0     |
| rwx | 7     |
| rw- | 6     |
| r-x | 5     |
| r-- | 4     |





