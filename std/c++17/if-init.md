---
layout: default
title: If-init statements
parent: C++17
grand_parent: Standards
---
# if-init and switch-init statements

Consider a situation like:

    auto f() {
        int x = getValue();
        if (x == 0) {
            doSomething();
        }

        moreCode();
    }

In this case, the variable `x` is only needed for the `if` statement.
C++17 adds the ability to declare and initialize the variable
in the conditional statement.

    auto f() {
        if ($Bint x = getValue();#B x == 0) {   // Note the semicolon
            doSomething();
        }

        moreCode();
    }

This is not only a nice convenience,
but it also keeps the variable `x` scoped to just the `if` statement,
as it should be.

This syntax is also available for `switch` statements.

    switch(int x = getValue(); x) {
        case 0: breakn;
        ...
    }

Standard `for` loops have always had a similar capability:

    for (int x=0; x<10; ++x) { ... }

but C++20 adds the ability to range-based `for`-loops.

    for (int count=0; item : itemList) { ... }
