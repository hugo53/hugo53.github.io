---
layout: post
title: "Weird Behaviour with ViewController"
description: "If you're crazy, and if you're creating your awesome app without storyboard or xib, sometimes you may encounter a case which on iOS 7 your view is shifted to bottom automatically a distance of status bar and navigation bar (64px)."
category: 
tags: []
---
{% include JB/setup %}

If you're crazy, and if you're creating your awesome app without storyboard or xib, sometimes you may encounter a case which on iOS 7 your view is shifted to bottom automatically a distance of status bar and navigation bar (64px), like below:

![Before go Background](http://hugo53.github.io/images/weird-behaviour-view-controller/before-go-background.png "back front")

![After go from Background](http://hugo53.github.io/images/weird-behaviour-view-controller/after-go-from-background.png " fore front")

In fact, iOS 7 supports us an option for adjusting view by content inset. By default, it will be:

{% highlight objectivec %}
self.automaticallyAdjustsScrollViewInsets = YES;  // self is view controller
{% endhighlight %}

Then, if you want to get rid of the that default, just do in view controller:

{% highlight objectivec %}
self.automaticallyAdjustsScrollViewInsets = NO;
{% endhighlight %}

You also may face to this problem when do with table view, scroll view.




