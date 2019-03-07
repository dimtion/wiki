Fail2ban
========


Fail to ban will not work with the default configuration on ubuntu 18.04, you
need to specify systemd as the log provider

Change:
```
# /etc/fail2ban/jail.conf
backend = auto
```

into:
```
# /etc/fail2ban/jail.conf
backend = systemd
```
Sources:
* https://unix.stackexchange.com/a/335528
