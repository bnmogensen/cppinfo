---
layout: default
title: Delegating constructors
parent: C++11
grandparent: Standards
---
# Delegating constructors

When a class has multiple constructors,
duplicate code can be required in all the defined constructors.

    class Thing {
        int x;
        int y;
        int z;
        double d;
        bool valid;
    pubic:
        Thing() : x{0}, y{0}, z{0}, d{0.0}, valid{false} {}
        Thing(double val) : x{0}, y{0}, z{0}, d{val}, valid{false} {}
        Thing(int x, int y, int z, double d, bool v) :
            x{x}, y{y}, z{z}, d{d}, valid{v} {}
    }

Here the class has several data members,
all of which have to be initialized in every constructor.
One way to get around this is to define an `init()` function that does common work:

	init() { x=0; y=0; z=0; d=0.0; valid=false; }
	Thing() { init(); }
	Thing(double val) { init(); d=val; }
	Thing(int x, int y, int z, double d, bool v) :
		x{x}, y{y}, z{z}, d{d}, valid{v} {} // hmm, use init() here?

There are a few problems with this approach:
 1. you can't do the common initialization in the initializer list,
 2. the constructor bodies still have redundant calls to init(), and
 3. there are still various ways to do this incorrectly,
	like trying to make the init() function virtual.

C++11 lets one constructor delegate to another defined one:

	Thing() : Thing{0.0} {}
	Thing(double val) : Thing{0, 0, 0, val, false} {}
	Thing(int x, int y, int z, double d, bool v) :
		x{x}, y{y}, z{z}, d{d}, valid{v} { LOG("construct X..."); }

Note that when a constructor delegates,
its initializer list can only contain the other constructor call, not any other variables.
Also note that there can be a whole chain of delegations,
as Thing() delegates to Thing(double) which delegates to Thing(x,y,z,d,v).

The body of the delegated to constructor is called first,
and the calling constructor is called after that.
So behavior beyond just member initialization can be shared as well.

The need for delegating constructors can be reduced when using
[C++ inline class member initializers](TODO).
