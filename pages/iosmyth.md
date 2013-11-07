---
layout: page
title: "iOS Myth"
description: ""
---
{% include JB/setup %}

## How an object is created?

When you allocate an object, part of what happens is what you might expect, given the term. Cocoa allocates enough memory for the object from a region of application virtual memory. To calculate how much memory to allocate, it takes the object’s instance variables into account—including their types and order—as specified by the object’s class.

To allocate an object, you send the message alloc or allocWithZone: to the object’s class. In return, you get a “raw” (uninitialized) instance of the class. The alloc variant of the method uses the application’s default zone. A zone is a page-aligned area of memory for holding related objects and data allocated by an application. See Advanced Memory Management Programming Guide for more information on zones.

An allocation message does other important things besides allocating memory:

	- It sets the object’s retain count to one.
	- It initializes the object’s isa instance variable to point to the object’s class, a runtime object in its own right that is compiled from the class definition.
	- It initializes all other instance variables to zero (or to the equivalent type for zero, such as nil, NULL, and 0.0).

An object’s isa instance variable is inherited from NSObject, so it is common to all Cocoa objects. After allocation sets isa to the object’s class, the object is integrated into the runtime’s view of the inheritance hierarchy and the current network of objects (class and instance) that constitute a program. Consequently an object can find whatever information it needs at runtime, such as another object’s place in the inheritance hierarchy, the protocols that other objects conform to, and the location of the method implementations it can perform in response to messages.

In summary, allocation not only allocates memory for an object but initializes two small but very important attributes of any object: its isa instance variable and its retain count. It also sets all remaining instance variables to zero. But the resulting object is not yet usable.Initializing methods such as init must yet initialize objects with their particular characteristics and return a functional object.

> ALLOC: Do three tasks but object is not usable. INIT will make allocated object be usable. 

## How many introspection methods?
- [objectX class]
- [objectX superclass]
- [objectX isKindOfClass]
- [objectX isMemberOfClass]
- [objectX respondsToSelector:(SEL)selector]
- [objectX conformsToProtocol:@protocol(protocolName)]
- [objectX isEqual:objectY]

## isEqual and Hash
The hash and isEqual: methods, both declared by the NSObject protocol, are closely related. The hash method must be implemented to return an integer that can be used as a table address in a hash table structure. If two objects are equal (as determined by the isEqual: method), they must have the same hash value. If your object could be included in collections such as NSSet objects, you need to define hash and verify the invariant that if two objects are equal, they return the same hash value. The default NSObject implementation of isEqual: simply checks for pointer equality.

> If two objects are equal, their hashes must be equal too. But the same hash does not need to be equal. The following example will explain this case.

	If two objects are equal (as determined by the isEqual: method), they must have the same hash value. This last point is particularly important if you define hash in a subclass and intend to put instances of that subclass into a collection.

	If a mutable object is added to a collection that uses hash values to determine the object’s position in the collection, the value returned by the hash method of the object must not change while the object is in the collection. Therefore, either the hash method must not rely on any of the object’s internal state information or you must make sure the object’s internal state information does not change while the object is in the collection. Thus, for example, a mutable dictionary can be put in a hash table but you must not change it while it is in there. (Note that it can be difficult to know whether or not a given object is in a collection.)

Note that in all isEqualToType: methods of the Cocoa frameworks, nil is not a valid parameter and implementations of these methods may raise an exception upon receiving a nil. 
However, for backward compatibility, isEqual: methods of the Cocoa frameworks do accept nil, returning NO. In summary, _isEqual accepts nil object but isEqualToType is not.

## 



