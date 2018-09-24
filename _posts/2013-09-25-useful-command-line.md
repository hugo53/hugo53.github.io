---
layout: post
title: "Useful command line on Mac OS/Linux"
description: "This post will list some useful commands which are usually used when you do with Linux/Mac OS."
category: 
tags: []
---
{% include JB/setup %}

This post will list some useful commands which are usually used when you do with Linux/Mac OS.

- Show size of file (or files in a directory): *ls -lh file-name/folder-path*
- Show size of current system (included all hard disks): *df -h*
- Remove sleep image (when your boot disk is full): *sudo rm /var/vm/sleepimage*. This command will disable Hibernate mode which is reason for sleep image existing. If you want to enable this mode, should try *sudo pmset -a hibernatemode 3*.

- Check for /Users/username/Library/Developer/Xcode/DerivedData/ to see why harddisk get full.

### Switch user
```
➜  ~ dscl . list /Users | grep -v '^_'
daemon
geek
Guest
nobody
root
➜  ~ whoami
geek
➜  ~ sudo -i
macmini:~ root# whoami
root
macmini:~ root# su - geek
➜  ~ whoami
geek
```

### Kill a range of consecutive processes
```
$ kill -9 {3457..3464}
```

Kill by process command (name)
```
~ sudo lsof -i :8080
COMMAND  PID USER   FD   TYPE      DEVICE          SIZE/OFF NODE NAME
nginx   5381 root    8u  IPv4 0xd68467d8b74c1095      0t0  TCP *:http-alt (LISTEN)
nginx   5381 root    9u  IPv6 0xd68467d8b135a38d      0t0  TCP *:http-alt (LISTEN)
nginx   5382 geek    8u  IPv4 0xd68467d8b74c1095      0t0  TCP *:http-alt (LISTEN)
nginx   5382 geek    9u  IPv6 0xd68467d8b135a38d      0t0  TCP *:http-alt (LISTEN)

~ sudo pkill nginx
```

### Manage connection
- Count number of opening connections at specific port
```
netstat -ant | grep 1883 | grep EST | wc -l
```

- Show overview tcp connections
```
# ss -s (On Linux) (on Mac: netstat -s)
Total: 1788 (kernel 3134)
TCP:   1638 (estab 1409, closed 162, orphaned 0, synrecv 0, timewait 127/0), ports 0

Transport Total     IP        IPv6
*         3134      -         -        
RAW       0         0         0        
UDP       74        69        5        
TCP       1476      1444      32       
INET      1550      1513      37       
FRAG      0         0         0     

```


