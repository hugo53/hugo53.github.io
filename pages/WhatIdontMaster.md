---
layout: page
title: "What I do not master, until NOW"
description: "Cloud keywords"
---
{% include JB/setup %}
Don't be curious this page, it's not for you. Go away!

## 7/11/2013
- [NSInvocation](https://developer.apple.com/library/mac/documentation/cocoa/conceptual/distrobjects/Tasks/invocations.html). This is for sending a message at a certain time, not immediately like _shutdown -h +300_

- [NSValue](https://developer.apple.com/library/mac/documentation/cocoa/Conceptual/NumbersandValues/Articles/Values.html#//apple\_ref/doc/uid/20000174-BAJJHDEG)
	
	- [By NHipster](http://nshipster.com/nsvalue/)

- [Compiler Directive & Literal](http://nshipster.com/at-compiler-directives/): relating to '@' symbol in objective. There are many ways to use this symbol, not just for creating a NSString, or defining property.
_@synchronized(){}_ is also a compiler directive.

- [NSOperation and GDC](http://www.raywenderlich.com/19788/how-to-use-nsoperations-and-nsoperationqueues). What is _dispatch\_async()_?

- [What is NSNotificationCenter?](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSNotificationCenter\_Class/NSNotificationCenter\_Class.pdf) and it is for what?

- Security

	- How _CommonCrypto_ work? What are amazing task supported by Security framework?
	- How to encrypt and decrypt a data stream?

- [Stream Programming](https://developer.apple.com/library/ios/documentation/cocoa/Conceptual/Streams/Articles/CocoaStreamsOverview.html). How NSInputStream and NSOutputStream work? What are benefits of these streams?

	- [For writing file](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/TechniquesforReadingandWritingCustomFiles/TechniquesforReadingandWritingCustomFiles.html#//apple\_ref/doc/uid/TP40010672-CH5-SW3)

	- [Use NSOutputStream as NSInputStream](http://stackoverflow.com/questions/3221030/buffering-nsoutputstream-used-as-nsinputstream), and [here](http://bjhomer.blogspot.co.uk/2011/04/subclassing-nsinputstream.html)

- Class Cluster
- Class Factory Method: like +(id)dataWithContentsOfFile:(NSString \*)path;

## 8/11/2013
- The Layer-Based Drawing Model, Layer-Based Animations
- Drawing model

## 12/11/2013
- NSError and its benefits
- The **copy-on-write technique** means that when data is copied through a virtual memory copy, an actual copy of the data is not made until there is an attempt to modify it.
- [File Type Association for making "Open this file with ..."](http://www.infragistics.com/community/blogs/stevez/archive/2013/03/04/associate-a-file-type-with-your-ios-application.aspx)


## 29/11/2013
- CAShapeLayer for draw layer, it has a attribute called 'path'

{% highlight objectivec linenos %}
/* The path defining the shape to be rendered. If the path extends
 * outside the layer bounds it will not automatically be clipped to the
 * layer, only if the normal layer masking rules cause that. Defaults
 * to null. Animatable. (Note that although the path property is
 * animatable, no implicit animation will be created when the property
 * is changed.) */
@property CGPathRef path;
{% endhighlight %}


## 09/12/2013
- Cocoa Data
	- [Build A Cocoa Data App](http://cocoadevcentral.com/articles/000085.php)
	- [iOS: Connecting to an online service and persisting data with CoreData](http://ios-blog.co.uk/tutorials/ios-connecting-to-an-online-service-and-persisting-data-with-coredata/)
	
- [libHN](http://ios-blog.co.uk/tutorials/libhn-cocoa-framework-for-adding-hackernews-to-your-iosmac-app/)

## 16/12/2013
- Core Graphics
	- [Transformation](https://developer.apple.com/library/mac/documentation/graphicsimaging/conceptual/drawingwithquartz2d/dq_affine/dq_affine.html)
	- [Opal](http://svn.gna.org/svn/gnustep/libs/opal/trunk/README)
	- [Bezier Path](https://developer.apple.com/library/ios/documentation/2ddrawing/conceptual/drawingprintingios/BezierPaths/BezierPaths.html)
	- [Drawing Overview](https://developer.apple.com/library/ios/documentation/2ddrawing/conceptual/drawingprintingios/graphicsdrawingoverview/graphicsdrawingoverview.html)
	- [Improve Drawing Performance](https://developer.apple.com/library/ios/documentation/2ddrawing/conceptual/drawingprintingios/DrawingTips/DrawingTips.html#//apple_ref/doc/uid/TP40010156-CH18-SW1)

	- Using Coordinate Transforms to Improve Drawing Performance
Modifying the CTM is a standard technique for drawing content in a view because it allows you to reuse paths, which potentially reduces the amount of computation required while drawing. For example, if you want to draw a square starting at the point (20, 20), you could create a path that moves to (20, 20) and then draws the needed set of lines to complete the square. However, if you later decide to move that square to the point (10, 10), you would have to recreate the path with the new starting point. Because creating paths is a relatively expensive operation, it is preferable to create a square whose origin is at (0, 0) and to modify the CTM so that the square is drawn at the desired origin.

In the Core Graphics framework, there are two ways to modify the CTM. 
	- You can modify the CTM directly using the CTM manipulation functions defined in CGContext Reference. 
	- You can also create a CGAffineTransform structure, apply any transformations you want, and then concatenate that transform onto the CTM. Using an affine transform lets you group transformations and then apply them to the CTM all at once. 
You can also evaluate and invert affine transforms and use them to modify point, size, and rectangle values in your code. For more information on using affine transforms, see Quartz 2D Programming Guide and CGAffineTransform Reference.


#18/12/2013
- [Event Handling](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/GestureRecognizer_basics/GestureRecognizer_basics.html#//apple_ref/doc/uid/TP40009541-CH2-SW2)
- [Great book for practice](http://www.apeth.com/iOSBook/)
