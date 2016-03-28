---
layout: post
title: "C++ Features and Resources"
date: 2015-03-28
---

## Overloaded virtual function
[Discussion of GCC -Woverloaded-virtual option](https://gcc.gnu.org/ml/gcc/1999-02n/msg00180.html)  Compiler is unable to tell 
"I am creating a new virtual function" from "I am overriding a virtual function from my baseclass". So GCC implements this option 
to detect potential errors. But I think the option is not a good idea, especially when c++11 introduce a new key word override to solve
override existing virtual function or create new virtual function.  

## Learning materials
- [Learn CPP websit](http://www.learncpp.com/) It provides very comprehensive contents about c++, very good materials for references.
- [C and C++ API Reference](http://www.cplusplus.com/) Very good websit for C++ STL API lookup, lots of usage demoes facilitate beginners.

