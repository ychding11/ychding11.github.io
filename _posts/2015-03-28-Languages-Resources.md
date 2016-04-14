---
layout: post
title: "C++ Features and Resources"
date: 2015-03-28
---

## overloaded virtual function
[Discussion of GCC -Woverloaded-virtual option](https://gcc.gnu.org/ml/gcc/1999-02n/msg00180.html) 
Compiler is unable to tell "I am creating a new virtual function" from
"I am overriding a virtual function from my baseclass". So GCC adds this option 
to detect potential errors. But I think the option is not a good idea, especially
when c++11 introduce a new key word *override* to explicitly tell compiler to 
override existing virtual function or create new virtual function. For quick
reference, I list some frequently used GCC compile options here. More compile
options please read gcc manual. 

- Werror, Treat all warnings as errors.
- Wall, 
- Wextra,
- Wl,option, pass options to linker, *option* is linker's 
option.

## const reference
Under what condition is this keyword usefull? What problems will it cause?
When user-defined complex type used as an input parameter, it is a good choice.
It is highly recommanded in google's c++ code style.     
But used as a data holder is a very bad idea. It almost have no flexibility.
For example, following design is impossible for reference type. But sometimes 
it is a very efficient design. In this case, const pointer type is good choice.

```
struct Holder
{
	int type;
	union
	{
		int &ri;
		float &rf;
	};
};
```



## Learning materials
- [Learn CPP websit](http://www.learncpp.com/) It provides very comprehensive contents about c++, very good materials for references.
- [C and C++ API Reference](http://www.cplusplus.com/) Very good websit for C++ STL API lookup, lots of usage demoes facilitate beginners.
- [A simple inroduction to C++ Object Memory layout](http://lifegoo.pluskid.org/upload/doc/object_models/C++%20Object%20Model.pdf)
- [Paper about C++ multiple inheritance object layout](http://lifegoo.pluskid.org/upload/doc/object_models/C++%20Object%20Model.pdf) 
A little difficult to understand.

