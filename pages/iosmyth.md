---
layout: page
title: "iOS Myth"
description: ""
---
{% include JB/setup %}

# Table of Contents

- [How an object is created?](#How-an-object-is-created)
- [How many introspection methods?](#How-many-introspection-methods?)
- [isEqual and Hash](#isEqual-and-Hash)
- [Event Queue and how iOS manage events](#Event-Queue-and-how-iOS-manage-events)
- [The Dynamism of Objective-C](#The-Dynamism-of-Objective-C)
- [View, Frame and Bound - What is difference?](#View-Frame-and-Bound-What-is-difference?)
- [Atomic and NonAtomic attributes](#Atomic-and-NonAtomic-attributes)
- [Naming convention for accessor methods](#Naming-convention-for-accessor-methods)
- [Instance variable and Property](#Instance-variable-and-Property)
- [ARC for Foundation framework but not for C-based or Core Foundation Framework](#ARC)
- [Dont use Introspection for Mutable Data](#Dont-use-introspection-for-Mutable-Data)
- [Keyed and Sequential Archivers](#archivers)
- [Inline Method in C and ObjectiveC](#inline-method)
- [Key in NSDictionary](#key-nsdictionary)
- [Objects for Collections type](#object-collection-type)

## How an object is created? {#How-an-object-is-created}

When you allocate an object, part of what happens is what you might expect, given the term. Cocoa allocates enough memory for the object from a region of application virtual memory. To calculate how much memory to allocate, it takes the object’s instance variables into account—including their types and order—as specified by the object’s class.

To allocate an object, you send the message alloc or allocWithZone: to the object's class. In return, you get a _raw_ (uninitialized) instance of the class. The alloc variant of the method uses the application's default zone. A zone is a page-aligned area of memory for holding related objects and data allocated by an application. See Advanced Memory Management Programming Guide for more information on zones.

An allocation message does other important things besides allocating memory:

- It sets the object's retain count to one.
- It initializes the object's isa instance variable to point to the object's class, a runtime object in its own right that is compiled from the class definition.
- It initializes all other instance variables to zero (or to the equivalent type for zero, such as nil, NULL, and 0.0).

An object's isa instance variable is inherited from NSObject, so it is common to all Cocoa objects. After allocation sets isa to the object's class, the object is integrated into the runtime's view of the inheritance hierarchy and the current network of objects (class and instance) that constitute a program. Consequently an object can find whatever information it needs at runtime, such as another object's place in the inheritance hierarchy, the protocols that other objects conform to, and the location of the method implementations it can perform in response to messages.

In summary, allocation not only allocates memory for an object but initializes two small but very important attributes of any object: its isa instance variable and its retain count. It also sets all remaining instance variables to zero. But the resulting object is not yet usable.Initializing methods such as init must yet initialize objects with their particular characteristics and return a functional object.

> ALLOC: Do three tasks but object is not usable. INIT will make allocated object be usable. 

## How many introspection methods? {#How-many-introspection-methods?}
- \[objectX class\]
- \[objectX superclass\]
- \[objectX isKindOfClass\]
- \[objectX isMemberOfClass\]
- \[objectX respondsToSelector:(SEL)selector\]
- \[objectX conformsToProtocol:@protocol(protocolName)\]
- \[objectX isEqual:objectY\]

## isEqual and Hash {#isEqual-and-Hash}
The hash and isEqual: methods, both declared by the NSObject protocol, are closely related. The hash method must be implemented to return an integer that can be used as a table address in a hash table structure. If two objects are equal (as determined by the isEqual: method), they must have the same hash value. If your object could be included in collections such as NSSet objects, you need to define hash and verify the invariant that if two objects are equal, they return the same hash value. The default NSObject implementation of isEqual: simply checks for pointer equality.

### Mutable Objects in Collections
Storing mutable objects in collection objects can cause problems. Certain collections can become invalid or even corrupt if objects they contain mutate because, by mutating, these objects can affect the way they are placed in the collection. First, the properties of objects that are keys in hashing collections such as NSDictionary objects or NSSet objects will, if changed, corrupt the collection if the changed properties affect the results of the object's **hash** or **isEqual:** methods. (If the **hash** method of the objects in the collection does not depend on their internal state, corruption is less likely.) Second, if an object in an ordered collection such as a sorted array has its properties changed, this might affect how the object compares to other objects in the array, thus rendering the ordering invalid.

> If two objects are equal, their hashes must be equal too. But the same hash does not need to be equal. The following example will explain this case.

	If two objects are equal (as determined by the isEqual: method), they must have the same hash value. This last point is particularly important if you define hash in a subclass and intend to put instances of that subclass into a collection.
	
	If a mutable object is added to a collection that uses hash values to determine the object’s position in the collection, the value returned by the hash method of the object must not change while the object is in the collection. Therefore, either the hash method must not rely on any of the object’s internal state information or you must make sure the object’s internal state information does not change while the object is in the collection. Thus, for example, a mutable dictionary can be put in a hash table but you must not change it while it is in there. (Note that it can be difficult to know whether or not a given object is in a collection.)


Note that in all isEqualToType: methods of the Cocoa frameworks, nil is not a valid parameter and implementations of these methods may raise an exception upon receiving a nil. 
However, for backward compatibility, isEqual: methods of the Cocoa frameworks do accept nil, returning NO. In summary, _isEqual_ accepts nil object but _isEqualToType_ is not.



## Event Queue and how iOS manage events {#Event-Queue-and-how-iOS-manage-events}
In the main event loop, an application continuously routes incoming events to objects for handling and, as a result of that handling, updates its appearance and state. An event loop is simply a run loop: an event-processing loop for scheduling work and coordinating the receipt of events from various input sources attached to the run loop. Every thread has access to a run loop. In all but the main thread, the run loop must be configured and run manually by your code. In Cocoa applications, the run loop for the main thread—the main event loop—is run automatically by the application object. What distinguishes the main event loop is that its primary input source receives events from the operating system that are generated by user actions—for example, tapping a view or entering text using a keyboard.

####The Application Object Gets and Dispatches Events
Just after an application is launched, it sets up the infrastructure for the main event loop. It establishes a connection with those underlying system components that are responsible for the delivery of low-level user events. The application receives these events through an input source installed in the main thread’s run loop. Because the application must handle each event separately, in order of its arrival, these low-level events are placed in a first-in first-out event queue.

Once the initial user interface is on the screen, the application is thereafter driven by external events. The application object obtains the topmost object in the event queue, converts it to an event object (UIEvent on iOS, NSEvent on OS X), and dispatches it to other objects in the application for handling. When the call that dispatched the event returns, the application fetches the next object in the queue and dispatches it. It continues doing this until the application terminates.

####Core Objects Respond to Events and Draw the User Interface
When an application is launched, it also sets up a core group of objects that are responsible for drawing the user interface and handling events. These core objects include windows and various kinds of views. When the application object gets an event from the event queue, it dispatches it to the window in which the user event occurred. The window sends the event **to the view** that is the most appropriate handler for it:

- For multitouch and mouse events, **the view** is the one under the touch or mouse pointer.

- For keyboard, motion, and other events, **the view** is the first responder.

If this initial view does not handle the event, it can pass it to other views in the application via the responder chain.

In handling the event, the view often initiates a series of actions that modify the appearance of the application and update the application’s state and data. When these actions have been completed, control returns to the application, which fetches the next event from the event queue.

> Note _First Responder_ comes from here. In summary, iOS uses an event queue in main thread to manage event objects.

## The Dynamism of Objective-C {#The-Dynamism-of-Objective-C}
Objective-C is a very dynamic language. Its dynamism frees a program from compile-time and link-time constraints and shifts much of the responsibility for symbol resolution to runtime, when the user is in control. Objective-C is more dynamic than other programming languages because its dynamism springs from three sources:

 - Dynamic typing—determining the class of an object at runtime
 - Dynamic binding—determining the method to invoke at runtime 
 - Dynamic loading—adding new modules to a program at runtime

For **dynamic typing**, Objective-C introduces the id data type, which can represent any Cocoa object. A typical use of this generic object type is shown in this part of the code example following:


	id word;
	while (word = [enm nextObject]) {
	    // do something with 'word' variable....
	}


The id data type makes it possible to substitute any type of object at runtime. You can thereby let runtime factors dictate what kind of object is to be used in your code. Dynamic typing permits associations between objects to be determined at runtime rather than forcing them to be encoded in a static design. Static type checking at compile time may ensure stricter data integrity, but in exchange for that stricter integrity, dynamic typing gives your program much greater flexibility. And through object introspection (for example, asking a dynamically typed, anonymous object what its class is) you can still verify the type of an object at runtime and thus validate its suitability for a particular operation. (Of course, you can always statically check the types of objects when you need to.)

Dynamic typing gives substance to dynamic binding, the second kind of dynamism in Objective-C. Just as dynamic typing defers the resolution of an object's class membership until runtime, **dynamic binding** defers the decision of which method to invoke until runtime. Method invocations are not bound to code during compilation; they are bound only when a message is actually delivered. With both dynamic typing and dynamic binding, you can obtain different results in your code each time you execute it. Runtime factors determine which receiver is chosen and which method is invoked.

The runtime's message-dispatch machinery enables dynamic binding. When you send a message to a dynamically typed object, the runtime system uses the receiver's isa pointer to locate the object's class, and from there the method implementation to invoke. The method is dynamically bound to the message. And you don't have to do anything special in your Objective-C code to reap the benefits of dynamic binding. It happens routinely and transparently every time you send a message, especially one to a dynamically typed object.

**Dynamic loading**, the final type of dynamism, is a feature of Cocoa that depends on Objective-C for runtime support. With dynamic loading, a Cocoa program can load executable code and resources as they’re needed instead of having to load all program components at launch time. The executable code (which is linked prior to loading) often contains new classes that become integrated into the runtime image of the program. Both code and localized resources (including nib files) are packaged in bundles and are explicitly loaded with methods defined in Foundation's NSBundle class.

This _lazy-loading_ of program code and resources improves overall performance by placing lower memory demands on the system. Even more importantly, dynamic loading makes applications extensible. You can devise a plug-in architecture for your application that allows you and other developers to customize it with additional modules that the application can dynamically load months or even years after the application is released. If the design is right, the classes in these modules will not clash with the classes already in place because each class encapsulates its implementation and has its own namespace.

> Investigate more in Dynamic loading (with NSBundle) and Dynamic binding (Does it relate to performSelector or delegate method)

## View, Frame and Bound - What is difference? {#View-Frame-and-Bound-What-is-difference?}
Firstly, Frame and Bound are two of view's properties.

**A view** is an object that draws itself within a rectangular area of a window and that can respond to user actions such as finger taps or mouse clicks. A view draws a visual representation of itself and presents a surface that responds to touches and input from devices. Not all views handle events, but views are more likely to handle events than other types of responder objects (that is, objects capable of responding to events). Views also provide the content for printing. _For a view to be useful, it must be situated in the view hierarchy of a window._ View  inherit, directly or indirectly, from NSView in Mac OSX or from UIView in iOS.

The frame and bounds of a view define its boundaries and its relationships to other views. 

**The frame** specifies the placement and size of a view within its **superview's coordinate system**.

**The bounds** of a view specifies the **local coordinate system** that a view uses for drawing itself. (Views in UIKit also have a property that locates the center of their rectangular area.)

![alt text](http://hugo53.github.io/images/viewProperties.jpg "leading")


#### Layer and CALayer
In both iOS and OS X, each view is (or can be) backed by a Core Animation layer object (CALayer), which is accessible through the _layer_ property. The layer object caches a view’s drawing content, assists in the layout and rendering of that content, and can composite and animate that content.

### View Hierarchy
A view hierarchy defines the relationships of views in a window to each other. 

- You can think of a view hierarchy as an **inverted tree** structure with the _window being the top node of the tree_. Under it come views structurally specified by parent-child relationships. 

- **From a visual perspective**, the essential fact of a view hierarchy is enclosure: one view contains one or more other views, and the window contains them all.

Each view has three properties to describe relationships with other views: _window, subviews, superview_. See the following image for details.

![alt text](http://hugo53.github.io/images/viewhierarchyrelationships.jpg "leading")

Note that, **in iOS, a Window is a View**. In OS X a window has a single "content view", a background view from which, structurally, all other views in the hierarchy descend. However, in iOS applications, a window is a view (*UIWindow inherits from UIView*), and so it acts as its own content view.

## Atomic and NonAtomic attributes {#Atomic and NonAtomic attributes}
This means that the synthesized accessors ensure that a value is always fully retrieved by the getter method or fully set via the setter method, even if the accessors are called simultaneously from different threads.

Because the internal implementation and synchronization of atomic accessor methods is private, it’s not possible to combine a synthesized accessor with an accessor method that you implement yourself. You’ll get a compiler warning if you try, for example, to provide a custom setter for an atomic, readwrite property but leave the compiler to synthesize the getter.

You can use the nonatomic property attribute to specify that synthesized accessors simply set or return a value directly, with no guarantees about what happens if that same value is accessed simultaneously from different threads. For this reason, it’s faster to access a nonatomic property than an atomic one, and it’s fine to combine a synthesized setter, for example, with your own getter implementation.

## Naming convention for accessor methods {#Naming-convention-for-accessor-methods}
Because of the importance of this pattern, Cocoa defines some conventions for naming accessor methods. Given a property of type type and called name, you should typically implement accessor methods with the following form:

	- (type)name;
	- (void)setName:(type)newName;


The one exception is a property that is a Boolean value. Here the getter method name may be isName. For example:


	- (BOOL)isHidden;
	- (void)setHidden:(BOOL)newHidden;


> This naming convention is important because **much other functionality in Cocoa relies upon it**, in particular **key-value coding**. Cocoa does not use _getName_ because methods that start with "get" in Cocoa indicate that the method will return values by reference.

## Instance variable and Property {#Instance-variable-and-Property}
By default, a _readwrite_ property will be backed by an instance variable, which will again be synthesized automatically by the compiler.

An instance variable is a variable that exists and holds its value for the life of the object. The memory used for instance variables is allocated when the object is first created (through alloc), and freed when the object is deallocated.

Unless you specify otherwise, the synthesized instance variable has the same name as the property, but with an underscore prefix. For a property called firstName, for example, the synthesized instance variable will be called \_firstName.

Although it's best practice for an object to access its own properties using accessor methods or dot syntax, it's possible to access the instance variable directly from any of the instance methods in a class implementation

Define: 


	@interface SomeClass : NSObject {
	    NSString *instanceVariable;
	}
	...
	@end


But, how about


	@interface SomeClass : NSObject {
	    NSString *instanceVariable;
	}
	@property (nonatomic, retain) NSString *instanceVariable;
	...
	@end


This case is for what purpose? An instance variable and a property are same name.

## ARC for Foundation framework but not for C-based or Core Foundation Framework {#ARC}
Like garbage collection, ARC ignores all objects created using _stdlib_ and Core Foundation APIs. It will not insert calls to _malloc()_ and _free()_, which are needed to create and dispose of a struct or union. Nor will it insert calls to _CFRetain()_ and _CFRelease()_ to retain or dispose of a CF object.

Make sure to supply these calls yourself. Take care to balance each call to _malloc()_ with a call to _free()_ and each call to _CFRetain()_ with _CFRelease()_. 

Also make sure to avoid calls to _free()_ or _CFRelease()_ twice on the same object.

[ARC Documentation](https://developer.apple.com/library/ios/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html)

## Dont use Introspection for Mutable Data {#Dont-use-introspection-for-Mutable-Data}

To determine whether it can change a received object, the receiver of a message must rely on the formal type of the return value. If it receives, for example, an array object typed as immutable, it should not attempt to mutate it. **It is not an acceptable programming practice to determine if an object is mutable based on its class membership**.For example:

	if ( [anArray isKindOfClass:[NSMutableArray class]] ) {
	    // add, remove objects from anArray
	}

For reasons related to implementation, what _isKindOfClass:_ returns in this case may not be accurate. But for reasons other than this, you should not make assumptions about whether an object is mutable based on class membership. Your decision should be guided solely by what the signature of the method vending the object says about its mutability. If you are not sure whether an object is mutable or immutable, assume it’s immutable.

A couple of examples might help clarify why this guideline is important:

- You read a property list from a file. When the Foundation framework processes the list, it notices that various subsets of the property list are identical, so it creates a set of objects that it shares among all those subsets. Afterward you look at the created property list objects and decide to mutate one subset. Suddenly, and without being aware of it, you’ve changed the tree in multiple places.

- You ask NSView for its subviews (with the subviews method) and it returns an object that is declared to be an NSArray but which could be an NSMutableArray internally. Then you pass that array to some other code that, through introspection, determines it to be mutable and changes it. By changing this array, the code is mutating internal data structures of the NSView class.

So don’t make an assumption about object mutability based on what introspection tells you about an object. Treat objects as mutable or not based on what you are handed at the API boundaries (that is, based on the return type). If you need to unambiguously mark an object as mutable or immutable when you pass it to clients, pass that information as a flag along with the object.

> Dont try _isKindOfClass:_ with Mutable Data Type because it is not reliable. 

## Keyed and Sequential Archivers {#archivers}
The Foundation framework provides two sets of classes for archiving and unarchiving networks of objects. They include methods for initiating the archiving and unarchiving processes and for encoding and decoding the instance data of your objects. Objects of these classes are sometimes referred to as archivers and unarchivers.

Keyed archivers and unarchivers (**NSKeyedArchiver** and **NSKeyedUnarchiver**). These objects use string keys as identifiers of the data to be encoded and decoded. They are the preferred objects for archiving and unarchiving objects, especially with new applications.SHOULD USE THIS WAY!

Sequential archivers and unarchivers (**NSArchiver** and **NSUnarchiver**). This _old-style_ archiver encodes object state in a certain order; the unarchiver expects to decode object state in the same order. Their intended use is for legacy code; new applications should use keyed archives instead.

In my case, I did not understand why a property holds the value of the other property next to after decoding. Finally, I found that I was using Sequential Archive. Therefore, the first time I encoded and wrote data to file was OK. But before load data from file, I changed order of properties. This reason made unexpected value for properties.

> In summary, DONT USE **NSKeyedArchiver** and **NSKeyedUnarchiver**, because they are not safe. Instead, USE **NSKeyedArchiver** and **NSKeyedUnarchiver**

### Bonus: Purpose of Archiving
#### Definition
Archiving is the process of converting a group of related objects to a form that can be stored or transferred between applications. The end result of archiving—an archive—is a stream of bytes that records the identity of objects, their encapsulated values, and their relationships with other objects. Unarchiving, the reverse process, takes an archive and reconstitutes an identical network of objects.

If objects of a class wants to be archived, the class must **adopt the NSCoding protocol** and implement the required methods for encoding and decoding objects.

#### Purpose
The main value of archiving is that it provides a generic way to make objects persistent. Instead of writing object data out in a special file format, applications frequently store their model objects in archives that they can write out as files. An application can also transfer a network of objects—commonly known as an object graph—to another application using archiving. Applications often do this for pasteboard operations such as copy and paste.

## Inline Method in C and ObjectiveC {#inline-method}
In almost cases, inline method is for tunning performance. [refer here](http://www.drdobbs.com/the-new-c-inline-functions/184401540)

## Key in NSDictionary {#key-nsdictionary}
Beware that objects for key in NSDictionary (or NSMutableDictionary) must be members of a class which implement **hash** and **isEqual** methods. In almost cases, we use NSString as key because its **hash** is based on its character and **isEqual** is built in. 

## Objects for Collections type {#object-collection-type}
Collection types like NSArray, NSDictionary only allows insert objects into them. That means we cannot put **int**, **BOOL** or **float**, **enum**, ... or **C struct** into these collection types. As a result, iOS provides two kinds of data wrappers for this reason: **NSNumber** and **NSValue**.

In short, **NSNumber** is for wrapping primitive data such as **int**, **BOOL**, **enum**. **NSValue** is for some types of data like **C-Struct**. 

