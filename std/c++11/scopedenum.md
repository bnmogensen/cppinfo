---
layout: default
title: Scoped enums
parent: C++11
grandparent: Standards
---
# scoped enums

C++11 introduced the `enum class` syntax
for *scoped* or *strongly-typed* enums.
This is an improvement over regular enums in a number of ways.

A scoped enum can be defined similarly to a regular enum.

    $Benum class#B ThreadState {
        Running,
        Stopping,
        Stopped,
        Shutdown
    };

However,
(1) the enum names are not in the surrounding namespace,
so they must be fully qualified (like `ThreadState::Running`) when used.

    void f()
    {
        T t1 = Running;                  // ERROR: 'Running' not declared
        T t2 = ThreadState::Running;     // OK
        T t3{ThreadState::Running};      // OK, same
        t3 = t2;                         // OK
        t3 = Stopped;                    // ERROR
        t3 = ThreadState::Stopped;       // OK
        setState(ThreadState::Stopping); // OK
    }

Also, (2) scoped enums don't convert between ints and other types,
so accidental misuse is much harder.

    enum THR_STATE { RUNNING, STOPPING, STOPPED };            // old style
    $Benum class#B ThreadState { Running, Stopping, Stopped };    // new style

    void g(int) { }

    void f()
    {
        THR_STATE s1 = RUNNING;                 // OK
        ThreadState t1 = ThreadState::Running;  // OK
        ThreadState t2 = 7;                     // ERROR
        ThreadState t3 = int{7};                // OK in C++17, ERROR before that
        int i1 = s1;                            // OK
        int i2 = t1;                            // ERROR: cannot convert 'T' to 'int'
        int i3 = int(t1);                       // OK, explicit conversion
        int i4 = static_cast<int>(t1);          // OK, same, explicit cast
        int i5 = int{t1};                       // ERROR: uniform/brace initialization

        g(1);                                   // OK
        g(RUNNING);                             // OK, but does it make sense?
        g(ThreadState::Running);                // ERROR
        g(17 + RUNNING);                        // OK, but highly suspicious
        g(int(ThreadState::Running));           // OK, makes clear this is done on purpose
    }

The underlying type used to store old-style enums is left up to the compiler.
In most cases the type will be `int`,
but the compiler is allowed to use a `short` or `unsigned char`
if all the values fit within those types.</p>

With the new strongly-typed enums,
(3) the underlying type can be specified as some form of integral type.
The underlying type is made available through a std template.

    enum SmallerEnum : std::uint16_t { a, b, c};
    enum BiggerEnum : long long { big, value };
    enum Vowel : char { a='a', e='e', i='e', o='o', u='u' };

    using VowelType = std::underlying_type<Vowel>::type;    // in C++11
    using VowelType = std::underlying_type_t<Vowel>;        // helper added in C++14
