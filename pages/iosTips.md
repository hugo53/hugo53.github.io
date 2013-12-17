---
layout: page
title: "iOS Tips and Tricks"
description: ""
---
{% include JB/setup %}

## Table of Contents
- [Get current orientation](#get-current-orientation)
- [BAD_EXEC error](#bad-exec)
- [Improving Drawing Performance](#drawing-performance)

## Get current orientation {#get-current-orientation}

The below is not the best way. Sometimes, it may return ```UIDeviceOrientationUnknown``` value.
{% highlight objectivec %}
UIDeviceOrientation orientation = [[UIDevice currentDevice] orientation];
{% endhighlight %}

Using the below is better. Based on status bar orientation, we will get current orientation more accuracy.
{% highlight objectivec %}
UIInterfaceOrientation orientation = [[UIApplication sharedApplication] statusBarOrientation];
{% endhighlight %}


## BAD_EXEC error {#bad-exec}
When BAD_EXEC occurs, the best way to detect the root cause is turn on NSZoombie flag in Scheme.
To do that: ```Product``` -> ```Scheme``` -> ```Edit Scheme``` -> ```Diagnostics``` -> Tick on ```Enable Zoombie Objects```. 
Then, in output console, it will show us the crash cause. It usually mentions cause relating to destroyed objects.

## Improving Drawing Performance {#drawing-performance}
Drawing is a relatively expensive operation on any platform, and optimizing your drawing code should always be an important step in your development process. Table [here](https://developer.apple.com/library/ios/documentation/2ddrawing/conceptual/drawingprintingios/DrawingTips/DrawingTips.html#//apple_ref/doc/uid/TP40010156-CH18-SW1) lists several tips for ensuring that your drawing code is as optimal as possible. In addition to these tips, you should always use the available performance tools to test your code and remove hotspots and redundancies.

Belows is some tips.
- Reuse paths by modifying the current transformation matrix. Drawing minimum paths and doing transformation with CTM to translation, rotation, scalation to move context to desired location to draw paths.

	Modifying the CTM is a standard technique for drawing content in a view because it allows you to reuse paths, which potentially reduces the amount of computation required while drawing. For example, if you want to draw a square starting at the point (20, 20), you could create a path that moves to (20, 20) and then draws the needed set of lines to complete the square. However, if you later decide to move that square to the point (10, 10), you would have to recreate the path with the new starting point. Because creating paths is a relatively expensive operation, it is preferable to create a square whose origin is at (0, 0) and to modify the CTM so that the square is drawn at the desired origin.

In the Core Graphics framework, there are two ways to modify the CTM. 
	- Modify the CTM directly using the CTM manipulation functions defined in CGContext Reference. 
	- Create a CGAffineTransform structure, apply any transformations you want, and then concatenate that transform onto the CTM. Using an affine transform lets you group transformations and then apply them to the CTM all at once. 
	You can also evaluate and invert affine transforms and use them to modify point, size, and rectangle values in your code. For more information on using affine transforms, see Quartz 2D Programming Guide and CGAffineTransform Reference.

- Call ```setNeedsDisplay:``` judiciously. Calling this method carefully. If view doesn't not need to redraw, don't call it!