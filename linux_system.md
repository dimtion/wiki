# Linux system notes


Check available entropy:
```bash
cat /proc/sys/kernel/random/entropy_avail
```

If the number is smaller that 100-200 we are lacking entropy.


## SSH

### `ssh-keygen`

Remove a host key:
```bash
ssh-keygen -R <hostname>
```

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


### `numfmt`

Convert numbers to a human readable form:
```
$ numfmt --to=si --suffix=flops 12345678901004
13flops

$ numfmt --to=iec-i --suffix=B 12345678901004
12TiB
```

Sources:
* https://www.gnu.org/software/coreutils/manual/html_node/numfmt-invocation.html#numfmt-invocation

## `sshfs`

SSHFS is a FUSE program that allow a user to mount a filesystem over ssh.

```bash
sshfs -o allow_other,defer_permissions root@xxx.xxx.xxx.xxx:/ /mnt/dest
```

```bash
sshfs -o allow_other,defer_permissions,IdentityFile=~/.ssh/id_rsa root@xxx.xxx.xxx.xxx:/ /mnt/dest
```

It can be added the fstab:
```
# /etc/fstab
sshfs#root@xxx.xxx.xxx.xxx:/ /mnt/droplet
```

Sources:
* DO tutorial: https://www.digitalocean.com/community/tutorials/how-to-use-sshfs-to-mount-remote-file-systems-over-ssh
* OSX installation: http://osxfuse.github.io/


## `flock`: Manage locks from shell scripts

Tool to create an use locks from the a shell script, if the lock cannot be
acquired immediatly flock waits.

```bash
(
flock -s 200

# ... commands executed under lock ..

) 200>/var/lock/mylockfile
```

Lock for 15 seconds, in write mode a single command:
```bash
flock -x -w 15 /var/lock/mylock -c curl resel.fr
```

Source:
* https://linux.die.net/man/1/flock

## System scheduler

Sources:
* https://web.archive.org/web/20131010205236/http://www.ibm.com/developerworks/linux/library/l-scheduler/
* https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_MRG/1.3/html-single/Realtime_Reference_Guide/index.html

### Voluntary and non volontary context switch

**Volontary** context switches happen when a process is waiting for a syscall to
return. **Non volontary** context switch happens when a process has used all its
scheduled CPU time. If the process has a lot of non volontary context switches,
it might mean that it is CPU bound.

The volontary and non volontary context switches can be seen in the
`/proc/<pid>/status/` file:

```
# cat /proc/27288/status
Name:   find
State:  D (disk sleep)
Tgid:  27288
Pid:    27288
PPid:   27245
TracerPid:  0
Uid:    0   0   0   0
Gid:    0   0   0   0
FDSize: 256
Groups: 0 1 2 3 4 6 10
VmPeak:   112628 kB
VmSize:   112280 kB
VmLck:         0 kB
VmHWM:      1508 kB
VmRSS:      1160 kB
VmData:      260 kB
VmStk:       136 kB
VmExe:       224 kB
VmLib:      2468 kB
VmPTE:        88 kB
VmSwap:        0 kB
Threads:    1
SigQ:   4/15831
SigPnd: 0000000000040000
ShdPnd: 0000000000000000
SigBlk: 0000000000000000
SigIgn: 0000000000000000
SigCgt: 0000000180000000
CapInh: 0000000000000000
CapPrm: ffffffffffffffff
CapEff: ffffffffffffffff
CapBnd: ffffffffffffffff
Cpus_allowed:   ffffffff,ffffffff
Cpus_allowed_list:  0-63
Mems_allowed:   00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000001
Mems_allowed_list:  0
voluntary_ctxt_switches:    9950
nonvoluntary_ctxt_switches: 17104
```

And also  in the `/proc/<pid>/sched` file:
```
# cat /proc/27288/sched
find (27288, #threads: 1)
---------------------------------------------------------
se.exec_start                      :     617547410.689282
se.vruntime                        :       2471987.542895
se.sum_exec_runtime                :          1119.480311
se.statistics.wait_start           :             0.000000
se.statistics.sleep_start          :             0.000000
se.statistics.block_start          :     617547410.689282
se.statistics.sleep_max            :             0.089192
se.statistics.block_max            :         60082.951331
se.statistics.exec_max             :             1.110465
se.statistics.slice_max            :             0.334211
se.statistics.wait_max             :             0.812834
se.statistics.wait_sum             :           724.745506
se.statistics.wait_count           :                27211
se.statistics.iowait_sum           :             0.000000
se.statistics.iowait_count         :                    0
se.nr_migrations                   :                  312
se.statistics.nr_migrations_cold   :                    0
se.statistics.nr_failed_migrations_affine:                    0
se.statistics.nr_failed_migrations_running:                   96
se.statistics.nr_failed_migrations_hot:                 1794
se.statistics.nr_forced_migrations :                  150
se.statistics.nr_wakeups           :                18507
se.statistics.nr_wakeups_sync      :                    1
se.statistics.nr_wakeups_migrate   :                  155
se.statistics.nr_wakeups_local     :                18504
se.statistics.nr_wakeups_remote    :                    3
se.statistics.nr_wakeups_affine    :                  155
se.statistics.nr_wakeups_affine_attempts:                  158
se.statistics.nr_wakeups_passive   :                    0
se.statistics.nr_wakeups_idle      :                    0
avg_atom                           :             0.041379
avg_per_cpu                        :             3.588077
nr_switches                        :                27054
nr_voluntary_switches              :                 9950
nr_involuntary_switches            :                17104
se.load.weight                     :                 1024
policy                             :                    0
prio                               :                  120
clock-delta                        :                   72
```

Sources:
* https://stackoverflow.com/a/4430641
* https://blog.tanelpoder.com/2013/02/21/peeking-into-linux-kernel-land-using-proc-filesystem-for-quickndirty-troubleshooting/
## eBPF

eBPF is a virtual machine running in the Linux kernel, it allows system
administrators to probe low level events in the kernel in a secure and a low
overhead way.

One way to develop tools for this eBPF is to use one of the high-level
frameworks such as bcc and bptrace.

bcc tools can be installed on Ubuntu with the
[bpfcc-tools](https://packages.ubuntu.com/bionic/bpfcc-tools) package.

Some tools provided by bcc:

### execsnoop

Print every new process launched by catching calls to `exec()` and `fork()`, useful for short lived processes.

```
# execsnoop-bpfcc
PCOMM            PID    PPID   RET ARGS
sshd             2916   1159     0 /usr/sbin/sshd -D -R
sh               2918   2916     0
env              2919   2918     0 /usr/bin/env -i PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin run-parts --lsbsysinit /etc/update-motd.d
run-parts        2919   2918     0 /bin/run-parts --lsbsysinit /etc/update-motd.d
00-header        2920   2919     0 /etc/update-motd.d/00-header
uname            2921   2920     0 /bin/uname -o
uname            2922   2920     0 /bin/uname -r
uname            2923   2920     0 /bin/uname -m
```

### Other tools
List of tools in bpfcc : https://github.com/iovisor/bcc/blob/master/docs/tutorial.md

* `opensnoop`: print every new `open()` syscalls
* `ext4slower`: print slow operations on ext4 fs
* `biolatency`: create an histogram of block device io latencies
* `biosnoop`: print every block device operation with latency
* `cachestat`: print information cache hits and cache misses
* `tcpconnect`: print a new line for every new tcp connection
* `tcpaccept`: same but only for passive tcp connections
* `tcpretrans`: print tcp retransimissions
* `runqlat`: create an histogram of thread waiting times, helps check cpu saturation
* `profile`: cpu profiler that print passed stack traces


# Sources and documentation
* http://www.brendangregg.com/ebpf.html
* Bcc tutorial: https://github.com/iovisor/bcc/blob/master/docs/tutorial.md
* An introduction to the BPF Compiler collection: https://lwn.net/Articles/742082/
* Some advanced BCC topics: https://lwn.net/Articles/747640/
* Original BPF paper: http://www.tcpdump.org/papers/bpf-usenix93.pdf
* List of links I find: https://shaarli.dimtion.fr/?searchtags=ebpf

