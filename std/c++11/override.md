---
layout: default
title: override specifier
parent: C++11
---
# override

When overriding virtual functions defined in a base class in the derived class,
the **`override`** specifier can be used to state this explicitly.

    struct Base {
        virtual void f(int) {}
    };
    struct Derived : Base {
        virtual void f(int) override { ... }	// Compiler checks that void f(int) exists in the base class
        virtual void g(int) override { ... }	// ERROR! void g(int) doesn't exist in base class
    };

This makes the intent clear, and allows the compiler to check for the correct behavior.
This avoids situations like the following that can happen accidentally,
especially with more complex parameter lists.

    struct Base {
        virtual void f(int) {}
    } ;
    struct Derived : Base {
        virtual void f(float) {} 	// This "hides" f(int) and creates a whole new function
    };

