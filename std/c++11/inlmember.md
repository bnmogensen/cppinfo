---
layout: default
title: Inline member initialization
parent: C++11
---
# Inline member initialization

In C++11, you can specify the default value that a data member should have
in the class definition.

    struct Thing {
        int x = 7;
        Thing() {}					// When default constructed, x will be 7
        Thing(int val) : x{val} {}	// This constructor sets x directly
    }

The initializer is equivalent to adding that value to the initializer list for all constructors
that don't specify the value themselves.

Doing this can make the class easier to understand and
is less duplication when multiple constructors are defined.

Initializers can also use the **uniform initialization** syntax;

    struct Thing {
        int x{7};       // uniform initialization works
        int y(8);       // ERROR! trying to call constructor with parens doesn't compile
    }
