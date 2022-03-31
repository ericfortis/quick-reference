# Nginx
## View compilation flags
```shell
2>&1 nginx -V | xargs -n1 | grep -
```

## Reload
```shell
nginx -s reload
```

## Use a custom config
Clears out the prefix. Otherwise, there's no way to use a relative or absolute path.
```shell
nginx -p '' -c myconf.conf
```
