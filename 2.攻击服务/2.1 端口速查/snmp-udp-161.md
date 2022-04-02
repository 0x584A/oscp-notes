## SNMP (:161/udp)- PROBING

```bash
snmpwalk -c public -v1 -t <IP>
snmp-check -c public -v1 <IP>
snmpenum -c public -v1 <IP> 1
snmpenum -t <IP>

onesixtyone -c private -i <IP>

metasploit use auxiliary/scanner/snmp/snmp_enum
```
