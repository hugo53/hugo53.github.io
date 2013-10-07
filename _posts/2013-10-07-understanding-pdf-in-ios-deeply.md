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

- [Operator Table](http://my.safaribooksonline.com/book/office-and-productivity-applications/0321304748/operator-summary/app01)

## Reading PDF Flow

### Getting PDF Objects from Scanner Stack
   * CGPDFScannerPopObject
   * CGPDFScannerPopBoolean
   * CGPDFScannerPopInteger
   * CGPDFScannerPopNumber
   * CGPDFScannerPopName		_get font's name_
   * CGPDFScannerPopString		_get content string_
   * CGPDFScannerPopArray
   * CGPDFScannerPopDictionary
   * CGPDFScannerPopStream

> Copy from [CGPDFScanner Reference](http://www.cocoachina.com/wiki/index.php?title=CGPDFScanner_Reference)

## Processing PDF content

## Some add-on elements for showing PDF



# References

