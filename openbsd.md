#OpenBSD
## Restart network interface
```bash
sh /etc/netstart em0
sh /etc/netstart # all interfaces 
```

## Remove all unused pkgs
```bash
pkg_delete -a
```

## Connecting to serial from macOS
```shell
screen /dev/tty.usbserial 115200
```

## Upgrading OpenBSD
Copy the new ramdrive as `/bsd.rd`.
Then, in the boot menu type `bsd.rc`. Then choose `U` for upgrade.

```boot> bsd.rd```
