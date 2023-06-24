# Tips

## Benchmarking
```shell script
sudo apt install apache2-utils
ab -c 100 -n 5000 -r https://my.uidrafter.com/login
```

## Lorem Ipsum IPs
These addresses are reserved for use in documentation and sample configurations
```shell script
192.0.2.0/24
198.51.100.0/24
203.0.113.0/24
```

## Measure SSH Connection time
```shell script
time ssh hvm true
```
The `true` built-in command is as fast as it can be.


## socat
```shell
brew install socat
sudo pacman -S socat
```
TODO I think these proxy/relays can be done with `netcat`, which is included in macOS and Arch Linux.


Use `10.0.0.10:7777` as a proxy for `127.0.0.1:7777`
```shell
socat TCP4-LISTEN:7777,bind=10.0.0.10,fork,reuseaddr TCP4:127.0.0.1:7777
```

Use `0.0.0.0:8888` as a proxy for `127.0.0.1:7777`. Note that the ports have to
be different, otherwise it would fail trying to reuse the target (localhost:7777)
```shell
socat TCP4-LISTEN:8888,fork,reuseaddr TCP4:127.0.0.1:7777
```



