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