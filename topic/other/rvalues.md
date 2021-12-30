# rvalues

TODO:

- [cppreference value categories](https://en.cppreference.com/w/cpp/language/value_category)
- [n3055](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2010/n3055.pdf)
- [medium article](https://medium.com/@barryrevzin/value-categories-in-c-17-f56ae54bccbe)
- [P0135: guaranteed elision](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0135r1.html)
- [Sutter's most important const](https://herbsutter.com/2008/01/01/gotw-88-a-candidate-for-the-most-important-const/)

Originally C++ distinguished between `lvaues` and `rvalues`.
`lvalues` are values that have a location and generally a name.
`rvalues` are temporaries
The names come from the idea that `lvalues` can appear on the left
side of an assignment,
but `rvalues` can only appear on the right side.
In practice, things are more complicated than that.

    void f(int& i)
    {
    }
    int main()
    {
        int x = 7;   // x is an lvalue, 7 is an rvalue
        $Bf(x);#B      // f is called passing an lvalue
        f(8);        // f is called passing an rvalue
    }

## Why rvalues?

There are many situations in expressions or passing arguments
to functions where temporaries are created and copies made.
The compiler can sometimes optimize away these extra operations,
but in other cases more information is needed.

Consider operations on an object large enough to be expensive to copy,
like a Matrix.

    Matrix a, b, c, $Bm#B;
    m = a + b + c;

To evaluate `a + b + c`, first `a + b` is computed by `operator+`
by creating and returning a temporary `t1` holding the first sum.
Then `t1 + c` is computed returning another temporary `t2`.
Finally `m` is assigned the value of `t2`.
