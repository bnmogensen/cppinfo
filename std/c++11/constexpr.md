---
layout: default
title: constexpr
parent: C++11
grandparent: Standards
---
# constexpr

C++11 added the `constexpr` keyword for generalized constant expressions to increase the number of things you can do at compile time.

C++14 extended the range of things allowed in a constexpr function.

## constexpr vs const

`constexpr` and `const` mean different things, though in many contexts they can overlap and the difference can be a bit confusing.

For example, int these two statements,

    const int x = 7;
    constexptr int y = 13;

Both variables define constants, but `constexpr` adds the additional requirement
that the value be computable at compile time.
A `const` variable can not be changed after it is initially set,
but may be initialized at run time.

Consider,

    int f() {
        ...
    }
    const int x = f();          // ok, f() is computed at runtime
    constexptr int y = f();     // error, f() must be constexpr

Making `f()` `constexpr` makes it usable for `y`:

    constexpr int f() {
        return 7;
    }
    const int x = f();          // ok, f() is computed at runtime
    constexptr int y = f();     // ok, f() is now constexpr

## constexpr functions

`constexpr` functions allow computations to be performed at compile time.
In C++11, the rules for what expressions are legal in a constexpr functions
are fairly restrictive.
In C++14 and C++17, the available functionality has been expanded.

## C++11 constexpr functions

In C++11, the body of the function is restrivted to basically a single return statement
with a constant expression.

TODO:

Links:

* [Simplify C++ blog](http://arne-mertz.de/2016/06/constexpr/)
* [N2335 constexpr ver 5](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2007/n2235.pdf)
* n3268 added static asseert, using, typedef to the single return value
* n3444 (2012) proposed more general functions with specific things disallowed, so if allowed.
* n3597 (2013) is an improved n3444
* n3598 (2013) are constexpr member functions const?
* n3615 (2013) for constexpr variable templates
* n4487 (2015) constexpr lambdas
* P0292R0 (2016) if constexpr(cond)
* P0596R0 P0597 P0598 (2017) proposed constexpr() operator, trace, assert, and vector
* 