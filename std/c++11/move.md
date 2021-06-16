---
layout: default
title: Move semantics
parent: C++11
grandparent: Standards
---
# move semantics

C++11 introduces the idea of *moving* a value from one object to another
as an optimized alternative to copying.
This can be done when the original object will not be needed after the move is made.

For example, to copy a string,
a new buffer has to be allocated in the new string
and the characters copied from the original string.
To *move* the string,
the pointer to the original character buffer can just be copied to the new string.
The original string is then invalid.

Moves are useful for dealing with temporary objects
that occur during computations, construction, and parameter passing.
Often the temporary value gets stored/copied in some other location
and then the temporary is discarded.
Moving the value from the temporary is more efficient.

Moves are enabled with a new `&&` type of reference argument
that only matches on literal or temporary type of parameters,
referred to as **rvalue**s.
In other words, rvalues are values that do not have a named storage space.
Variables, data members, etc that have a name and can be assigned to
are referred to as **lvalue**s.
Consider,

    // Two overloaded versions of a function, one taking
    // a normal reference, the other taking an rvalue ref.

    void f(const int& x) { cout << "f(int&)\n"; }
    void f(const int&& x) { cout << "f(int&&)\n"; }
    int getInt() { return 7; }
    const int& getRef() { static int x = 7; return x; }

    int main()
    {
        int x = 7;   // x is an lvalue, 7 is an rvalue
        f(x);        // Calls the int& version, x has a name and can be assigned
        f(7);        // Calls the int&& version
                     // Note that if there were no f(int&&) version defined,
                     // f(7) would cause a temporary to be passed to f(int&).

        f(x+x);      // Calls the int&& version, passing a temporary.
        f(getInt()); // Calls the int&& version, passing a temporary.
        f(getRef()); // Calls the int& version.
    }

## && is about *binding*

Notice that the `&&` specifier is about the type of value that can bind
to the variable,
it *is **not** part of the type of the variable itself*.

    int main()
    {
        int x = 7;     // x is an lvalue
        int& y = x;    // y is a reference assigned to x, also an lvalue
        int&& z = x;   // ERROR! z cannot bind to an lvalue
        int&& z = 7;   // ok, z is bound to an rvalue, but z is an lvalue

        f(z);          // Calls the int& version of f().
    }

This is most important, and sometimes confusing,
when following rvalue references through multiple function or template calls.

    void g(int&& val)  // g() can only be called with an rvalue
                       // val itself is an lvalue
    {
        f(val);        // Probably not what you want!
                       // this will call the int& version of f()
                       // rvalues are not in some sense "transitive"
    }

## std::move()

Sometimes, you may want to treat an lvalue as an rvalue.
For example, in the `g()` function above,
you know `val` holds an rvalue,
so calling the rvalue version of `f()` would probably be better.

To do this, `std::move` is provided to convert a value into an rvalue.
Though it looks like a function call,
it is better to think of `std::move` as a cast operator.

    void h(int&& val)       // h() is called with an rvalue
    {
        f(std::move(val));  // calls the int&& version of f()
    }

An example of this is the common modern idiom
of passing large, movable objects by value,
rather than by const reference as would have been done in older C++.
This allows taking advantage of the move constructor when passing the value.
(See TODO for a more complete rationale for this.)

Consider an object that is going to store a copy of some `LargeValue`
that is passed in the constructor (say, a Matrix or list of long string).
When `object` constructed with an rvalue,
the move constructor for the parameter will be called
to avoid copying the large value.
Then when storing the value in the object,
the move constructor can be used again to store the otherwise unused parameter.

    object::object(LargeValue v) :
        val{std::move(v)}             // std::move is required here because
                                      // v is an lvalue
    {
    }

## Move constructors, etc

Any complex object that needs to be *movable* needs to implement
a move constructor and move assignment operator.

    Object::Object(Object&& obj) {
        // implement move semantics here, such as copying pointers
        // to internal data structures.
    }
    Object& Object::operator=(Object&& rhs) { }  // also here

While the moved-from object is technically invalid after being moved from,
and shouldn't be used, it is a good idea to leave the object in a valid
but "empty" or default state.
This usually means setting pointers back to `nullptr`, etc.

Whenever a move constructor does not exist,
the copy constructor will be used.

TODO: The Rule of Three has been replaced by several possible variations,
such as the Rule of Five...

TODO: The exact rules for default/delete of move functions.

* If no copy written, default copy is provided.
* If copy is default, and no move, then copy is used for moves
* If copy is default, but move is deleted, moves are compile error.
* If both defaulted, both defaults are provided, but act as copies.

## Rvalue Functions

In some cases, you may want an object's member function
to behave differently based on whether the value is an lvalue or rvalue.
A function can be tied to only operate on lvalues or rvalues
with the `&` or `&&` suffix, respectively.

This is often done by providing overloaded versions to capture the
unique behavior.

    struct Object {
        void f() $B&#B {}     // Normal version of f(), called on lvalue objects
        void f() $B&&#B {}    // Version of f() called on rvalue objects
    }

    Object a;
    a.f();                // calls the normal f()
    Object().f()          // calls the rvalue f()&&
