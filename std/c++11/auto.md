---
layout: default
title: Auto
parent: C++11
grand_parent: Standards
---

# Overview

C++11 allows variables to be declared as type `auto`,
meaning that the type will be deduced from the context.
In common cases, the deduced type is obvious enough that there will be no confusion.

    int main()
    {
        auto i = 0;                 // i is int
        auto c = "str";             // c is const char*
        auto s = string("str");     // s is string
        auto n = new Object();      // n is Object*
        auto f = someFunc();        // f is whatever type someFunc() returns
    }

## Const, References, Pointers

In dealing with references and pointers and cv-qualifiers (const, volatile),
the rules need to be remembered. (See Herb Sutter's auto gotw 
[part 1](https://herbsutter.com/2013/06/07/gotw-92-solution-auto-variables-part-1/))

Examples

    int main()
    {
        int i;
        auto ii = i;      // ii is int, a copy of i
        auto& ir = i;     // ir is a reference to i
        auto jr = ir;     // jr is int, not int&

        const int ci = i;
        auto ai = ci;        // ai is int, not const because it is a copy
        const auto cai = ci; // cai is const int

        int* ip = &i;
        auto ap = ip;        // ap is int*

        const char* str = "text";
        auto astr = str;     // astr is const char*
        const char* const cstr = "text";
        auto $Bacstr#B = cstr;     // acstr is still const char*
                                 // const is preserved on the thing pointed to,
                                 // but not on the pointer.
    }

## AAA - Almost Always Auto style

Herb Sutter has been proponent of what he calls AAA or Almost Always Auto style.
For why, see his
[gotw93 part 2](https://herbsutter.com/2013/06/13/gotw-93-solution-auto-variables-part-2/)
and [AAA](https://herbsutter.com/2013/08/12/gotw-94-solution-aaa-style-almost-always-auto/).
AAA style means to almost always declare variables using the form:

    auto x = expr

    auto x = 7;                 // int, simple cases deduce as expected
    auto widget = Widget{7};    // object name on the right is still clear and explicit
    auto rv = func(7);          // rv is whatever func() returns, no code change for
                                // minor changes like int->long, const->non-const, etc
    auto dx = double{x};        // make change from int to double explicit
