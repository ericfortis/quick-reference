# Troubleshooting Tips


## VirtualBox
- Avoid **Close** → **Save State**, it messes up the time.
- Host-Only Adapter
    - Ideal when there's no Internet
    - The Host and VM's can talk to each other
    - VM's can't talk to the outside
- Bridged Adapter
    - VM's can talk to the outside

#### Troubleshooting
The guest connects to the internet, but the host can't access the guest
(e.g. ssh). Maybe the host has both the WiFi and LAN connected. Options:
- Disconnect the non-bridged one.
- Trunk the WiFi and LAN (not possible in macOS AFAIK).



## ssh
### Too many authentication failures
...in Linux for no reason? Try:
```shell script
killall ssh-agent
eval `ssh-agent`
```
By the way, this fixes in ~/.ssh/config
```
Host *
IdentitiesOnly yes
```
If that doesn't help, try connecting with `ssh -v` (verbose).

