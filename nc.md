# nc
Transfer files with netcat on macOS

## Send from Server
Server
```shell
cat thefile | nc -l 5555
```

Client
```shell
nc 192.0.2.55 5555 > thefile
```

## Send from Client
Client
```shell
nc -l 5555 > thefile
```

Server
```shell
cat thefile | nc 192.0.2.55 5555
```


