---
layout: default
title: Endl
parent: Others
grand_parent: Topics
---
# endl

There is some debate about using `endl` vs manual newlines.

[Jason Turner/C++ Weekly](https://www.youtube.com/watch?v=GMqQOEZYVJQ&list=PLs3KjaCtOwSZ2tbuV1hx8Xz-rFZTan2J1&index=7)

Doing `endl` is the same as doing a `'\n'` and a `flush`. So obviously:

1. If the newline can be added to the previous string, do that.
2. If you don't care about flushes, don't use `endl`

done