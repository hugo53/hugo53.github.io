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

##### Descender and Ascender
In typography, **Descender** is a term refer to the part which is below **word baseline** (known as the underline of the word) and *Ascender** is the part of charater which is above the **meanline**. For example, in the image above, descender is a part from baseline to descent line (known as **beardline**), ascender is a part from meanline to ascender line (known as **topline**). Height of them are **descent**, **ascent** respectively. That means the height of a character is sum of three part: descent, x-height and ascent.

![alt text](http://cdn.designinstruct.com/files/243-basics_typography/02_lines.jpg "baseline")

##### Font size
Font size is measuared by distance from ascent line to descent line. In general term, point is unit for desmonstrating font size which is equal to 1/72 inch per point (1 point = 1/72 inch. That means Arial 12pt = 1/6 inch = 4.3 mm). In PDF specification, Font information is usually in _Resources_ dictionary which contains a _Font_ dictionary. We can open PDF file by a text editor and search for _Resources_ keyword to locate Font description.

##### Glyph
( _Need more study_ )

#### Font

##### Font type (in PDF scope)
There are some ways to divide font type into bucket. We can base on how a font be organized or how a font can be drawn.

If using font structure, we have two kinds:
- ```Simple Font```
- ```Composite Font```

Only ```Type 0``` is composite font. Others are of simple font, included: ```Type 1```, ```Type 3```, ```TrueType```, ```CIDFont```.

Or if using drawing method, we also have two buckets:
- Bitmap-based font: ```Type 3```
- Outline font (Raster font): ```Type 0```, ```Type 1```, ```TrueType```, ```CIDFont```. 

##### Font and CIDFont
Font is a collection of glyphs. CIDFont is also a set of glyphs. But, CIDFont is only used as a component of Type 0 font, cannot be used directly like other Fonts .

##### Descendant Font
is CIDFont, Type 0 will use glyphs of descendant font to draw characters. 


##### CIDToGIDMap
When CIDToGIDMap is Identity:

##### Font Symbolic

##### Font program

##### Font dictionary

## Reading PDF Flow

### PDF Operator

PDF uses _operators_ to determine what kind of text will be shown based on pre-defined format. However, it is so complex to understand how pdf is organized. 

	BT % Begin text object
	/F1 1 Tf % Set text font and size 
	64 0 0 64 7.1771 2.4414 Tm % Set text matrix 
	0 Tc % Set character spacing 
	0 Tw % Set word spacing
	ET

In the above block, you can see _Tf_, _Tm_, _Tc_, _Tw_ are four operators which define font (and font size), text matrix, character spacing, word spacing are used. In clearly words, if you want to read and do some processing tasks relating to pdf content such as highlighting word or bolding word, you must handle with as much as possible operators to get exactly text block information for making your job be accurate as your desire. You may need to check [**operator table**](http://my.safaribooksonline.com/book/office-and-productivity-applications/0321304748/operator-summary/app01) to know more about pdf operators.

### Scanner Stack
Stack is data structure to store PDF Objects when PDF file is being read. Prefix strategy is the method to read objects from object stack. Below is an example.

	BT % Begin text object
	/F1 1 Tf % Set text font and size 
	64 0 0 64 7.1771 2.4414 Tm % Set text matrix 
	0 Tc % Set character spacing 
	0 Tw % Set word spacing
	ET

Stack will be:

BT -> /F1 -> 1 -> Tf(operator) -> 64 -> 0 -> 0 -> 64 -> 7.1771 -> 2.4414 -> Tm(operator) -> ... -> ET (top of the stack).


### Getting PDF Objects from Scanner Stack
In iOS, you should implement some callback functions for several important operators to help scanner can recognize what sort of value must get for each operator when it scans through the pdf document. Fortunately, iOS supports us by providing _Pop_ functions to get our desire objects when scanner meets a specific operator. Therefore, we must know clearly the format of each operator. For example, in the above block, _Tf_ is font operator, when the value is _Tf_, scanner knows this operator and pop two most recent values: _/F1_ and _1_ by two functions: _CGPDFScannerPopName_ and _CGPDFScannerPopNumber_.

The following list displays _Pop_ functions supplied by iOS.
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

## Parsing PDF Content
To parse exactly PDF content, we need to read all plain PDF information. However, except some pure control data, all streams in PDF are usually represented in compressed format. That means we need to decompress them to get the pure text. The compress algorithm is given clearly in  stream description, for example:

{% highlight bash %}
12 0 obj
<< /Length 13 0 R /N 3 /Alternate /DeviceRGB /Filter /FlateDecode >>
{% endhighlight %}

/FlateDecode lets us that the encode algorithm is Flate and should use this algorithm to decode the stream. 

As a standard, almost PDF creator use Flate Encoder to compress stream. Therefore, to decode that data, we can use some tools. In my experience, I usually use [PDFToolkit](http://www.pdflabs.com/tools/pdftk-server/) (Server version) like this:

{% highlight bash %}
$ pdftk compressedPDF.pdf output decompressedPDF.pdf uncompress
{% endhighlight %}

Then, use ```less``` command to see decoded text:

{% highlight bash %}
$ less decompressedPDF.pdf
{% endhighlight %}

Do that, now we can see pure PDF text specification. Hence, we can parse PDF content based on PDF Operators Specification.

> By a reason that PDF stream (text content, image content, ...) is usually compressed for decreasing data size, PDF Reader and/or PDF Parser must uncompresses that stream before rendering or parsing. Almost PDF Document stream is decoded using FlateDecode filter which is based on the zlib/deflate algorithm. For getting plain text to analyze, we can use some tools to convert compressed PDF to uncompressed PDF. In my case, I utilize [pdftk](http://www.pdflabs.com/tools/pdftk-server/) with the command:

{% highlight bash %}
pdftk compressedPDF.pdf output uncompressedPDF.pdf uncompress
{% endhighlight %}

> Besides that, someone suggests [itextrups](http://sourceforge.net/projects/itextrups/) which is laid on iText library (written in Java). See the discussion [here](http://stackoverflow.com/questions/13368660/view-source-on-pdf-rendered-inside-a-browser). 

## Processing PDF content

#### Get Text Rect

[Get Text Rect](http://stackoverflow.com/questions/4255298/how-does-an-annot-cgpdfdictionary-rect-translate-to-objective-c-rect/4255586#4255586)

{% highlight objectivec %}
CGPDFArrayRef rectArray;
if(CGPDFDictionaryGetArray(annotDict, "Rect", &rectArray)) {
    //continue;
    CGPDFReal coords[4];

    for( int k = 0; k < arrayCount; ++k ) {
        CGPDFObjectRef rectObj;
        if(!CGPDFArrayGetObject(rectArray, k, &rectObj)) {
            continue;
        }

        CGPDFReal coord;
        if(!CGPDFObjectGetValue(rectObj, kCGPDFObjectTypeReal, &coord)) {
            continue;
        }

        coords[k] = coord;
    }      
}

//blx,bly,trx,try>tlx,tly,w,h
CGRect rect = CGRectMake(coords[0],coords[3],coords[2]-coords[0],coords[3]-coords[1]);
{% endhighlight %}

## Some add-on elements for showing PDF

### Annotation

### Linking Media/Web Elements

## Performing with Font and Charset Encoding

#### Problem with Identity-H Encoding
Identity-H is a encoding which is used by Google Docs (when you export your docs to PDF file). 


## References

#### For starter

- [Very detail introduction for reading PDF Content. Just paraphase the PDF specification but truly understandable. It is the best for newbie.](http://www.gnupdf.org/Introduction_to_PDF#A_first_example). The backup file is at [here](https://www.dropbox.com/s/lg169kwgnu552bj/Introduction%20to%20PDF%20-%20GNUpdf.pdf).

- [PDF Techniques by W3C.](http://www.w3.org/WAI/GL/WCAG20-TECHS/pdf.html)

- [Great summary](http://stackoverflow.com/questions/3889634/fast-and-lean-pdf-viewer-for-iphone-ipad-ios-tips-and-hints?rq=1)

- [PDF Specification, v.1.7](http://www.verypdf.com/document/pdf-format-reference/bookmark.htm). HTML form of PDF Specification v.1.7, 2006. I found that it is very clear and easy to focus rather than read a PDF version.

#### Advance
- [Displaying and Searching PDF Content on iPhone](http://blog.random-ideas.net/?p=184) from scratch, for the first time do with PDF in iOS. Really helpful and full of information.

- [Cocoa China docs](http://www.cocoachina.com/wiki/index.php?title=CGPDFDocument_Reference). Re-write Apple đocs in easy follow way. More information than original Apple docs.

- [List of limitations in performing with PDF in iOS](http://www.badrit.com/blog/2012/2/1/ios-sdk-pdf-limitations)

- [Parsing table of contents](http://stackoverflow.com/questions/4495413/pdf-table-of-contents-parsing-with-ios-quartz-2d) by PSPDFKit creator.

- [Great slide describe how to understand PDF Specification](https://www.dropbox.com/s/y6ahxkwpypyymmg/PDFTutorial050411Annoted.pdf).


### Open Sources for iOS
- [PDF Reader](https://github.com/vfr/Reader) and [PDFViewer](https://github.com/vfr/Viewer) by Julius Oklamcak.
- [DTPDF - Wrapper for Scanning PDF](https://github.com/Cocoanetics/DTPDF) by Oliver Drobnik.
- [PDFKitten - A search and highlight library](https://github.com/KurtCode/PDFKitten) by Marcus Hedenström.
- [FastPdfKit](https://github.com/mobfarm/FastPdfKit)

### Open sources for other language
- [Mozilla PDF](https://github.com/mozilla/pdf.js). Good but there are some mistakes when parses complex tables. Take a test with this [doc](https://www.dropbox.com/s/4i5o3e55pm9vmrp/DSN_Magazin_1303.pdf) and [this](https://www.dropbox.com/s/nn9dlzwdgaqjacr/issue_deapgschlagmannprodukte_238b99b0.1.pdf).

- [PDF Minner](https://github.com/euske/pdfminer). Great pdf parser but still has mistakes with PDF generated by Google Docs which using composite font and Identity-H Encoding. Give a try with [this](https://www.dropbox.com/s/arpkkzvi9e7evfc/Untitleddocument2.pdf) document (its output is at [here](https://www.dropbox.com/s/g4jq9t7taahdgce/googledocs2.txt)).

- [mupdf](https://github.com/yolanother/mupdf). This library is used by [librelio](http://www.librelio.com/) on [Android version](https://github.com/libreliodev/android/tree/72444610a1d02800165e04e3d70342f8e688145c/src/com/artifex/mupdf).

- [PDFBox](http://pdfbox.apache.org/). An Apache opensource which supports fully PDF tasks including text extraction, filling, splitting, merging, signing, printing and PDF/A Validation. It's written by Java.

- [iText](http://itextpdf.com/). Also using Java as mainly language. Support backend and has an [Android porting version](https://github.com/itext/itextg).




