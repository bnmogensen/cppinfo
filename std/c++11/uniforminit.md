---
layout: default
title: Uniform initialization
parent: C++11
grandparent: Standards
---
# Uniform initialization

C++11 introduces a new initialization syntax using braces "{}".
The intent was to provide more powerful initialization for arrays and objects
when initializing with brace specifiers,
and to solve some syntax gotchas like the "most vexing parse".

    Object obj();       // obj is not a variable of type Object,
                        // but a declaration of a function named obj()

Uniform initialization can be used wherever an object needs to be initialized:

    int f()
    {
        int i{7};        // initialize integer i to 7
        int i = int{7}   // likewise,
        auto i = int{7}  // likewise

        Object obj{};    // variable obj default constructed
        Object* obj = new Object{x, y, z};

        int arr[4] = { 1, 2, 3, 4 };     // Initialize array of values
        vector<int> v = {1, 2, 3, 4};    // Works for vectors now, too
    }

    class Object {
        int m_x = 7;
        int m_y{8};      // uniform initialization works for member initialization
        int m_z{9};      // m_z(9), like a contructor call, does NOT work
    }

    Object::Object(x, y, z) :
        m_x{x},            // constructor initializers
        m_y{y},
        m_z{z}
    {
    }

<todo>
	Issues with initializer list..
</todo>
