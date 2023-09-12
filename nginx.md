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

## gzip_static Media Types
Without registering the MIME types, gzip_static doesn't work.
BTW, something similar is not needed for `brotli_static`.
```shell
gzip_types 	text/plain text/css application/json application/javascript text/xml; 
```

## `add_header` caveats
https://www.peterbe.com/plog/be-very-careful-with-your-add_header-in-nginx