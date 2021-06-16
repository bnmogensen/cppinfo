---
layout: default
title: final
parent: C++11
grand_parent: Standards
---
# final

C++11 adds the `final` specifier for classes and virtual functions.

When a class is marked `final`, it can no longer be derived from.
When a function is marked  `final`,
it can no longer be overridden in a derived class.

    class Base {
        $Bfinal#B virtual void f() {}
    }
    $Bfinal#B class Derived : public Base {
        virtual void f() override {}     // Error! function f() is final
    }
    class More : public Derived {}       // Error! class Derived is final


`final` allows you to make the design intent clear,
and avoid clients doing things that may violate that intent.
However, some people argue that final should be used rarely,
since there is no reason to preclude users from doing things that you may not have foreseen.

One important advantage of `final` is that
the compiler can optimize the code to avoid a virtual call where it knows
the actual function being called since it knows the final type of the object.

    class Base { virtual void f() {} };
    final class Derived {
        virtual void f() override {}
    };

    void doSomething(const Derived& obj)
    {
        obj.f();        // Compiler knows the obj is a Derived, and can't be a class
                        // further derived from Derived that may override f(). So,
                        // it knows that it can call Derived::f() directly.
    }

## final patterns

