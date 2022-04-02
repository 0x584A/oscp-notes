## RPCBIND(:111/tcp) & MSRPC (:135/tcp) - PROBING & EXPLOITATION
```bash
rpcinfo -p <IP>
    srvinfo
    enumdomusers
    getdompwinfo
    querydominfo
    netshareenum
    netshareenumall

rpcinfo -s <IP>
rpcinfo -p <IP>

rpcclient -U "" -N <IP>
rpcclient -U "" -N <IP> --command=enumprivs -N <IP>

showmount -a <IP>
showmount -e <IP>
showmount -d <IP>
```
