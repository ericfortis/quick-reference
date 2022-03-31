# curl

- `-O`, saves using the remote file name
- `-o`, `--output` (e.g. to `/dev/null` or `myfile.html`)
- `-s`, `--silent` suppresses the progress bar
- `-v`, `--verbose` shows debug messages

## Print Headers only
```shell script
curl --head -XGET https://uidrafter.com 
```
Using `-XGET` because it defaults to a HEAD req.

## Print Status Code only for Health Check
```shell script
curl -so /dev/null --write-out "%{http_code}"  --resolve my.uidrafter.com:443:107.155.81.155  https://my.uidrafter.com/Hc
curl -so /dev/null --write-out "%{http_code}"  --resolve my.uidrafter.com:443:107.155.81.165  https://my.uidrafter.com/Hc
```


## Custom resolve IP 
Serves for Request bypassing Reverse proxies.
This is an alternative for not having to update `/etc/hosts`.
For example, to view the Origins' headers.
```shell script
curl -s --head --resolve uidrafter.com:443:107.155.81.155 https://uidrafter.com
curl -s --head --resolve uidrafter.com:443:107.155.81.165 https://uidrafter.com
```


## POST
```shell script
curl --data "{}" url
```


## Timing
```shell script
curl -o /dev/null -sw 'TCP=%{time_connect} TLS=%{time_appconnect} ALL=%{time_total}\n' https://uidrafter.com
```


## Download all the files from a list and exclude the query string when saving

This is sequential
```shell
xargs -n 1 curl --remote-header-name -O < myfile-list
```
