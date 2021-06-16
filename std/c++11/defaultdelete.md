---
layout: default
title: default/delete functions
parent: C++11
grand_parent: Standards
---

# default/delete
C++11 allows special class functions to be marked
as *deleted* or *default*.
This replaces previous idioms such as declaring the functions private
and providing no implementation.

    class Object {
        Object() = default;                           // Use the compiler-provided implementation of the no-arg constructor
        Object(const Object&) = delete;               // Mark the copy constructor deleted, so it can't be used or called

        Object& operator=(const Object&) = delete;    // Works for assignment operators as well
    }

This syntax makes the intent more clear and allows the compiler to do better checks
and give better error messages.

    struct X
    {
        int val = 7;

        X() = delete;
        X(X const&) = delete;

        X(int v) : val{v} {}
    };

    int main()
    {
        X x{13};
        X x2(x);
    }

This gives the error:

    $ g++ -o t t.cpp
    t.cpp: In function int main():
    t.cpp:14:11: error: use of deleted function X::X(const X&)
        X x2(x);
            ^
    t.cpp:6:5: note: declared here
        X(X const&) = delete;

# default/delete of copy/move constructors and operators

TODO