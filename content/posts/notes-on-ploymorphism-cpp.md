---
title: "Notes on Inheritance & Polymorphism in C++"
date: 2019-03-12T22:41:31Z
draft: false
tags: ["c++"]
---
These are some of my notes on trying to understand Inheritance and Polymorphism in C++.
<!--more-->

As is standard in Object Orientated languages, C++ supports inheritance.  In the example below, Bar extends Foo (to steal some Java speak).

{{< gist t0mmyt 45416ee511bc8988e48990673793791e "inheritance.cpp" >}}

For classes that want members to be overridden in a *Polymorphic* way, those members should be declared as *virtual*.  This means that the correct method will be determined at runtime (this does come with a slight, albeit minor, performance penalty).  For classes that are intended to be *abstract*, there is the concept of a *pure virtual* method.  A pure virtual method must be overridden and this is achieved by assigning the virtual method to zero.  It is also convention (since C++11) to identify the overriding methods with the keyword *override*.  The destructor can also be declared purely virtual in order to make the class abstract but then an implementation must also be provided (this is because the destructor will still be called, see below).

In the following example, Foo cannot be directly instantiated.

{{< gist t0mmyt 45416ee511bc8988e48990673793791e "abstract.cpp" >}}

Destructors of the *concrete* object and all parent classes are called when the object is deleted, starting at the child (see example at the bottom).

If objects are stored in a container, their destructors are called automatically when that container goes out of scope. However due to the way memory is allocated for the objects, storing a parent type will cause that object to be *sliced* at that level.  Continuing with Foo and Bar, storing a Bar in a container of type Foo would result in all Bars being stored as Foos.

The way to solve this is to store Foo *pointers* in the container instead, this however means that destructors are not automatically called when the container goes out of scope.  This will result in the memory allocated to those objects not being freed up (i.e. a memory leak).  Two options to avoid this are to either explicitly call destructors on each of the objects or to use a *special pointer* such as *uinque_ptr* or *shared_ptr*

A *unique_ptr* will call the destructors and free the memory allocated to it's object when it leaves scope, a *shared_ptr* keeps a count of references to that object and will call the destructors when the last reference is removed.  A shared pointer is useful for example when returning the object or storing references in multiple places.  Storing an object in a unique or shared pointer effectively results in a rudimentary garbage collector pattern.

The following is an example of all of the above and the output.

{{< gist t0mmyt 45416ee511bc8988e48990673793791e "engine.cpp" >}}
{{< gist t0mmyt 45416ee511bc8988e48990673793791e "engine_output.txt" >}}
