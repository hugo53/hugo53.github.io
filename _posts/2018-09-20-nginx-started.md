---
layout: post
title: "NGINX started"
description: "Notes on NGINX"
category: translation 
tags: []
---
{% include JB/setup %}

### Increase requests

```
# vi /usr/local/nginx/conf/nginx.conf (Linux)

# vi /usr/local/etc/nginx/nginx.conf (on MacOS)

# set open fd limit to 30000
worker_rlimit_nofile 30000;

```

```
$ulimit -Hn
$ulimit -Sn
```

```
$launchctl limit maxfiles
maxfiles    256            unlimited 

$sudo launchctl limit maxfiles 512 unlimited
```

```
$sysctl -a

$sysctl kern.maxfiles 
kern.maxfiles: 10240

$sysctl -w variable-name=value  (to set kernal variable)

```

### Change max file on MacOS
```
ulimit -n
```

[For 10.9 (Mavericks), 10.10 (Yosemite),  10.11 (El Capitan), and 10.12 (Sierra):You have to create a file at /Library/LaunchDaemons/limit.maxfiles.plist (owner: root:wheel, mode: 0644):](https://blogs.progarya.dk/blog/how-to-persist-ulimit-settings-in-osx/)

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
        "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>limit.maxfiles</string>
    <key>ProgramArguments</key>
    <array>
      <string>launchctl</string>
      <string>limit</string>
      <string>maxfiles</string>
      <string>262144</string> // -> soft limit (ulimit -Sn)
      <string>524288</string> // -> hard limit (ulimit -Hn)
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>ServiceIPC</key>
    <false/>
  </dict>
</plist>
```

Then, reboot.




