
## SPAWNING SHELL

使用 Socat，获取 socat 可移植二进制文件
> 一旦你在 Linux 机器上执行此操作以获得更好的shell

```bash
echo $TERM
CTRL+Z
stty raw -echo
fg
<ENTER>
<ENTER>
export TERM=<echo $TERM value>
echo $TERM
```



## UPGRADE LINUX TTY

```bash
python -c 'import pty;pty.spawn("/bin/bash")'
perl -e 'exec "/bin/sh";'
echo os.system('/bin/bash')
/bin/sh -i
perl -e 'exec "/bin/sh";'
perl> exec "/bin/sh";
ruby> exec "/bin/sh"
lua> os.execute('/bin/sh')
vi> :!bash
vi> :set shell=/bin/bash:shell
nmap> !sh
export PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```
