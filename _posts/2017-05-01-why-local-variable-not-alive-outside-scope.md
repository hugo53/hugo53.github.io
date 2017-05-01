---
layout: post
title: "Why local variable not alive outside scope?"
description: "By chance I got a tech question from my feed which was tagged an Software Architect for answer from him, could say: Why is a local variable only alive inside its codeblock (between two brackets {} in Java)?"
category: 
tags: []
---
{% include JB/setup %}

By chance I got a tech question from my feed which was tagged an Software Architect for answer from him, could say "Why is a local variable only alive inside its codeblock (between two brackets {} in Java)?"

We could say here, it's all about Heap and Stack Memory. 

First, if local variable is an instance of scalar type (value type) (Int, Float, Bool, ...), it will be allocacted in Stack memory within its function statements. So, when function call is end, that block statment on stack will be poped for the next function statements (another function call) running. Of course, the local variable will be destroyed as a result.

In another case, if local variable is an instance of reference type (object, such as Student, Class), it will be hold on Heap memory. Actually, we need to manage manually these variables (malloc/delete in C, or Automatic Reference Counting (ARC) in Objective-C do it for us). As Apple documentation said:

> Objective-C objects, by contrast, are allocated slightly differently. Objects normally have a longer life than the simple scope of a method call. In particular, an object often needs to stay alive longer than the original variable that was created to keep track of it, so an object’s memory is allocated and deallocated dynamically. 

```
https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html
```

It means that reference type local variable may alive longer and outside codeblock, that's called memory leak. So, we need to handle (or automatically handled by compiler like Objective-C with ARC, or simply IDE does not allow us access local variable from outside) to avoid this terrible case, that makes local variable should not live outside codeblock.
