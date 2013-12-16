---
layout: page
title: "iOS Tips and Tricks"
description: ""
---
{% include JB/setup %}

## Table of Contents
- [Get current orientation](#get-current-orientation)



## Get current orientation {#get-current-orientation}

The below is not the best way. Sometimes, it may return ```UIDeviceOrientationUnknown``` value.
{% highlight objectivec %}
UIDeviceOrientation orientation = [[UIDevice currentDevice] orientation];
{% endhighlight %}

Using the below is better. Based on status bar orientation, we will get current orientation more accuracy.
{% highlight objectivec %}
UIInterfaceOrientation orientation = [[UIApplication sharedApplication] statusBarOrientation];
{% endhighlight %}


