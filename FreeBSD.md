# FreeBSD Quick Reference

Also see: https://github.com/sbz/freebsd-commands

## General
- `poweroff`
- `shutdown -r now` Not `reboot`, it won't gracefully stop the jails.
- `su -` Because `sudo` and `doas` are not installed by default.
- `sockstat -4l` Shows listening sockets (ipv4).
- `tail -f /var/log/messages` Shows the syslog stream.


## Main Configs
- `/etc/rc.conf` Configs, daemons, net, etc.
- `/usr/local/etc/` Configs for user-installed programs.
- `/boot/loader.conf.local` and `/etc/sysctl.conf.local` Kernel tuning
    
    
## Services
- `service openssh restart` We don't use the default _sshd_.
- `service netif restart` Restart network configs.
- `service routing restart`
  - `netstat -rn` Shows the routing table.
  - `route add default 10.0.0.1` If 'routing restart' fails.
- Startup and Control scripts
  - `/etc/rc.d/` Base.
  - `/usr/local/etc/rc.d/` User installed, e.g. node.

## Jails
- `/etc/jail.conf` ip's, resources, etc.
- `jls` Lists jails.
- `service jail restart` Restarts all jails.
- `service jail stop myjail` Jails can't be `poweroff`.

Options for executing in jail:
- `jexec -U root myjail` Enters a jail sh. Then just `exit`.
- `jexec myjail mycommand` 
- `jexec -U <jailuser> myjail mycommand`. Runs as a user of the jail.
- `chroot /jails/myjail/ mycommand` 
- Or ssh into them

Jails can have higher `secure_level` than the host.

The ACL's on jails, when viewed from the host
won't reflect the actual users. But the host user.

### Can't restart them
VERIFY: It seems that this is no longer an issue. This is for both orch and location
servers. Currently, we can't easily restart jails. That's because their jail-to-jail
epairs are fixed at startup and for some reason they're getting deleted in FreeBSD
12.1 when doing `service jail restart`. This didn't happen in FreeBSD 12.0.

Workaround: `shutdown -r now` on the host, or try `service netif restart`


## Firewall
- `pfctl -sa` show everything it can show (rules, state)
- `/etc/pf.conf` Main conf. Handles the jails' NAT.
- `pfctl -vf /etc/pf.conf` Reloads firewall rules. `-v` shows the expanded rules.
- `tcpdump -lnettti pflog0` Shows live logged traffic.
  - e.g. needs rules like `block log`, or `pass log`
- `pfctl -t mytablename -T show`
- `pfctl -t mytablename -T add 10.0.0.10`
- We have a `<ratelimited>` table for manually denying access to abusers if needed 
    - Keep in mind that the Nginx logs won't show their full IP, as the last octect is
     anonymized to "0"


## ZFS
- `zfs list` Shows datasets.
- `zfs snapshot zroot/jails@my_snapshot_name` Creates a snap.
- `zfs list -t snapshot`
- Snapshots can be `clone`, `rollback`, `diff`, `send`, `receive`. 
- `zpool scrub zpool` Repairs (this is done via cron)
  - `zpool status`
  - https://www.freebsd.org/doc/handbook/zfs-zpool.html


## pkg and ports in conjunction
The Ports Collection and pkg are must be on the same branch release of the ports tree.
First, check the pkg url (Quarterly or Latest):
```shell
cat /etc/pkg/FreeBSD.conf | grep url
```
or `cat /usr/local/etc/pkg/repos/FreeBSD.conf`

Install the Ports Collection
```shell
pkg install git
BRANCH=2024Q1
git clone https://git.FreeBSD.org/ports.git -b $BRANCH --depth=1 /usr/ports
```

# Fixing FreeBSD
## Read-Write mount in Single-User mode
For example, if system doesn't boot. BTW, needs the root password. If the root
account doesn't have password, this won't work, we disallowed it in `/etc/ttys`.
In that case see Live CD Boot.
```shell script
mount -u /
zfs mount -a 
```
We don't use UFS, but for reference:
```shell script
mount -u /
mount -a -t ufs 
```

## Live CD Boot
This will also ask for the GELI encryption password.
```shell script
cd /tmp/
mkdir mounted

# For all disks
geli attach /dev/ada0p3
geli attach /dev/ada1p3

zpool import -f -R /tmp/mounted zroot
zfs mount zroot/ROOT/default
```
https://forums.freebsd.org/threads/boot-folder-in-encrypted-zfs-in-freebsd-11.60830/

## Without Disk Encryption password
- Type a wrong password three times (for each disk) 
- Then you can reinstall


## Troubleshooting ports
https://wiki.freebsd.org/BenWoods/DebuggingPorts


## Hardening Guide 
https://vez.mrsk.me/freebsd-defaults.html


# Forensics
See `savecore` for dumping a memory image.


# Upgrade and Updating FreeBSD

## Preparation
### Get the disk password
Get the disk encryption password (for the reboot) and connect to Hivelocity's
VPN. Or use the "Allow my IP temporarily"
feature. Then __Connect to IPMI__ &rarr; Remote Control &rarr; iKVM/HTML5

### Remove the DNS record
Remove the DNS record for the server (start with ServerA)
```shell
./$UXREPO/Infrastructure/experimental/config-clouflare-dns.js
```
When done, re-enable the DNS using that script too.


## Update
First update the Host then the jails. The host's patch-level has to be greater-or-equal than the jails' one.

```shell script
export PAGER=cat
freebsd-update --not-running-from-cron fetch install
for j in `jls name`; do
  freebsd-update --not-running-from-cron -b /jails/$j fetch install
done
logger "Patched FreeBSD"
```

Then restart the system, or just the affected daemon. For example
[SA-21-07](https://www.freebsd.org/security/advisories/FreeBSD-SA-21:07.openssl.asc)
was only for OpenSSL.Therefore, in that case only Nginx was needed to be restarted.

```shell
shutdown -r now "Rebooting for updates"
```

## Packages
Reboot after the system upgrade and before pkg upgrades.
Or upgrade the packages before patching the OS.

```shell script
export ASSUME_ALWAYS_YES=YES
pkg upgrade 
for j in `jls name`; do
  pkg -j $j upgrade 
done
unset ASSUME_ALWAYS_YES
logger "Upgraded all packages"
shutdown -r now "Rebooting after upgrading packages"
```


## Upgrade
- First, **Update** to the latest patch level
- Connect to Hivelocity VPN (needed for entering the disk encryption password)
  It upgrades the jails automatically
```shell script
zfs snapshot zroot@freebsd12_2p6
freebsd-update upgrade -r 13.0-RELEASE
logger "Upgraded FreeBSD 12.2 to 13.0"
reboot
freebsd-update install
```

### Need reboot?
```shell
if [ `freebsd-version -k` == `uname -r` ]; then 
  echo "No reboot needed"
else 
  echo "Please reboot"
fi
```

We no longer compile packages. We compiled mainly to use
`libressl`. But it was too complex and time-consuming.
