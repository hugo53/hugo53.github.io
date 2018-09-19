---
layout: post
title: "Benchmarking tool"
description: "Notes on benchmarking tool"
category: translation
tags: []
---
{% include JB/setup %}

## HTTP Benchmark

### Using ```bombardier```
##### JSON response
```
./bombardier-darwin-386 -H 'Content-Type: application/json' -c 125 -n 5000000 http://localhost:8080/
```

##### Raw text response
```
./bombardier-darwin-386 -c 125 -n 5000000 http://localhost:8080/
```

### Monitoring benchmark
To view http response
```
sudo tcpdump -A -i lo0 port 8080
```

will show

```
14:20:52.758255 IP localhost.http-alt > localhost.64521: Flags [P.], seq 1:109, ack 95, win 12756, options [nop,nop,TS val 36667104 ecr 36667102], length 108: HTTP: HTTP/1.1 200 OK
E.....@.@..............	8..j...3..1........
./~../~.HTTP/1.1 200 OK
content-length: 22
content-type: application/json; charset=UTF-8

{"data":"Hello world"}
```

### Using ```wrk```
TODO

### Example
- [https://github.com/jakubkulhan/hit-server-bench](https://github.com/jakubkulhan/hit-server-bench)

