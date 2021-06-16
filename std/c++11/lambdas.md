---
layout: default
title: Lambdas
parent: C++11
grandparent: Standards
---

# Lambdas
<div class="floatrightazure1pxgray">
<p>Effective Modern C++ has a good chapter on Lambdas.</p>
</div>

C++11 introduced *lambdas*,
which are functions defined locally inline in a function that can be called and passed around
as a first class object.

For example,

    int f() {
        auto sayIt = [](const char* str) { cout << str << endl; };

        sayIt("Hello");
        sayIt("World");
    }

In this example, an unnamed function is defined using
the lambda notation **`[]`** and then assigned
to the variable **`sayIt`**.
Then `sayIt` can be called like a function.
Note that the <em>type</em> of variable <code>sayIt</code> is not at all obvious,
so it has to be defined using `auto`.

Lambdas are objects that can be passed to functions, stored in variables and data members, etc.
They are equivalent to defining a functor (function object)
by defining a class with an **`operator()`** defined.

    using FPtr = std::function<void(const char*)>;

    void callFunc(FPtr func, const char* arg) {
        func(arg);
    }

    int main() {
        auto sayIt = [](const char* str) { cout << str << endl; };
        FPtr f = sayIt;

        callFunc(sayIt, "Hello");
        callFunc(f, "World");
    }

This example shows that lambdas can be assigned to other variables
and passed to functions expecting some sort of function object pointer.

If the lambda needs to return a value,
the type can be declared using the new-style return value declaration
with the type to the right of the argument list:

    int main() {
        auto addOne = [](int val) $B-> int#B { return val + 1; };
        auto x = 7;
        auto y = addOne(x);
    }

<h2>Motivation</h2>

<p>There are many interesting uses for lambdas.
&nbsp;One of the major motivating factors for adding them to the language is for using std algorithms,
where passing a lambda is much more convenient\
than defining a separate function or functor to be passed.</p>

    // Define a simple object that holds simple int.
    // In practice, imagine this could be something complex.
    struct MyObj {
        int x = 7;
        constexpr MyObj(int val) : x{val} {}
    };

    int main() {
        // Define an array of MyObj objects.
        std::array<MyObj, 5> arr = { 1, 3, 5, 2, 4 };

        // Sort them...
        std::sort(begin(arr), end(arr), COMPARISON??);

        // Print them...
        std::for_each(begin(arr), end(arr), HMMM??);
    }

The sort algorithm needs a less-than comparison function to use to do the sorting.
The question is, what should be passed as that third argument in this example?
&nbsp;If the MyObj class defines an&nbsp;<code>operator&lt;</code>,
the third argument is not needed, and things work well.</p>

    // Define a simple object that holds simple int.
    // In practice, imagine this could be something complex.
    struct MyObj {
        int x = 7;
        constexpr MyObj(int val) : x{val} {}
        bool operator<(MyObj const& other) const { return this->x < other.x; }
    };

    int main() {
        // Define an array of MyObj objects.
        std::array<MyObj, 5> arr = { 1, 3, 5, 2, 4 };

        // Sort based on defined operator<
        std::sort(begin(arr), end(arr));
    }

Unfortunately, many classes don't define an&nbsp;<code>operator&lt;</code>,
or there may be multiple ways to sort the objects.
In that case, we have to define a standalone function:

    bool XisLess(MyObj const& lhs, MyObj const& rhs) {
        return lhs.x < rhs.x;
    }

    // used as...
    std::sort(begin(arr), end(arr), &XisLess);

Or, you could do the same thing with a functor:

    struct XIsLess {
        bool operator()(MyObj const& lhs, MyObj const& rhs) {
            return lhs.x < rhs.x;
        }
    };

    std::sort(begin(arr), end(arr), XisLess{});

These examples show why people tended to avoid std algorithms
and fall back to hand-written loops instead.
Lambdas solve all this. For example, the original problem can be done like:

    struct MyObj {
        int x = 7;
        constexpr MyObj(int val) : x{val} {}
    };

    int main() {
        // Define an array of MyObj objects.
        std::array<MyObj, 5> arr = { 1, 3, 5, 2, 4 };

        // Sort them...
        std::sort(begin(arr), end(arr),
            [](MyObj const& a, MyObj const& b) ->bool { return a.x < b.x; });

        // Print them...
        std::for_each(begin(arr), end(arr),
            [](MyObj const& obj) { cout << obj.x << endl; });
    }

Of course, in this simplistic example, the two line&nbsp;<code>for_each</code> is not much simpler
than a for loop, but in practice there are many different algorithms
that may need many different small code sections to be used.
Lambdas make this much better.

<h2><span style="line-height: 1.25;">Closures</span></h2>
<p>A lambda can access variables from its environment,
    but since the lambda may be executed much later than the point where it is defined,
    the definition is said to &quot;capture&quot; the variables it is accessing.</p>
<p>For example:</p>

    int main() {
        auto x = 7;                                             // A local variable to be captured
        auto addX = [x] (int val) -> int { return val + x; };   // Lambda captures x with [x] and uses it.

        cout << addX(1) << endl;        // prints 8
    }

In the above example, <code>x</code>&nbsp;is captured by value,
    meaning that lambda captures the value of&nbsp;<code>x</code>
    at the time of definition and then always uses this value whenever called.

<p>The variable&nbsp;<code>x</code> can also be captured by reference
    by specifying&nbsp;<code>[&amp;x]</code>.
    In this case, a reference to x is captured so the value of x inside the lambda
    will be the value of x at the time the labda is executed.</p>
<p>The following example shows how the value depends on the capture by value or reference.</p>

    int main() {
        auto x = 7;                                                    // A local variable to be captured
        auto addXbyVal =  [x] (int val) -> int { return val + x; };    // Lambda captures x by value.
        auto addXbyRef = [&x] (int val) -> int { return val + x; };    // Lambda captures x by reference.

        auto show = [&](int val) {
            cout << "byVal=" << addXbyVal(val) << " byRef=" << addXbyRef(val) << endl;
        }

        show(1);     // prints "byVal=8 byRef=8"

        x = 9;

        show(1);     // prints "byVal=8 byRef=10"
    }

<p>The above example also shows the use of the general capture specification&nbsp;<code>[&amp;]</code>.
    This means to capture everything (or everything needed) from the environment by reference.
    The similar specification&nbsp;<code>[=]</code> means to capture everything by value.</p>
<p>So the two lambdas from above could have been defined as:</p>

    auto addXbyVal = [=] (int val) -> int { return val + x; };    // Lambda captures x by value.
    auto addXbyRef = [&] (int val) -> int { return val + x; };    // Lambda captures x by reference.

<p>Capturing variables by reference can, of course, be more efficient,
    but it also suffers from the same dangers as any references to local variables.
    If the lambda is stored or returned and used after the function has returned,
    the references will now refer to invalid memory and invoke undefined behavior.</p>
<h2>Capturing Member Variables</h2>
<p>Inside a member function, a lambda can access the data members of the object,
    but care must be taken because inside the lambda, only the&nbsp;<em>this</em> pointer is captured.</p>
<p>If the lambda is only used locally, there will not be problems,
    but if passed or stored externally, the captured this pointer might no longer be valid.</p>

    struct Thing {
        int val = 7;
        void f();
    };

    void prt(int v) { cout << v << endl; };

    void Thing::f() {
        auto x1 = []    { prt(val); };    // Doesn't compile, val not captured.
        auto x2 = [=]   { prt(val); };    // Works, but captures this
        auto x3 = [val] { prt(val); };    // Doesn't compile.

        auto v = val;
        auto x4 = [v]    { prt(v); };    // Workaround captures a local copy of val.
    }

<h2>Lambda Initializers</h2>
<p><strong>In C++14</strong>,&nbsp;<em>lambda initializers</em> were added.
    This allows a captured variable to be given a different name, like:</p>

    // Capture x into the lambda variable named 'myX' that is used inside.
    auto addXbyVal = [myX = x] (int val) -> int { return val + myX; };

<p>This is just cosmetic for local variables, but quite useful when capturing members of a class.
The&nbsp;<code>Thing</code> example in the previous section can be written as:</p>

    void Thing::f() {
        auto x5 = [v = val] { prt(v); };
    }

<p>Much cleaner.</p>

<h2>Generic Lambdas</h2>
<p>TODO</p>
<h2>More</h2>
<p>TODO</p>
