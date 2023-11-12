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


## Email on IP change
An alternative to Dynamic DNS services.

```shell
cat > ~/.email-on-ext-ip-change.sh << 'EOF'
#!/bin/sh

if [ ! -f ~/.oldip ]; then
	echo 0 > ~/.oldip
fi

old_ip=`cat ~/.oldip`
new_ip=`ifconfig em0 | awk '/inet/ {print $2}' 2>&1`
to_email=john@example.com

if [ "$new_ip" != "$old_ip" ]; then
	echo $new_ip > ~/.oldip
	echo $new_ip | mail -s "Your IP Changed" $to_email
fi
EOF
```

**Run it every minute**
```shell
chmod +x .email-on-ext-ip-change.sh
crontab -e
* * * * * $HOME/.email-on-ext-ip-change.sh
```