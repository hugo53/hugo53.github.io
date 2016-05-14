---
layout: page
title: "iOS Tips and Tricks"
description: ""
---
{% include JB/setup %}

## Table of Contents
- [Tip and Trick Document from Apple](#apple-tips)
- [System Functions](#system-function)
	- [Get current orientation](#get-current-orientation)
- [Debugging](#debugging)
	- [BAD_EXEC error](#bad-exec)
	- [Find Unlocalized Strings](#find-unlocalized-string)
- [Core Graphics](#core-graphics)
	- [Improving Drawing Performance](#drawing-performance)
- [Performance](#performance)
	- [Fast enumeration and Block](#fast-enum-and-block)
	- [Call UIView's methods in main thread](#call-uiview-method-in-main-thread)
- [Objective-C Techniques](#objective-c-techniques)

## Tip and Trick Document from Apple {#apple-tips}
- [Advanced App Tricks](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

- [Great Intro App States and Multitasking](https://developer.apple.com/library/ios/documentation/iphone/conceptual/iphoneosprogrammingguide/ManagingYourApplicationsFlow/ManagingYourApplicationsFlow.html#//apple_ref/doc/uid/TP40007072-CH4-SW47). Discuss more about app launching with some first methods at the launch time. 

## System Functions {#system-function}

#### Get current orientation {#get-current-orientation}

The below is not the best way. Sometimes, it may return ```UIDeviceOrientationUnknown``` value.

	UIDeviceOrientation orientation = [[UIDevice currentDevice] orientation];


Using the below is better. Based on status bar orientation, we will get current orientation more accuracy.

	UIInterfaceOrientation orientation = [[UIApplication sharedApplication] statusBarOrientation];

## Debugging {#debugging}

#### BAD_EXEC error {#bad-exec}
When BAD_EXEC occurs, the best way to detect the root cause is turn on NSZoombie flag in Scheme.
To do that: ```Product``` -> ```Scheme``` -> ```Edit Scheme``` -> ```Diagnostics``` -> Tick on ```Enable Zoombie Objects```. 
Then, in output console, it will show us the crash cause. It usually mentions cause relating to destroyed objects.

#### Find Unlocalized Strings {#find-unlocalized-string}
When we localize our app, sometimes the localized text is wrong. So we want to show the error string. It is easy when we add ```-NSShowNonLocalizedStrings YES``` to ```Arguments Passed On Launch``` in 

```
Product > Scheme > Edit Scheme > Run > Arguments > Arguments Passed On Launch
```

The error will be:

```
Localizable string "networkxyz" not found in strings table "Localizable" of bundle CFBundle 0x752de50 </Users/username/Library/Application Support/iPhone Simulator/6.1/Applications/1DE47B22-FB85-4C7A-85C1-B50239FC8206/appName.app> (executable, loaded).
```

## Core Graphics {#core-graphics}

#### Improving Drawing Performance {#drawing-performance}
Drawing is a relatively expensive operation on any platform, and optimizing your drawing code should always be an important step in your development process. Table [here](https://developer.apple.com/library/ios/documentation/2ddrawing/conceptual/drawingprintingios/DrawingTips/DrawingTips.html#//apple_ref/doc/uid/TP40010156-CH18-SW1) lists several tips for ensuring that your drawing code is as optimal as possible. In addition to these tips, you should always use the available performance tools to test your code and remove hotspots and redundancies.

Below is some tips.

- Reuse paths by modifying the current transformation matrix. Drawing minimum paths and doing transformation with CTM to translation, rotation, scalation to move context to desired location to draw paths.

	Modifying the CTM is a standard technique for drawing content in a view because it allows you to reuse paths, which potentially reduces the amount of computation required while drawing. For example, if you want to draw a square starting at the point (20, 20), you could create a path that moves to (20, 20) and then draws the needed set of lines to complete the square. However, if you later decide to move that square to the point (10, 10), you would have to recreate the path with the new starting point. Because creating paths is a relatively expensive operation, it is preferable to create a square whose origin is at (0, 0) and to modify the CTM so that the square is drawn at the desired origin.

In the Core Graphics framework, there are two ways to modify the CTM. 
	
	- Modify the CTM directly using the CTM manipulation functions defined in CGContext Reference. 
	
	- Create a CGAffineTransform structure, apply any transformations you want, and then concatenate that transform onto the CTM. Using an affine transform lets you group transformations and then apply them to the CTM all at once. 
	You can also evaluate and invert affine transforms and use them to modify point, size, and rectangle values in your code. For more information on using affine transforms, see Quartz 2D Programming Guide and CGAffineTransform Reference.

- Call ```setNeedsDisplay:``` judiciously. Calling this method carefully. If view doesn't not need to redraw, don't call it!

## Performance {#performance}

#### Fast enumeration and Block {#fast-enum-and-block}
There are three options to operate with each element in an array:

- [arr makeObjectsPerformSelector:@selector(_stubMethod)];
- Fast enumeration
- enumerateObjectsUsingBlock

According to [this SO discussion](http://stackoverflow.com/questions/4486622/when-to-use-enumerateobjectsusingblock-vs-for/4487012#4487012), ```enumerateObjectsUsingBlock``` is the fastest, then ```Fast enumeration``` and the last is ```makeObjectsPerformSelector```. 

#### Call UIView's methods in main thread {#call-uiview-method-in-main-thread}
	Manipulations to your application’s user interface must occur on the main thread. Thus, you should always call the methods of the UIView class from code running in the main thread of your application. The only time this may not be strictly necessary is when creating the view object itself but all other manipulations should occur on the main thread.

	from [Threading Considerations](https://developer.apple.com/library/ios/documentation/uikit/reference/uiview_class/UIView/UIView.html#//apple_ref/doc/uid/TP40006816-CH3-SW147). 

## Objective-C Techniques {#objective-c-techniques}

#### Create private method {#create-private-method}
In Objective-C, there is no ```public```, ```protected``` or ```private``` scope for method, they are just for instance variable. Hence, if we need to make some methods go private, two approaches should be in mind. 

##### Using category

	/*We declare the category*/
	@interface  MyClass (private)

	@property (nonatomic, retain) Type *aPrivateProperty; 
	- (void)aPrivateMethod;

	@end

	/*We implement it*/    
	@implementation MyClass (private)

	/*We cannot use synthesize in the category :-(*/
	@dynamic aPrivateProperty;
	- (Type *)aPrivateProperty {
	    //Getter code
	}

	- (void)setAPrivateProperty {
	    //Setter code
	}

	- (void)aPrivateMethod {        
	    //Some code there
	}
	@end

> The above code is of [this post](http://www.benjaminloulier.com/posts/private-properties-methods-and-ivars-in-objective-c/). 

But this approach has some drawbacks:
- You can't use synthesize to generate the implementation of your properties. Instead of, we need to implement runtime accessor methods.
- 

##### Using class extension

	/*We declare the class extension*/
	@interface  MyClass () 

	@property (nonatomic, retain) Type *aPrivateProperty; 
	- (void)aPrivateMethod;

	@end

	/*
	We can use the main implementation block to implement our properties
	and methods declared in the class extension.
	*/
	@implementation MyClass

	/*Therefore we can use synthesize ;-)*/
	@synthesize aPrivateProperty;

	- (void)aPrivateMethod {        
	    //Some code there
	}

	@end

> The above code is of [this post](http://www.benjaminloulier.com/posts/private-properties-methods-and-ivars-in-objective-c/).

In summary, it is better to use the second approach using Class Extension.

#### Use Preprocessor Macros to pre define version type
In some cases, we need to know the version is lite or pro version. There are some ways to do that and using Preprocessor Macros is one. 
We can set Preprocessor Macros in ```Target -> Buil Settings -> Apple LLVM 5.1 - Preprocessing``` and use some phrases like ```Target_LiteVersion``` or ```Target_ProVersion```. 
Then, check in ```application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions``` with statement:

	#if defined (Target_ProVersion)
	    // Do somethings
	#endif



