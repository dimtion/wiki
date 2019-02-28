# Linux system notes


Check available entropy:
```bash
cat /proc/sys/kernel/random/entropy_avail
```

If the number is smaller that 100-200 we are lacking entropy.
