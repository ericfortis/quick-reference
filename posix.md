# POSIX

### Search recursively, excluding binaries
```shell script
grep -Ir "searchme" .
```

### Prevent overwriting files
```shell script
set -o noclobber
```

### Abort script if a command fails
This has a few gotchas, e.g. , `foo=$(grep -c 'search' /file)` might cause
it to exit. Also return codes from functions, and chains don't count.
```shell script
set -o errexit
```


### Arithmetic
```shell
echo $((1 + 2))
```

### List files sorted by size
```shell
du -rh | sort -h
```

### CSH variables
```shell
setenv FOO bar
```

### File Name search
```shell
find / -name ".c" -print
```

### File Name search (not matches)
```shell
find / \! -name ".c" -print
```

