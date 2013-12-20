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
- [At First Glance](#first-glance)
- [Use Case](#use-case)

	- [Drawing fore-front of back-front?](#fore-or-back-front)

## At First Glance {#first-glance}
iOS provides two primary ways for drawing: Core Graphics and Open GL. However, there are more API types of these frameworks. It is simple for OpenGL when it has some clear API named OpenGL ES, Cocos2d. For Core Graphics framework, it may lead junior developer to a confused: Quartz, Quartz 2D or QuartzCore. Firstly, Quartz is heart of CoreGraphics, someone likes to refer Core Graphics as Quartz. Secondly, Quartz 2D is a part of Core Graphics, can refer as an API. Finally, QuartzCore (also known as Core Animation) is for animation, image processing and video image manipulation. It extends some function from Quartz (Core Graphics) but not a part of Quartz. In additional, iOS also provides us UIKit which contains some high-level implementation of Core Graphics, ex. [UIBezierPath](https://developer.apple.com/library/ios/documentation/uikit/reference/UIBezierPath_class/Reference/Reference.html) is high-level implementation of [CGPathRef](https://developer.apple.com/library/mac/documentation/graphicsimaging/reference/CGPath/Reference/reference.html). 

Some terms we should familliar with at the first glance are ```Drawing Context```, ```CTM``` (Current Transformation Matrix), ```Core Graphics Coordinate```.

#### Drawing Context
Everytime you want to draw a path, a shape on screen, you have to know the current context of screen. It is easy to get the current context via:

{% highlight objectivec %}
CGContextRef context = UIGraphicsGetCurrentContext();
{% endhighlight %}
> However, the above line code just need to be written when we draw using CoreGraphics framework. For UIKit with UIBezierPath, this framework gets current context for us automatically. 

Current context is needed to pass to some Core Graphics function to perform painting actions. 

In context, it has some significant properties. The most important property is CTM (which will discuss more below). We can get the CTM of current context by:
{% highlight objectivec %}
CGContextRef context = UIGraphicsGetCurrentContext();
CGAffineTransform ctm =  CGContextGetCTM(context);
{% endhighlight %}

#### CTM (Current Transformation Context)
Current Transformation Context is an Affine Transformation which plays a role in transforming coordinate, including translation, rotation, scale, flip. Everytime we want to transform coordinate, we must combine desired affine transformation with CTM to make a new CTM which describes right origin and direction of the coordinate.

CTM (and Affine Transformation) has 6 parameters: a, b, c, d, tx and ty at positions as below:

|  	       			|    			|  			|
| :-----------------: |:-------------: | :-------------: |
| a  		| 	b		|  	0 |
| c	    	| 	d		|	0 |
| tx		|	ty 		|	1 |


> a, b, c, d is for rotation
>
> a, b is for translation
>
> tx, ty is for scale


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

{% highlight objectivec %}
CGContextSaveGState(kCGBlendModeMultiply);

CGContextSetBlendMode(context, kCGBlendModeNormal);
drawHighlighFrame 	// Yellow frame

CGContextSetBlendMode(context, kCGBlendModeNormal);
drawUnderline		// Blue line

CGContextRestoreGState(ctx);
{% endhighlight %}

![alt text](http://hugo53.github.io/images/coregraphics/fore-front.png "fore front")

- ```kCGBlendModeMultiply``` next drawing is on back-front

{% highlight objectivec %}
CGContextSaveGState(kCGBlendModeMultiply);

CGContextSetBlendMode(context, kCGBlendModeNormal);
drawHighlighFrame 	// Yellow frame

// CGContextSetBlendMode(context, kCGBlendModeNormal);
drawUnderline		// Blue line

CGContextRestoreGState(ctx);
{% endhighlight %}
![alt text](http://hugo53.github.io/images/coregraphics/back-front.png "back front")

> Note: For two above images, desired color of underline is blue. The first image shows right color but the second does not. 

Besure that always saving and restoring context to avoid impact later drawing. 