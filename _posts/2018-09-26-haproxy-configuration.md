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


```
#---------------------------------------------------------------------
# Example configuration for a possible web application.  See the
# full configuration options online.
#
#   http://haproxy.1wt.eu/download/1.4/doc/configuration.txt
#
#---------------------------------------------------------------------

#---------------------------------------------------------------------
# Global settings
#---------------------------------------------------------------------
global
    # to have these messages end up in /var/log/haproxy.log you will
    # need to:
    #
    # 1) configure syslog to accept network log events.  This is done
    #    by adding the '-r' option to the SYSLOGD_OPTIONS in
    #    /etc/sysconfig/syslog
    #
    # 2) configure local2 events to go to the /var/log/haproxy.log
    #   file. A line like the following can be added to
    #   /etc/sysconfig/syslog
    #
    #    local2.*                       /var/log/haproxy.log
    #
    log         127.0.0.1 local2

    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     99999
    user        haproxy
    group       haproxy
    daemon

    #Be careful this line
    tune.ssl.default-dh-param 2048

    # turn on stats unix socket
    stats socket /var/lib/haproxy/stats

#---------------------------------------------------------------------
# common defaults that all the 'listen' and 'backend' sections will
# use if not designated in their block
#---------------------------------------------------------------------
defaults
    mode                    http
    log                     global
    option                  httplog
    option                  dontlognull
    option http-server-close
    option forwardfor       except 127.0.0.0/8
    option                  redispatch
    retries                 3
    timeout http-request    10s
    timeout queue           1m
    timeout connect         10s
    timeout client          1m
    timeout server          1m
    timeout http-keep-alive 10s
    timeout check           10s
    maxconn                 50000

#---------------------------------------------------------------------
# main frontend which proxys to the backends
#---------------------------------------------------------------------
frontend httpssl
    mode tcp
    bind *:443 ssl no-sslv3 crt /etc/haproxy/xyz.pem
#    bind *:443
    default_backend    xyzapi_ssl

frontend http-lan
   bind *:32891
   default_backend http_lan

frontend otaUpater
   bind *:5002
   default_backend     otaUpdater
   acl auth_ota_admin_ok http_auth(ota_admin_user)
   http-request auth realm UserAuth if !auth_ota_admin_ok

#---------------------------------------------------------------------
frontend kibana-log
   bind *:5601
   default_backend kibana
   acl auth_kibana_admin_ok http_auth(kibana_admin_user)
   http-request auth realm UserAuth if !auth_kibana_admin_ok

#---------------------------------------------------------------------
# basic authen (use for kibana)
#---------------------------------------------------------------------
userlist kibana_admin_user
    user admin insecure-password abc!@\#

userlist ota_admin_user
    user admin insecure-password abc!@\#
#---------------------------------------------------------------------
# static backend for serving up images, stylesheets and such
#---------------------------------------------------------------------
backend static
    balance     roundrobin
    server      static 127.0.0.1:4331 check

#---------------------------------------------------------------------
# round robin balancing between the various backends
#---------------------------------------------------------------------
backend xyzapi_ssl
    balance roundrobin
    mode tcp
    server app_ssl1 172.YY.XX.ZZ1:8082 check
    server app_ssl2 172.YY.XX.ZZ2:8082 check

backend http_lan
    balance roundrobin
    server http_lan1 172.YY.XX.ZZ1:8082 check
    server http_lan2 172.YY.XX.ZZ2:8082 check

backend otaUpdater
   balance roundrobin
   server app_ota1 172.YY.XX.ZZ3:5002 check

backend kibana
   balance roundrobin
   server kibana_1 172.YY.XX.ZZ4:5601 check
```
