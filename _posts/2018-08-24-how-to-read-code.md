---
layout: post
title: "How to read code"
description: "On discussion how to read code effectively"
category: Code Quality
tags: [code, reading, skill]
---
{% include JB/setup %}

To improve programming skill, the best way is to read great programs, such as programs listed at [http://wiki.c2.com/?GreatProgramsToRead](http://wiki.c2.com/?GreatProgramsToRead). However, in the real world, there are lots of programs which are unstructured, inconsistent, undocumented and maintained by many peoples (someones both available or not). So we need a strategy to go through those kinds of program source code. Fortunately, now we have Github, then there are many open source projects to help us practice **reading code** skill. 

As we need a strategy to read code effectively, we may follow these step by step, as Ward Cunningham mentioned in his [wiki](http://wiki.c2.com/?TipsForReadingCode).

## Prepare
### Step 1: Choose project
We should pick up open source projects from companies which have intensive review cycle as Google, Facebook, Amazon, ... 

There are some rules:

- High Github star/fork
- High reputation author/company
- Related business logic

### Step 2: Define Reading Target
We need to specify outcome of reading process:

- To understand business logic behind project
- To fix bug
- To add more feature (extend)

### Step 3: Search for General Information
First of all, we have to know general information about project:

- What is the project?
- What does manual say?
- What is project domain? Is it for medical, transportation or office tool?
- What features it supports? What is it suitable for?
- Which technologies being used?

### Step 4: Thinking
This is one of the most important steps as we need to dig deeply into the project by ourselves before start reading codebase.

- What if you design the whole system?
- What's the core challenge?
- Write down your questions and concerns
- Read with questions


## Reading
### Step 1: Build and Run
Firstly after grab the source code, we need to figure out how to build and run the program successfully. 

Build, to know which external libraries and other dependencies are being used. Of course, we also know linker options and something about compiler. 

It is better if we can run the program in debug mode. Adding logs are also helpful in some cases.

### Step 2: Find entry point
Almost programs have entry point where to start themselves. Using search feature in IDE (or text editor) to find keywords such as **main** (C++/Java project), **__init__** (Python project), **AppDelegate** (Objective-C/Swift project).

From this entry point, we can broaden and examine the whole project.
So it is very important to find out the entry point.


### Step 3: Figure out Architecture & Components
- Try to find project architect and core components. If it is possible, a flow chart can help us understand program working flow. These things can be found from documentation, manual or directory structure. 
- Identify what components to focus and what ones to ignore. Normally, we only need to examine core components and I/O flow.
- Read build file (makefile, pom) to know dependencies.

_Note: Step 3 maybe is Step 1 in several cases upon reading purpose. Sometimes we only need to understand how project construct at first due to shortage of time._

### Step 4: Examine external library call

### Step 5: Figure out which you want to read and search by keyword

### Step 6:  Leverage the Power of Code Comprehension Tools 
- Who calls this method?
- Who implements this interface or subclasses this class?
- What are this class' superclasses?
- Where are instances of this class created, held, and passed as arguments or returned?
- What superclass methods does this class override?
- Where might methods of this class be called polymorphically, i.e. through a base class or interface? 

### Step 7: Comment the code

### Step 8: Write Unit tests

### Step 9: Cleanup the codebase by refactoring skill

### Tips
#### Start at random position
> Reading big random programs reminds me very much of exploring mazes in Angband. I start at some random position (maybe found by grep) and work my way around by traversing callees and callers until I've mapped out enough of the immediate vicinity to see what's going on and do my hack. If I explore enough directions for long enough, things start to connect up and give me a more "global" sense, but often I don't explore so widely. 

#### Observations from Code Reading book
> Typical software evolution activities will require the understanding of code for a number of different purposes. In this chapter we read code to identify the Java source and documentation that needed changing, to find and port code we intended to reuse, and to understand and fix the errors in the new code we introduced. Notice how, in the context of software evolution, code reading is an opportunistic, goal-directed activity. In most cases, **we simply cannot afford to read and understand the code of an entire software system**. Any attempt to precisely analyze code will typically branch into numerous other classes, files, and modules and quickly overwhelm us; we therefore actively **try to limit the extent of code we have to understand to the absolutely necessary minimum**. Instead of striving for a global and complete understanding of the code, we try to find heuristic shortcuts (the book is full of such examples) and employ the build process and the execution of the running system to direct us to the code areas that require our attention. The grep tool and our editor's search functions are the tools of first choice and (often) our last resort. They allow us to quickly search over large code expanses, minimizing the code we need to analyze and understand. In many cases we may reach a dead end, encountering code that is very difficult to comprehend. To escape, we employ a breadth-first search strategy, attacking code-reading problems from multiple sides until one of them gives way.

### Conclusion
The next question is: **Which programs great enough to read?**

### References
- [http://wiki.c2.com/?TipsForReadingCode](http://wiki.c2.com/?TipsForReadingCode)
- [https://blogs.msdn.microsoft.com/csliu/2009/03/04/how-to-read-source-code/](https://blogs.msdn.microsoft.com/csliu/2009/03/04/how-to-read-source-code/)
- [http://himmele.blogspot.com/2012/01/how-do-you-read-source-code.html](http://himmele.blogspot.com/2012/01/how-do-you-read-source-code.html)
- [https://www.skorks.com/2010/05/why-i-love-reading-other-peoples-code-and-you-should-too/](https://www.skorks.com/2010/05/why-i-love-reading-other-peoples-code-and-you-should-too/)
- [http://www.gigamonkeys.com/code-reading/](http://www.gigamonkeys.com/code-reading/)