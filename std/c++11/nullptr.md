---
layout: default
title: nullptr
parent: C++11
grandparent: Standards
---
# nullptr

Forever there has been a debate in C++ about how to define a null pointer, as NULL or as 0.

In most cases, either works fine, and in most modern systems NULL is simply `#define`d to `0`.
This hasn't always been the case,
so some people have preferred 0 to NULL for simplicity and portability.
Other people prefer NULL because it better documents the intent of the code.

The problem with 0 or NULL is that 0 can mean an integer 0 or a null pointer,
and in some contexts this can lead to conflicting or unexpected behavior.

    int f(int x) { }
    int f(char* x) {}

    int someFunc()
    {
        f(0); // which f() does this call?
    }

This issue is more pronounced with the widespread use of templates and type deduction.
The need for a formal, type-correct way of representing a null pointer has long been recognized,
and has been addressed in C++11 with the introduction
of the&nbsp;<code>nullptr</code> keyword and&nbsp;<code>nullptr_t</code> type.</p>
