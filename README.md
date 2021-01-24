
# readme
The code is pulled straight from openbsd divert(4).

Need use case - process diverted packets in userland and re-inject them back
into kernel for further processing (IIUC). Poor man's eBPF for OpenBSD? :-)

# sample pf.conf

Simple divert
pf.conf
```
pass out on $int_if inet proto tcp to port 80 divert-packet port 700
```

# compile and run
```bash
$ cc divert.c -o divert-socket
$ nc -vv www.google.com 80 & # tcp out to 80 diverted to 700 and then re-injected back
$ ./divert-socket 
172.17.100.3:6844 -> 216.58.200.100:80
216.58.200.100:80 -> 172.17.100.3:6844
172.17.100.3:6844 -> 216.58.200.100:80
172.17.100.3:6844 -> 216.58.200.100:80
216.58.200.100:80 -> 172.17.100.3:6844
216.58.200.100:80 -> 172.17.100.3:6844
172.17.100.3:6844 -> 216.58.200.100:80
172.17.100.3:36626 -> 209.216.230.240:80
209.216.230.240:80 -> 172.17.100.3:36626
172.17.100.3:36626 -> 209.216.230.240:80
172.17.100.3:36626 -> 209.216.230.240:80
209.216.230.240:80 -> 172.17.100.3:36626
209.216.230.240:80 -> 172.17.100.3:36626
172.17.100.3:36626 -> 209.216.230.240:80
172.17.100.3:25478 -> 209.216.230.240:80
209.216.230.240:80 -> 172.17.100.3:25478
172.17.100.3:25478 -> 209.216.230.240:80
172.17.100.3:25478 -> 209.216.230.240:80
209.216.230.240:80 -> 172.17.100.3:25478
209.216.230.240:80 -> 172.17.100.3:25478

```
