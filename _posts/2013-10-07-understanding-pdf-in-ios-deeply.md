---
layout: post
title: "Understanding PDF in iOS Deeply"
description: "Operating with PDF in iOS is a really low-level task. It is not just using some built-in features of iOS to create your own product. Indeed, it requires intensive knowledge at very low iOS layer and at least basic PDF definitions. This post tends to supply some aspects of processing PDF in iOS which aims to help developers easy to win this hard job."
category: 
tags: [pdf, ios]
---
{% include JB/setup %}
Operating with PDF in iOS is really low-level task. It is not just using some built-in features of iOS to create your own product. Indeed, it requires intensive knowledge at very low iOS layer and at least basic PDF definition. This post tends to supply some aspects of processing PDF in iOS which aims to help developers easy to win this hard job.

## PDF Specification
#### Basic Definitions (mixed up Typography)
##### Leading
Leading is space between two lines. According to [Wikipedia](http://en.wikipedia.org/wiki/Leading), _In typography, leading /ˈlɛdɪŋ/ refers to the distance between the baselines of successive lines of type_. However, there are more than one _leading_ definition. The below image shows you 3 ways _leading_ is defined.

![alt text](http://hugo53.github.io/images/pdfpost/leadingoriginal.png "leading")


##### X-height
X-height describes height of __X__ character in a font collection as the image below demonstrates.

![alt text](http://hugo53.github.io/images/pdfpost/line.png "baseline")

##### Descent and Ascent
In typography, **Descent** is a term refer to the part which is below **word baseline** (known as the underline of the word). For example, in the image above, descender is a part from baseline to descent line, ascender is a part from baseline to ascent line. Height of them are **descent**, **ascent** respectively.  

##### Font size
Font size is measuared by distance from ascent line to descent line. In general term, point is unit for desmonstrating font size which is equal to 1/72 inch per point (1 point = 1/72 inch. That means Arial 12pt = 1/6 inch = 4.3 mm). 

##### Glyph
( _Need more study_ )

## Reading PDF Flow
### PDF Operator
PDF uses _operators_ to determine what kind of text will be shown based on pre-defined format. However, it is so complex to understand how pdf is organized. 

	BT % Begin text object
	/F1 1 Tf % Set text font and size 
	64 0 0 64 7.1771 2.4414 Tm % Set text matrix 
	0 Tc % Set character spacing 
	0 Tw % Set word spacing
	ET

In the above block, you can see _Tf_, _Tm_, _Tc_, _Tw_ are four operators which define font (and font size), text matrix, character spacing, word spacing are used. In clearly words, if you want to read and do some processing task relating to pdf content such as highlighting word or bolding word, you must handle as much as possible operators to get exactly text block information for making your job be accurate as your desire. You may need to check [**operator table**](http://my.safaribooksonline.com/book/office-and-productivity-applications/0321304748/operator-summary/app01) to know more about pdf operator.

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
In iOS, you should implement some callback functions for several important operator to help scanner can recognize what sort of value must get for each operator when it scans through the pdf document. Fortunately, iOS supports us by supply _Pop_ functions to get our desire objects when scanner meets a specific operator. Therefore, we must know clearly the format of each operator. For example, in the above block, _Tf_ is font operator, when the value is _Tm_, scanner knows this operator and pop two most recent values: _/F1_ and _1_ by two functions: _CGPDFScannerPopName_ and _CGPDFScannerPopNumber_. 

The following list is displayed _Pop_ functions supplied by iOS.
- CGPDFScannerPopObject
- CGPDFScannerPopBoolean
- CGPDFScannerPopInteger
- CGPDFScannerPopNumber		_to get number, for example: get font's size_
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

