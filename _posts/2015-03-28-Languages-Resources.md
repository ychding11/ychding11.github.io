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

Under what condition is this type useful? What problems will it cause?
When user-defined complex type used as an input parameter, it's a good 
choice. It's  highly recommanded in google's c++ code style.     
But used as a data holder is a very bad idea. It almost have no flexibility.
For example, following design is impossible for reference type. Sometimes 
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
qualifier then derived class member will hide or override the 
newly introduced ones, but they  are not conflict with each other.
[The web link](http://en.cppreference.com/w/cpp/language/using_declaration)
gives the detailed info.

## vtbl & vptr
Where is vtable stored? Is this compiler dependent?
Is vptr always offset 0 in an object memory layout?
How about multiple inheritance?

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
has a detailed discussion about this kind of error.
This [page](http://www.complete-concrete-concise.com/programming/c/keyword-switch-case-default)
explains c/c++ switch-case keyword.

## inline and virtual
Using *inline* and *virtual* for the same member function does 
make sense. virtual function is also a member function. When 
calling virtual function by base class pointer or reference,
late binding applies. But for a known object, resolving function 
call can be decided at compile time, so *inline* makes sense here.
And remember thant *inline* is just a hint to compiler, it is up 
to compiler to decide inline it or not.

## default member initializer


## auto_ptr
*ownership* is the key to understand the auto_ptr. At any time 
an auto_ptr has an ownership to an object or in null state.
So copy operation will make one auto_ptr lose ownership. 
Because of this reason, std containers are not suitable for 
auto_ptr, because some operations of these container may need
copy operation.     For const auto_ptr, it can only be used to 
do dereference. Copy from it will cause error.
[using auto_ptr effectively](http://www.gotw.ca/publications/using_auto_ptr_effectively.htm)
gives several examples for reference.

## private inheritance
private inheritance is something like composition(has-a).
while, Inheritance, in general, is a is-a. It has some
unique features compared to composition.

- use protected interface of Base class.
- tye case to Base class.
- call Base class interface directly.

This [page](https://isocpp.org/wiki/faq/private-inheritance) 
has a detailed explanation about private inheritance.

## operator overload
*operator overload* is to compiler how built-in operators apply to 
user-defined data types by function overloading. Properly written
operator overload can lead to more readable and maintainable code.

- overloaded operators are just regular function.
- operator overload can be implemented as a class
member function or a free function.
- lvalue and rvalue are key concepts in operator 
overload. The overloaded operators need to return 
somethings. When they returns const type, it is 
rvalue. non-const type, lvalue.
- *operator ++* and *operator --* are different, 
because they have two versions: prefix and postfix.
When overloading, compiler needs to tell one from 
another. They are unary operators. when overloading 
as a member function, no input parameter is needed.
In order to different them, postfix operator accepts
an int type parameter. 
- In operator overload, prefix ++ and -- are more 
efficient than posfix ones and implement posfix ones 
in terms of prefix ones.
- Rational operator overload, just need to overload 
operator < correctly. All other rational operators
can based on it. For a user-defined data type, after
implementing operator <, it can be insert into STL 
containers. In addition, operator < must adhere two 
laws: Trichotomy and tansitivity.
- stream insert operator << and >> should be 
overloaded as a free function and decare the 
function as an friend of the user-defined 
data type. 
- Some operators can not be overloaded, because 
from their meaning, they should be reinterpreted 
by user-defined data type. For example, scop resolution 
::, member selection ., sizeof, typid, typcast, 
ternary condition ?:.
- [reference](http://www.keithschwarz.com/cs106l/fall2010/course-reader/Ch10_OperatorOverloading.pdf)

## random number generator
[This page](http://www.scratchapixel.com/lessons/mathematics-physics-for-computer-graphics/monte-carlo-methods-in-practice/generating-random-numbers)
is a good introduction to the backgroud of random number 
generator. Presudorandom-number-generator is more easily integrated 
into software. It based on some generating algorithms, for example,
middle-square-method presented by Von Neumann. So the generated 
random sequence is periodic because of the limitation of computer.

## Learning materials

- [Learn CPP websit](http://www.learncpp.com/) It provides very comprehensive 
contents about c++, very good materials for references.
- [C and C++ API Reference](http://www.cplusplus.com/) 
Very good website for C++ STL API lookup, lots of usage 
demoes facilitate beginners.
- [Slide about C++ multiple inheritance object layout](http://lifegoo.pluskid.org/upload/doc/object_models/C++%20Object%20Model.pdf) 
A simple introduce to c++ memory layout, mainly focus on vptr implementation 
and runtime-type-info implementaion. It also mentioned the object construct 
order, but no detailed explanations.
- [Standford CS106L 2010 Fall](http://www.keithschwarz.com/cs106l/fall2010/)
