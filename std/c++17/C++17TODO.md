---
layout: default
title: C++17 Todo
parent: C++17
grand_parent: Standards
---
# TODO

- [Stack overflow answer](http://stackoverflow.com/questions/38060436/what-are-the-new-features-in-c17)
- [Herb Sutter trip report](https://herbsutter.com/2016/06/30/trip-report-summer-iso-c-standards-meeting-oulu/)
- [ISO list](https://isocpp.org/files/papers/p0636r0.html)

- std::optional
- std::any
- string_view
- lambda *this
- structured bindings
- if-init switch-init
- if constexpr
- inline variables
- constexpr lambdas
- guaranteed copy elision
- File System
- folding expressions
- has_include

- std::any (C++Weekly)
- std::invoke [C++Weekly ep17](https://www.youtube.com/watch?v=z-kUhwANrIw&list=PLs3KjaCtOwSZ2tbuV1hx8Xz-rFZTan2J1&index=17)


Template argument deduction for class templates
Like how functions deduce template arguments, now constructors can deduce the template arguments of the class
http://wg21.link/p0433r2 http://wg21.link/p0620r0 http://wg21.link/p0512r0
template <auto>
Represents a value of any (non-type template argument) type.
Non-type template arguments fixes
template<template<class...>typename bob> struct foo {}
( Folding + ... + expressions ) and Revisions
auto x{8}; is an int
modernizing using with ... and lists
Lambda

constexpr lambdas
Lambdas are implicitly constexpr if they qualify
Capturing *this in lambdas
[*this]{ std::cout << could << " be " << useful << '\n'; }
Attributes

[[fallthrough]], [[nodiscard]], [[maybe_unused]] attributes
[[attributes]] on namespaces and enum { erator[[s]] }
using in attributes to avoid having to repeat an attribute namespace.
Compilers are now required to ignore non-standard attributes they don't recognize.
The C++14 wording allowed compilers to reject unknown scoped attributes.
Syntax cleanup

Inline variables
Like inline functions
Compiler picks where the instance is instantiated
Deprecate static constexpr redeclaration, now implicitly inline.
namespace A::B
Simple static_assert(expression); with no string
no throw unless throw(), and throw() is noexcept(true).
Cleaner multi-return and flow control

Structured bindings
Basically, first-class std::tie with auto
Example:
const auto [it, inserted] = map.insert( {"foo", bar} );
Creates variables it and inserted with deduced type from the pair that map::insert returns.
Works with tuple/pair-likes & std::arrays and relatively flat structs
Actually named structured bindings in standard
if (init; condition) and switch (init; condition)
if (const auto [it, inserted] = map.insert( {"foo", bar} ); inserted)
Extends the if(decl) to cases where decl isn't convertible-to-bool sensibly.
Generalizing range-based for loops
Appears to be mostly support for sentinels, or end iterators that are not the same type as begin iterators, which helps with null-terminated loops and the like.
if constexpr
Much requested feature to simplify almost-generic code.
Misc

Hexadecimal float point literals
Dynamic memory allocation for over-aligned data
Guaranteed copy elision
Finally!
Not in all cases, but distinguishes syntax where you are "just creating something" that was called elision, from "genuine elision".
Fixed order-of-evaluation for (some) expressions with some modifications
Not including function arguments, but function argument evaluation interleaving now banned
Makes a bunch of broken code work mostly, and makes .then on future work.
Direct list-initialization of enums
Forward progress guarantees (FPG) (also, FPGs for parallel algorithms)
I think this is saying "the implementation may not stall threads forever"?
u8'U', u8'T', u8'F', u8'8' character literals (string already existed)
"noexcept" in the type system
__has_include
Test if a header file include would be an error
makes migrating from experimental to std almost seamless
Arrays of pointer conversion fixes
inherited constructors fixes to some corner cases (see P0136R0 for examples of behavior changes)
aggregate initialization with inheritance.
std::launder, type punning, etc