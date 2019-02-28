# Linux system notes


Check available entropy:
```bash
cat /proc/sys/kernel/random/entropy_avail
```

If the number is smaller that 100-200 we are lacking entropy.


## SSH

### SOCKS proxy
Use a dynamic SOCKS proxy with `-D`:
```bash
ssh -D <localport> <remote_host>
```

You then need to configure your web browser to use this SOCKS proxy.


### VPN tunnel

```bash
ssh -w [local_tun]:[remote_tun] <remote_host>
```

The remote server needs to be configured to accept tun interface:
```
# /etc/sshd_config
PermitTunnel yes
```

This will create a tun interface to do a VPN over SSH. However **you need to
configure manually the network once the network is configured.**

sources:
* https://wiki.archlinux.org/index.php/VPN_over_SSH
* https://help.ubuntu.com/community/SSH_VPN
