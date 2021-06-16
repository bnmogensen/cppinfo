---
layout: default
title: Range-based for loops
parent: C++11
---
# Range-based for loops

Range-based for loops is a new syntax for `for` loops introduced in C++11.
It is a much more convenient way to iterate over a container or range of values.

    int f(vector<int>& v)
    {
        // Each time through the loop x is set to the next
        // int element of the vector.
        for (int x : v)
        {
            cout << "x = " << x << endl;
        }
    }

    // The for loop is roughly equivalent to..
    for (vector<int>::iterator iter = begin(v); iter != end(v); ++iter)
    {
        int x = *iter;
        cout << "x = " << x << endl;
    }

The type of the loop variable can be declared:

* as a reference to avoid making a copy of the element in the range,
* `auto` as a convenience for declaring the type,
* `const` when the value is const within the loop (like using a `const_iterator`).
  If the container is `const` the variable will be `const` by default,
  but it may be clearer to still declare the loop variable const.

An example:

    int f(list<BigObject>& l)
    {
        for (const auto& x : l)
        {
            // x is a "const BigObject&"
        }
    }

Range for loops work for built in arrays and initializer lists as well:

    int f()
    {
        int arr[] = { 1, 2, 3 };
        for (auto x : arr) { ... }
        for (auto x : { 1, 2, 3 }) { ... }
    }
