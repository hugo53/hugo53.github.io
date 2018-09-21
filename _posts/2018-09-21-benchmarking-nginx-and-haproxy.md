---
layout: post
title: "Benchmarking NGINX and HAProxy"
description: "Notes on benchmarking NGINX and HAProxy"
category: translation
tags: []
---
{% include JB/setup %}
We do benchmarking on two kinds of web servers: NodeJS and Java Springboot Netty, using ```wrk``` tool.

## Configuration
#### Hardware
```
Hardware:
Mac Mini (Late 2014)
Processor: 2.6 GHz Intel Core i5
Memory: 8 GB 1600 MHz DDR3
Graphics: Intel Iris 1536 MB
Disk: 256GB SSD

Model Identifier:    Macmini7,1
Processor Name:    Intel Core i5
Processor Speed:    2.6 GHz
Number of Processors:    1
Total Number of Cores:    2
L2 Cache (per Core):    256 KB
L3 Cache:    3 MB
Memory:    8 GB
Boot ROM Version:    MM71.0232.B00
SMC Version (system):    2.24f32
```

#### NGINX
```
user geek admin;
worker_processes  1;
worker_rlimit_nofile 2621440;

error_log   /var/log/nginx/nginx_error.log;
#pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    log_format combined_log '$remote_addr - $remote_user [$time_local]  '
		    '"$request" $status $body_bytes_sent '
		    '"$http_referer" "$http_user_agent"';

    sendfile        on;
    #tcp_nopush     on;
    keepalive_timeout  65;
    #gzip  on;
	
    upstream backend_hosts {
    	server localhost:3000;
    	server localhost:3001;
   }

    server {
        listen       8080;
        listen 	     [::]:8080 default_server;
        server_name  localhost;
    	access_log /var/log/nginx/access.log combined_log;

        location / {
	        proxy_pass http://backend_hosts;
            proxy_set_header Host $host;
        }

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    include servers/*;
}

```

#### HAProxy
```
global
  ulimit-n 999999
  maxconn 999999
  maxpipes 99999

frontend http-frontend
  mode tcp
  bind *:8080
  default_backend http-backend

backend http-backend
  mode tcp
  balance roundrobin
  server http-instance-0 localhost:3000 maxconn 999999
  server http-instance-1 localhost:3001 maxconn 999999

```

## NodeJS
```
➜  nginx-nodejs pm2 status           

┌──────────┬────┬─────────┬──────┬────────┬─────────┬────────┬──────┬───────────┬──────┬──────────┐
│ App name │ id │ mode    │ pid  │ status │ restart │ uptime │ cpu  │ mem       │ user │ watching │
├──────────┼────┼─────────┼──────┼────────┼─────────┼────────┼──────┼───────────┼──────┼──────────┤
│ app      │ 0  │ cluster │ 2108 │ online │ 0       │ 37m    │ 0.1% │ 38.9 MB   │ geek │ disabled │
│ app      │ 1  │ cluster │ 2109 │ online │ 0       │ 37m    │ 0.1% │ 80.2 MB   │ geek │ disabled │
│ app      │ 2  │ cluster │ 2119 │ online │ 0       │ 37m    │ 0.1% │ 79.8 MB   │ geek │ disabled │
│ app      │ 3  │ cluster │ 2120 │ online │ 0       │ 37m    │ 0%   │ 39.0 MB   │ geek │ disabled │
└──────────┴────┴─────────┴──────┴────────┴─────────┴────────┴──────┴───────────┴──────┴──────────┘
```

#### HAProxy
```
➜  nginx-nodejs wrk -t4 -d30s -c400 http://localhost:8080
Running 30s test @ http://localhost:8080
  4 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    45.81ms   84.98ms   1.99s    98.79%
    Req/Sec     2.61k   622.91     4.03k    71.24%
  311983 requests in 30.04s, 66.80MB read
  Socket errors: connect 0, read 140, write 3, timeout 9
Requests/sec:  10387.29
Transfer/sec:      2.22MB
```

#### Nginx
```
➜  nginx-nodejs wrk -t4 -d30s -c400 http://localhost:8080
Running 30s test @ http://localhost:8080
  4 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   205.02ms   81.42ms 501.27ms   67.17%
    Req/Sec   456.36    168.73     0.90k    67.71%
  29334 requests in 30.05s, 6.90MB read
  Socket errors: connect 0, read 127, write 0, timeout 0
Requests/sec:    976.12
Transfer/sec:    234.97KB
```

## Java SpringBoot Netty

Two Spring applications run at port 3000, 3001.

#### HAProxy
```
➜  nginx-nodejs wrk -t4 -d30s -c400 http://localhost:8080
Running 30s test @ http://localhost:8080
  4 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    17.95ms   10.18ms 185.36ms   89.13%
    Req/Sec     5.76k     1.58k    9.08k    68.54%
  688692 requests in 30.11s, 73.54MB read
  Socket errors: connect 0, read 2, write 0, timeout 0
Requests/sec:  22874.94
Transfer/sec:      2.44MB
```

#### NGINX
```
➜  nginx-nodejs wrk -t4 -d30s -c400 http://localhost:8080
Running 30s test @ http://localhost:8080
  4 threads and 400 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    61.40ms   29.62ms 256.70ms   87.30%
    Req/Sec     1.68k   653.56     3.14k    62.29%
  201222 requests in 30.09s, 121.69MB read
  Socket errors: connect 0, read 1, write 0, timeout 0
  Non-2xx or 3xx responses: 184885
Requests/sec:   6686.79
Transfer/sec:      4.04MB
```

## References
- [Another benchmark on NodeJS and JAVA AIO](http://www.olympum.com/java/java-aio-vs-nodejs/)
- [Fun-fact: Arimo (prior Adatao) has used HAProxy from beginning](https://www.quora.com/Which-software-load-balancer-is-better-HAProxy-or-nginx/answer/Christopher-Cuong-Nguyen)