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

- [Security]

	- How _CommonCrypto_ work? What are amazing task supported by Security framework?
	- How to encrypt and decrypt a data stream?

- [Stream Programming](https://developer.apple.com/library/ios/documentation/cocoa/Conceptual/Streams/Articles/CocoaStreamsOverview.html). How NSInputStream and NSOutputStream work? What are benefits of these streams?

	- [For writing file](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/TechniquesforReadingandWritingCustomFiles/TechniquesforReadingandWritingCustomFiles.html#//apple\_ref/doc/uid/TP40010672-CH5-SW3)

	- [Use NSOutputStream as NSInputStream](http://stackoverflow.com/questions/3221030/buffering-nsoutputstream-used-as-nsinputstream), and [here](http://bjhomer.blogspot.co.uk/2011/04/subclassing-nsinputstream.html)

- Class Cluster
- Class Factory Method: like +(id)dataWithContentsOfFile:(NSString *)path;

