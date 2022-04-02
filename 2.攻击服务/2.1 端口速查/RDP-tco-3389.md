## RDP (:3389/tcp) - PROBING & EXPLOITATION

```bash
rdesktop <IP> -u <user> -p <password> -5 -K -r clipboard:CLIPBOARD -g 90% -r disk:E:=/tmp
rdesktop -a 16 -z -u <user>  -p <pass> <IP>

xfreerdp /u:<USER> /p:<PASSWORD> /v:<IP>
xfreerdp /u:<USER> /pth:<NTLM> /v:<IP>
xfreerdp /d:admin /u:lab /v:<IP> +clipboard
```

