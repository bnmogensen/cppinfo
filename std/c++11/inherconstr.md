---
layout: default
title: Inherited constructors
parent: C++11
---
## Inherited constructors

Consider the following base class:

    class Base {
        int m_x, m_y, m_z;
    public:
        Base(int x, int y, int z) : m_x{x}, m_y{y}, m_z{z} {}
        virtual void f() { ... }
    };

Now you want to implement a derived class that overrides the definition of `f()`.

    class Derived : public Base {
    public:
        virtual void f() { ... }
    };

Without defining a constructor,
no object of type Derived can be created
because the Base requires being constructed with x, y, and z.
So, you have to put in a Derived constructor that is mostly boilerplate:

    class Derived : public Base {
    public:
        Derived(int x, int y, int z) : Base{x, y, z} {}
        virtual void f() { ... }
    };

Creating a Derived now calls the Derived constructor that just forwards to the Base constructor.
This is a minor annoyance,
especially when there are multiple derived classes and the arguments are more complicated,
like requiring template type names.
Also, if the Base constructor is changed, all the derived constructors will have to be updated,
even though they don't really care about construction.

C++11 allows you to "pull in" the base class constructors in a derived class
so they don't have to be redefined.
This is done with the `using` directive.

    class Derived : public Base {
    public:
        using Base::Base;			// This pulls in all the Base constructors
        virtual void f() { ... }
    };

If class Derived defines any new data members,
then it is most likely not a good idea to inherit constructors
unless the new data is initialized in-line.
