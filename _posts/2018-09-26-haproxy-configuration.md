---
layout: post
title: "HAProxy configuration"
description: "Notes on HAProxy configuration"
category: translation
tags: []
---
{% include JB/setup %}
```
global
  ulimit-n 400039
  maxconn 99999
  maxpipes 99999
  tune.maxaccept 500
  log 127.0.0.1 local0
  log 127.0.0.1 local1 notice
  chroot /var/lib/haproxy
  user haproxy
  group haproxy
  stats socket /var/lib/haproxy/stats
  stats socket /var/run/haproxy.sock mode 600 level admin

defaults
  log global
  mode http
  option dontlognull
  timeout connect 5000ms
  timeout client 50000ms
  timeout server 50000ms
  retries        3
#  errorfile 400 /etc/haproxy/errors/400.http
#  errorfile 403 /etc/haproxy/errors/403.http
#  errorfile 408 /etc/haproxy/errors/408.http
#  errorfile 500 /etc/haproxy/errors/500.http
#  errorfile 502 /etc/haproxy/errors/502.http
#  errorfile 503 /etc/haproxy/errors/503.http
#  errorfile 504 /etc/haproxy/errors/504.http


listen mqtt
  bind *:8883
  mode tcp
  #Use this to avoid the connection loss when client subscribed for a topic and its idle for sometime
  option clitcpka # For TCP keep-alive
  timeout client 3h #By default TCP keep-alive interval is 2hours in OS kernal, 'cat /proc/sys/net/ipv4/tcp_keepalive_time'
  timeout server 3h #By default TCP keep-alive interval is 2hours in OS kernal
  option tcplog
  balance leastconn
  server mosca_1 172.YY.ZZ.XX1:8884 check
  server mosca_2 172.YY.ZZ.XX2:8884 check

#---------------------------------------------------------------------
# stats
#---------------------------------------------------------------------
listen stats *:9040
        mode            http
        log             global

        maxconn 9999

        timeout client  100s
        timeout server  100s
        timeout connect 100s
        timeout queue   100s

        stats enable
        stats hide-version
        stats refresh 30s
        stats show-node
        stats auth admin:password
        stats uri /stats
```

```
global
  ulimit-n 400039
  maxconn 99999
  maxpipes 99999
  tune.maxaccept 500
  log 127.0.0.1 local0
  log 127.0.0.1 local1 notice
  chroot /var/lib/haproxy
  user haproxy
  group haproxy
  stats socket /var/lib/haproxy/stats
  stats socket /var/run/haproxy.sock mode 600 level admin

defaults
  log global
  mode http
  option dontlognull
  timeout connect 5000ms
  timeout client 50000ms
  timeout server 50000ms
#  errorfile 400 /etc/haproxy/errors/400.http
#  errorfile 403 /etc/haproxy/errors/403.http
#  errorfile 408 /etc/haproxy/errors/408.http
#  errorfile 500 /etc/haproxy/errors/500.http
#  errorfile 502 /etc/haproxy/errors/502.http
#  errorfile 503 /etc/haproxy/errors/503.http
#  errorfile 504 /etc/haproxy/errors/504.http


listen mqtt
  bind *:8883
  mode tcp
  #Use this to avoid the connection loss when client subscribed for a topic and its idle for sometime
  option clitcpka # For TCP keep-alive
  timeout client 3h #By default TCP keep-alive interval is 2hours in OS kernal, 'cat /proc/sys/net/ipv4/tcp_keepalive_time'
  timeout server 3h #By default TCP keep-alive interval is 2hours in OS kernal
  option tcplog
  balance leastconn
  server mosca_1 172.YY.ZZ.XX1:8884 check
  server mosca_2 172.YY.ZZ.XX2:8884 check
```