# Serial

```shell
DEV=/dev/cu.usbserial-999
```

I can't simply `cat file > $DEV` because only it could saturate the device
(plotter). For instance, when using **Software Flow Control** my plotter
sends `XOFF` when it can't process more data, conversely sends `XON`.


## Flow Control
(*) Software (XON/XOFF)
( ) Hardware (RTS/CTS)
( ) Hardware (DSR/DTR + RTS/CTS)


## /usr/bin/screen
### Option A (Pasting)
```shell
pbcopy < myfile
screen $DEV
cmd+v
ctrl+a ctrl+\ y (exit)
```

### Option B (Reading)
```shell
screen $DEV 
ctrl+a : readbuf /Users/efortis/myfile
ctrl+a : paste .
ctrl+a ctrl+\ y (exit)
```
