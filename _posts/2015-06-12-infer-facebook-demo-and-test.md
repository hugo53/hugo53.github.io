---
layout: post
title: "Infer Facebook Demo"
description: "Facebook just opened their analyzing tool for mobile development, primarily focusing on iOS and Android project, called Infer. As Facebook said, A tool to detect bugs in Android and iOS apps before they ship, Infer can analyze and detect much more implicit bugs in a mobile project before sending to users."
category: 
tags: []
---
{% include JB/setup %}
> Facebook just opened their analyzing tool for mobile development, primarily focusing on iOS and Android project, called [Infer](http://fbinfer.com). As Facebook said, A tool to detect bugs in Android and iOS apps before they ship, Infer can analyze and detect much more implicit bugs in a mobile project before sending to users.

Because of its awesomeness, I'm curious and just spent some minutes to give a try. 

Firstly, of course, [Installing Infer](http://fbinfer.com/docs/getting-started.html), needs some terminal lines but that's not difficult to geek. Infer is written by Python by the way.

I checked with [sample project](https://github.com/facebook/infer/tree/2bce7c6c3dbb22646e2d67a2c6ade77f060b4bca/examples/ios_hello) provided by Facebook. Just go to directory contains that project

```cd ios_hello/```

then type in terminal

```infer -- xcodebuild -target HelloWorldApp -configuration Debug -sdk iphonesimulator```

So, Infer takes several minutes to analyze full project and when finish, it will show us some cool stuffs, about MEMORY LEAK, RESOURCE_LEAK, PARAMETER_NOT_NULL_CHECKED, NULL_DEREFERENCE, PREMATURE_NIL_TERMINATION_ARGUMENT and so on. Absolutely awesome!

![alt text](http://hugo53.github.io/images/infer/infer-result.png "Infer result")
