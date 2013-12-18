---
layout: post
title: "Core Graphics - Something should be in mind"
description: "Core Graphics is a really low-level framework in iOS which requires more graphics background knowledge to perform drawing fluently. Because of many built-in functions and options, it is hard to catch and understand all of them. This post attempts to provide some common functions/options for very new Core Graphics Hackers."
category: ios
tags: [ios, core graphics]
---
{% include JB/setup %}
> Core Graphics is a really low-level framework in iOS which requires more graphics background knowledge to perform drawing fluently. Because of many built-in functions and options, it is hard to catch and understand all of them. This post attempts to provide some common functions/options for very new Core Graphics Hackers.

## Table of Contents
- [First Glance](#first-glance)
- [Use Case](#use-case)
	- [Drawing fore-front of back-front?](#fore-or-back-front)

## First Glance {#first-glance}
Some terms we should familliar with at the first glance are ```Drawing Context```, ```Current Transformation Matrix (CTM)```, ```Core Graphics Coordinate```.


## Use Case {#use-case}
#### Drawing fore-front of back-front? {#fore-or-back-front}
Core Graphics provides a functions and some options to configure the next drawing action is fore-font or back-front in current context.

{% highlight objectivec %}
CGContextSaveGState(context);

CGContextSetBlendMode(context, blend_option);

CGContextRestoreGState(ctx);
{% endhighlight %}

Blend Options may be:

- ```kCGBlendModeNormal``` next drawing is on fore-front

![alt text](http://hugo53.github.io/images/coregraphics/fore-front.png "fore front")

- ```kCGBlendModeMultiply``` next drawing is on back-front

![alt text](http://hugo53.github.io/images/coregraphics/back-front.png "back front")

Besure that always saving and restoring context to avoid impact later drawing. 