---
layout: post
title: "Understanding PDF in iOS Deeply"
description: "Operating with PDF in iOS is really low-level task. It is not just using some built-in features of iOS to create your own product. Indeed, it requires intensive knowledge at very low iOS layer and at least basic PDF definition. This post tends to supply some aspects of processing PDF in iOS which aims to help developers easy to win this hard job."
category: 
tags: [pdf, ios]
---
{% include JB/setup %}
Operating with PDF in iOS is really low-level task. It is not just using some built-in features of iOS to create your own product. Indeed, it requires intensive knowledge at very low iOS layer and at least basic PDF definition. This post tends to supply some aspects of processing PDF in iOS which aims to help developers easy to win this hard job.

## PDF Specification
#### Basic Definitions (mixed up Typography)
##### Leading
Leading is space between two lines. According to [Wikipedia](http://en.wikipedia.org/wiki/Leading), _In typography, leading /ˈlɛdɪŋ/ refers to the distance between the baselines of successive lines of type_

##### X-height
X-height descripts height of _X_ character in a font collection.
##### Glyph

##### Descent

##### Ascent


## Reading PDF Flow
### PDF Operator

- [Operator Table](http://my.safaribooksonline.com/book/office-and-productivity-applications/0321304748/operator-summary/app01)

### Scanner Stack
Stack is data structure to store PDF Objects when PDF file is being read. Prefix strategy is the method to read objects from object stack. Below is an example.

	BT % Begin text object
	/F1 1 Tf % Set text font and size 
	64 0 0 64 7.1771 2.4414 Tm % Set text matrix 
	0 Tc % Set character spacing 
	0 Tw % Set word spacing
	ET

Stack will be:

BT -> /F1 -> 1 -> Tf(operator) -> 64 -> 0 -> 0 -> 64 -> 7.1771 -> 2.4414 -> Tm(operator) -> ... -> ET (top of stack).


### Getting PDF Objects from Scanner Stack
- CGPDFScannerPopObject
- CGPDFScannerPopBoolean
- CGPDFScannerPopInteger
- CGPDFScannerPopNumber		_to get number, for example: get font's size
- CGPDFScannerPopName		_to get name, for example: get font's name_
- CGPDFScannerPopString		_to get content string_
- CGPDFScannerPopArray
- CGPDFScannerPopDictionary
- CGPDFScannerPopStream

> Copy from [CGPDFScanner Reference](http://www.cocoachina.com/wiki/index.php?title=CGPDFScanner_Reference)

## Processing PDF content

## Some add-on elements for showing PDF
### Annotation

### Linking Media/Web Elements


# References

