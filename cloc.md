# Count lines of Code

## Excluding match (only js sources)
```shell
cloc . --not-match-f='\.test\.js$' 
```
    
## Including match (only test sources)
```shell
cloc . --match-f='\.test\.js$' 
```

## cloc ignore subdirectores and files ending in *.test.js
```shell
cloc . --not-match-d=./ --fullpath --not-match-f='\.test\.js$' 
```
