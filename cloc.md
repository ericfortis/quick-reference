# Count lines of Code

## Excluding match (only js sources)
```bash
cloc . --not-match-f='\.test\.js$' 
```
    
## Including match (only test sources)
```bash
cloc . --match-f='\.test\.js$' 
```

