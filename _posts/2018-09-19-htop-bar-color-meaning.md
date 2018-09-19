---
layout: post
title: "htop Bar Color Meaning"
description: "Notes on htop bar color meaning"
category: 
tags: []
---
{% include JB/setup %}

[```htop```](https://hisham.hm/htop/) is super helpful tool to monitor system usage include cpu, memory, openning threads. As we can see, there are several colors in cpu/memory bar which make us confused their meaning. Fortunately, this [SO question](https://serverfault.com/questions/180711/what-exactly-do-the-colors-in-htop-status-bars-mean) clarifies that. 

![htop-screenshot](https://raw.githubusercontent.com/hugo53/hugo53.github.io/master/images/htop-screenshot.png)

#### Default mode

    Blue: low priority processes (nice > 0)
    Green: normal (user) processes
    Red: kernel time (kernel, iowait, irqs...)
    Orange: virt time (steal time + guest time)

#### Detailed mode

    Blue: low priority threads (nice > 0)
    Green: normal (user) processes
    Red: system processes
    Orange: IRQ time
    Magenta: Soft IRQ time
    Grey: IO Wait time
    Cyan: Steal time
    Cyan: Guest time

#### Memory meters are more straightforward:

    Green: Used memory pages
    Blue: Buffer pages
    Yellow/Orange: Cache pages

_Note: Info obtained from htop source code at [https://github.com/hishamhm/htop/blob/master/CPUMeter.c](https://github.com/hishamhm/htop/blob/master/CPUMeter.c)