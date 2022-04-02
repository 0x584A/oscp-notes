## 案例

```bash
hashcat -a 0 -m 400 hash --show

# kirbi2 凭证转 hashcat
https://raw.githubusercontent.com/jarilaos/kirbi2hashcat/master/kirbi2hashcat.py
python kirbi2hashcat.py <path_name>.kirbi > krb5tgs_hash
hashcat -m 13100 ./krb5tgs_hash  /usr/share/wordlists/rockyou.txt

hashcat --help | grep -i "KeePass" 
  13400 | KeePass 1 (AES/Twofish) and KeePass 2 (AES)      | Password Managers
```



## 密钥对应模式

https://hashcat.net/wiki/doku.php?id=example_hashes

```bash
-m 400 $P$BeOfRd4R....

-m 1000 B4B9B02E6F09A9BD760F388B67351E2B --> NT Hash

-m 3000 299BD128C1101FD6 --> LM Hash

-m 3200 $2y$10$90gyQVv7... --> PHP password_hash  bcrypt $2*$, Blowfish (Unix)


-m 5500 u4-netntlm::kNS:338d...:9526...:cb... --> smb NTLMV1 hash

-m 5600 admin::N46iSNekpT:08ca4...:5c7830...  --> [SMB] NTLMv2 Hash

-m 13100 $krb5tgs$23$7ffc7...

-m 13400 $keepass$*2*6000*0*1af405cc00f979ddb....
```
