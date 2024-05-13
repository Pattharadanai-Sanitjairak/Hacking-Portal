# Linux Privilege Escalation with PATH
- Check SUID file using `find / -perm -u=s -type f 2>/dev/null`

<img src=img/SUID.png>

- Test only `unusual` or `user customized binary` to see their functionality. - `/home/murdoch/test`

<img src=img/thm.png>

- If `customized binary` trigger other known scripts such as `thm` - We can trick `thm` to be `shell`
- Append `/tmp` to be a part of PATH using `export PATH=/tmp:$PATH` 

<img src=img/path.png>

- Create custom script name `thm` with content `/bin/bash` and save to `/tmp/thm`
- Add execute permission with `chmod 777 /tmp/thm`
- Then, if the `customized script` is called, it will call `fake thm command` so that it will pop `/bin/bash` or `root shell`.

<img src=img/popshell.png>