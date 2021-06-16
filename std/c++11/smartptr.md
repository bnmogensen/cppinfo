---
layout: default
title: Smart pointers
parent: C++11
---
# Smart pointers

C++11 adds better smart pointers that should be used wherever a pointer is stored
	to get correct memory management.
	Basically, if you are ever calling new or delete directly, you are probably doing it wrong. 

A smart pointer expresses *ownership* so whenever an object is constructed,
	it must have a clear owner that is responsible for deleting the object.
	There are three basic types of smart pointers:

* `std::unique_ptr<>` defines a pointer with a single owner.
	When the unique_ptr goes away, the object pointed to is deleted.
	unique_ptrs work inside objects and containers.  

* `std::shared_ptr<>` defines a reference counted pointer where ownership is shared
	between all the shared_ptrs.
	When the last shared_ptr goes away, the object is deleted.
	Since shared_ptrs are more expensive than unique_ptrs,
	it is good to consider whether one owner can be made the unique owner
	and other users can use non-owning raw pointers or weak_ptrs.

* `std::weak_ptr<>` defines a pointer that observes an object owned elsewhere,
	so it may go away.
	The weak_ptr defines special functions to check pointer validity
	and to grab temporary ownership.  

When simply using a pointer without participating in ownership,
	a raw pointer can often be used. Observing means not newing or deleting,
	and the lifetime of the object is guaranteed to be such that the object
	will not be deleted while being used. 
	
