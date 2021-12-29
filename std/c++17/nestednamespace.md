---
layout: default
title: Nested Namespaces
parent: C++17
grand_parent: Standards
---
# Nested Namespaces

C++17 added a convenience syntax for nested namespaces.
Instead of saying:

    namespace One {
        namespace Two {
            class X {}
        }
    }

It is now possible to say:

    namespace One::Two {
        class X {}
    }

This is a small, but nice, change that makes the management
of large projects with many namespaces cleaner.
