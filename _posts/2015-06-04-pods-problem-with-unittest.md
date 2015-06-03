---
layout: post
title: "Pod's problem with UnitTest"
description: "I was in trouble with an error come from Unit Test when I add new Cocoapod library."
category: 
tags: []
---
{% include JB/setup %}
I was in trouble with an error come from Unit Test when I add new Cocoapod library. I import right statement but error still appears when build, like this:

![alt text](http://hugo53.github.io/images/podUnitTest/before.png "Error")

#### How to fix
See the below image for more details. Choose `Pods.debug` for `XYZTests` in `Debug`.

![alt text](http://hugo53.github.io/images/podUnitTest/after.png "Error")
