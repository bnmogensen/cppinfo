---
layout: default
title: Return type deduction
parent: C++14
grand_parent: Standards
---
# Return type deduction

C++14 adds the ability to make the return type for a function `auto`,
and let the compiler deduce the actual return type.

    $Bauto#B f() {
        return 7;
    }

In order for this to work,
the type deduced from every `return` statement in the function
must be consistent and unambiguous.

Why would you want to use this?

When the return type is verbose or difficult to express,
using `auto` is a convenience.
```cpp
    auto getFunction() {
        return []() { return 7; };
    }

    auto getIterator() {
        some::complex::type::iterator i = ...;
        return i;
    }
```
This applies in templates as well,
where `auto` return type can avoid uses of typedecl, etc.

Also,
just like in the declaration of variables,
using `auto` can make code more resilient to small types changes.

    int f() { return 7 }

    auto g() { return f(); }

In this example, `g()` does not have to change if the type of `f()` changes,
from `int` to `long` or `double` for instance.
This is a tradeoff, clearly,
that can be considered either good or bad
depending on the situation and your stylistic preferences.

## decltype(auto)

`auto` return types do not preserve constness or refness.
`decltype(auto)` does, and allows perfect forwarding, etc.
