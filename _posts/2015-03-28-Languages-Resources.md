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

## using statement

using keyword can introduce base class's member into derived class.
For example expose base class's protected member to public in drived 
class.     `using base::member` if member is name of overloaded function 
in base class, then all functions with the same name will be introduced 
into derived class. The point is very important.     In such case, if 
derived class already have a member with same name, parameter list and 
qualifications, then derived class member will hiden or override the 
newly introduced ones, but they won't conflict with each other.
[The web link](http://en.cppreference.com/w/cpp/language/using_declaration)
gives the detailed info.

## vtbl & vptr
where is vtable stored? Is this a specification or implementation dependent?
vptr is always in offset 0 in a object memory layout?

## type casting

### const_cast
Const type cast is used to add or remove the constness of an object pointed
by a pointer. For example `const_cast< const type*>(pointer)` or 
`const_cast<type*>(pointer)`. The latter one is always used to match interface.
*When using, be caution, write operation will cause undefined behavior.*

### static_cast & dynamic_cast
These two type cast can do pointer upcast and downcast. Upcast is converting
a pointer-to-derived to a pointer-to-base. Downcast is converting a 
pointer-to-base to a pointer-to-derived. static_cast does it without runtime 
checking. `Base *pb = new Base(); static_cast<Derived*>(pb);` is valid.
Dynamic_cast requires runtime type info to check object type, so above usage
will get a NULL pointer. Some compilers don't support runtime-type-info,
in such case, the above usage won't work properly.

## cross initialization && switch-case

```
switch(a)
{
	case 1: 
		int num = 1; // in c++, cross initialization error.
		break
	default:
		break
}
```

switch-case is something like jump table.
[stackoverflow link](http://stackoverflow.com/questions/92396/why-cant-variables-be-declared-in-a-switch-statement)
has a detailed discussion about this type error.
This [page](http://www.complete-concrete-concise.com/programming/c/keyword-switch-case-default)
explains c/c++ switch-case keyword.

## Learning materials

- [Learn CPP websit](http://www.learncpp.com/) It provides very comprehensive 
contents about c++, very good materials for references.
- [C and C++ API Reference](http://www.cplusplus.com/) 
Very good websit for C++ STL API lookup, lots of usage 
demoes facilitate beginners.
- [Slide about C++ multiple inheritance object layout](http://lifegoo.pluskid.org/upload/doc/object_models/C++%20Object%20Model.pdf) 
A simple introduce to c++ memory layout, mainly focus on vptr implementation 
and runtime-type-info implementaion. It also mentioned the object construct 
order, but no detailed explanations.

