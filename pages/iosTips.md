---
layout: page
title: "IosTips"
description: ""
---
{% include JB/setup %}

## Table of Contents
- [Get current orientation](#get-current-orientation)



## Get current orientation {#get-current-orientation}

The below is not the best way. Sometimes, it may return ```UIDeviceOrientationUnknown``` value.
{% highlight objective-c %}
UIDeviceOrientation orientation = [[UIDevice currentDevice] orientation];
{% endhighlight %}

Using the below is better. Based on status bar orientation, we will get current orientation more accuracy.
{% highlight objective-c %}
UIInterfaceOrientation orientation = [[UIApplication sharedApplication] statusBarOrientation];
{% endhighlight %}


