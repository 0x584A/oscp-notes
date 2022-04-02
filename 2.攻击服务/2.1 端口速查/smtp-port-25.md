## SMTP (:25/tcp) - PROBING & EXPLOITATION

```bash
nc -nvC <IP> 25
VRFY root
HELO <user>@<domain>.com
listusers
setpassword <USER> <PASSWORD>

nmap -p25 --script smtp-vuln* <IP>
nmap -p25 --script smtp-enum-users <IP>
smtp-user-enum -M VRFY -U list.txt -t <IP>
smtp-user-enum -M RCPT -U /usr/share/wordlists/metasploit/unix_users.txt -t <IP>
smtp-user-enum -M EXPN -u unix_users.txt -t <IP>
smtp-user-enum -M EXPN -D <domain.com> -U unix_users.txt -t <IP>
finger -p "<USER>"
swaks --to <USER>@<DOMAIN>.local --from "<USER>@<DOMAIN>.local" --header "Subject: Status" --body body.txt --attach status.pdf --server <IP> --port 25 --auth LOGIN --auth-user "<USER>@<DOMAIN>.local" --auth-password <PASSWORD>
```

