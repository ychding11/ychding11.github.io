---
layout: post
title: "C++ features and resources"
date: 2016-11-24
---

The post summarizes new features of c++ and list available internet resources about cpp.


## overloaded virtual function

[Discussion of GCC -Woverloaded-virtual option](https://gcc.gnu.org/ml/gcc/1999-02n/msg00180.html) 
introduce this issue. Compiler is unable to tell "I am creating a new virtual function" from
"I am overriding a virtual function from my baseclass". So GCC adds this option 
to detect potential errors. But I think the option is not a good idea, especially
when c++11 introduce a new key word *override* to explicitly tell compiler whether
it wants to override existing virtual function or create new virtual function.
For quick reference, I list some frequently used GCC compile options here. More
compiler options please refer gcc manual. 

- Werror, Treat all warnings as errors.
- Wall, 
- Wextra,
- Wl,option, pass options to linker, *option* is linker option.
- g3, include macro infomation into debug info.

## const reference

Under what condition is this type useful? What problems will it cause?
When user-defined complex type used as an input parameter, it is a good 
choice and  highly recommanded in google c++ code style.     
It is a very bad idea when used as a data holder It almost have no flexibility.
For example, following design is impossible for reference type. In this case,
const pointer type is good choice.

{% highlight cpp linenos %}

struct Holder
{
	int type;
	union
	{
		int &ri;
		float &rf;
	};
};

{% endhighlight %}


## using statement

`using` keyword can introduce base class member into derived class.
For example expose base class protected member to public in drived 
class.`using base::member` if member is name of overloaded function 
in base class, then all functions with the same name will be introduced 
into derived class. This is very important. In such case, if 
derived class already have a member with same name, parameter list and 
qualifier then derived class member will hide or override the 
newly introduced ones, but they  are not conflict with each other.
[The web link](http://en.cppreference.com/w/cpp/language/using_declaration)
gives the detailed info and a good example.

## vtbl & vptr

Where is vtable stored? Is this compiler dependent?
Is vptr always offset 0 in an object memory layout?
How about multiple inheritance?

Is vtbl class specified? Derived class and base class
have their own vtbl even no function override occur?

derived class does not override base class virtual function.

{% highlight cpp %}

class Base
{
	public:
    Base() {}
    ~Base() { std::cout << "~Base()" << std::endl; }
    virtual void f(int, int)
    { std::cout << "Base::f(int, int)" << std::endl; }
    virtual void f(int)
    { std::cout << "Base::f(int)" << std::endl; }
};

class Derived : public Base
{
public:
    Derived() {}
    ~Derived(){}
};

int main()
{
   Base base;
   Derived derive;
   return 0;
}                                                    

{% endhighlight %}

vtbl info:

```
(gdb) info vtbl base
vtable for 'Base' @ 0x400c30 (subobject @ 0x7fffffffdf20):
[0]: 0x400a86 <Base::f(int, int)>
[1]: 0x400ab6 <Base::f(int)>
(gdb) info vtbl derive
vtable for 'Derived' @ 0x400c10 (subobject @ 0x7fffffffdf10):
[0]: 0x400a86 <Base::f(int, int)>
[1]: 0x400ab6 <Base::f(int)>
```

derived class partly overrides base class virtual function

```
 43 class Base
 44 {
 45  public:
 46     Base() {}
 47     ~Base() { std::cout << "~Base()" << std::endl; }
 48     virtual void f(int, int)
 49     { std::cout << "Base::f(int, int)" << std::endl; }
 50     virtual void f(int)
 51     { std::cout << "Base::f(int)" << std::endl; }
 52 };
 53 
 54 class Derived : public Base
 55 {
 56 public:
 57     Derived() {}
 58     ~Derived(){}
 59     virtual void f(int)
 60     { std::cout << "Derived::f(int)" << std::endl; }
 61 };
 62 
 63 int main()
 64 {
 65    Base base; // will be instanced with protected destructor?
 66    Derived derive;
 67    return 0;
 68 }                                                   
```

```
(gdb) info vtbl base
vtable for 'Base' @ 0x400c70 (subobject @ 0x7fffffffdf20):
[0]: 0x400a86 <Base::f(int, int)>
[1]: 0x400ab6 <Base::f(int)>
(gdb) info vtbl derive
vtable for 'Derived' @ 0x400c50 (subobject @ 0x7fffffffdf10):
[0]: 0x400a86 <Base::f(int, int)>
[1]: 0x400b30 <Derived::f(int)>
```

derived class completely overrides base class virtual function and 
create new virtual function.

```
 43 class Base
 44 {
 45  public:
 46     Base() {}
 47     ~Base() { std::cout << "~Base()" << std::endl; }
 48     virtual void f(int, int)
 49     { std::cout << "Base::f(int, int)" << std::endl; }
 50     virtual void f(int)
 51     { std::cout << "Base::f(int)" << std::endl; }
 52 };
 53 
 54 class Derived : public Base
 55 {
 56 public:
 57     Derived() {}
 58     ~Derived(){}
 59     //using Base::f;
 60     virtual void f(int, int, int)
 61     { std::cout << "Derived::f(int, int, int)" << std::endl; }
 62     virtual void f(int, int)
 63     { std::cout << "Derived::f(int, int)" << std::endl; }
 64     virtual void f(int)
 65     { std::cout << "Derived::f(int)" << std::endl; }
 66 };
 67 
 68 int main()
 69 {
 70    Base base;
 71    Derived derive;
 72    Base *b = &derive;
 73    b->f(0, 1);
 74    return 0;
 75 }                                     
```
Corresponding vtbl info:

```
(gdb) info vtbl base
vtable for 'Base' @ 0x400df0 (subobject @ 0x7fffffffdf20):
[0]: 0x400b52 <Base::f(int, int)>
[1]: 0x400b82 <Base::f(int)>
(gdb) info vtbl derive
vtable for 'Derived' @ 0x400db0 (subobject @ 0x7fffffffdf10):
[0]: 0x400c30 <Derived::f(int, int)>
[1]: 0x400c60 <Derived::f(int)>
[2]: 0x400bfc <Derived::f(int, int, int)>
(gdb) info vtbl b
vtable for 'Base' @ 0x400db0 (subobject @ 0x7fffffffdf10):
[0]: 0x400c30 <Derived::f(int, int)>
[1]: 0x400c60 <Derived::f(int)>

```

```
class Base
{
 public:
    Base() {}
    ~Base() { std::cout << "~Base()" << std::endl; }
    virtual void f(int, int)
    { std::cout << "Base::f(int, int)" << std::endl; }
    virtual void f(int)
    { std::cout << "Base::f(int)" << std::endl; }
};

class Derived : public Base
{
public:
    Derived() {}
    ~Derived(){}
    virtual void f(int)
    { std::cout << "Derived::f(int)" << std::endl; }
    virtual void f(int, int)
    { std::cout << "Derived::f(int, int)" << std::endl; }
};

int main()
{
   Base base; // will be instanced with protected destructor?
   Derived derive;
   Base *p = &derive;
   p->f(0);
   return 0;
}                            
```
Check the virutal table.

```
(gdb) info vtbl base
vtable for 'Base' @ 0x400d70 (subobject @ 0x7fffffffdf20):
[0]: 0x400b52 <Base::f(int, int)>
[1]: 0x400b82 <Base::f(int)>
(gdb) info vtbl derive
vtable for 'Derived' @ 0x400d50 (subobject @ 0x7fffffffdf10):
[0]: 0x400c2a <Derived::f(int, int)>
[1]: 0x400bfc <Derived::f(int)>
```

## type casting

### const_cast

Const type cast is used to add or remove the constness of an object pointed
by a pointer. For example `const_cast< const type*>(pointer)` or 
`const_cast<type*>(pointer)`. The latter one is always used to match interface.
*When used, be caution, write operation will cause undefined behavior.*

### static_cast & dynamic_cast

These two type cast can do pointer upcast and downcast. Upcast is converting
a pointer-to-derived to a pointer-to-base. Downcast is converting a 
pointer-to-base to a pointer-to-derived. static_cast does it without runtime 
checking. `Base *pb = new Base(); static_cast<Derived*>(pb);` is valid.
Dynamic_cast requires runtime type info to check object type, so above usage
will get a NULL pointer. Some compilers don't support runtime-type-info,
in such case, the above usage won't work properly.

## cross initialization && switch-case

Following code demostrate this kind of error.

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
call can be decided at compile time, *inline* makes sense here.
Remember that *inline* is just a hint to compiler, it is up 
to compiler to decide inline it or not.

## default member initializer

The behavior is the same with constructor initializing list.

## auto_ptr

*ownership* is the key to understand auto_ptr. At any time 
an auto_ptr has an ownership to an object or in null state.
So copy operation will make one auto_ptr lose ownership. 
Because of this reason, std containers are not suitable for 
auto_ptr, because some operations of these container may need
copy operation.     For const auto_ptr, it can only be used to 
do dereference. Copy from it will cause error.
[using auto_ptr effectively](http://www.gotw.ca/publications/using_auto_ptr_effectively.htm)
gives several good examples for reference.

## private inheritance

private inheritance is something like composition(has-a).
while, Inheritance, in general, is a is-a. It has some
unique features compared to composition.

- use protected interface of Base class.
- tye case to Base class.
- call Base class interface directly.

This [page](https://isocpp.org/wiki/faq/private-inheritance) has a detailed explanation about private inheritance.

## operator overload

*operator overload* is to tell compiler how built-in operators apply to 
user-defined data types by function overloading. Properly written
operator overload can lead to more readable and maintainable code.

- overloaded operators are just regular function.
- operator overload can be implemented as a class
  member function or a free function.
- lvalue and rvalue are key concepts in operator 
  overload. The overloaded operators need to return 
  somethings. When they returns const type, it is 
  rvalue. non-const type, lvalue.
- *operator ++* and *operator --* are different from
  other operators. Because they have two versions:
  prefix and postfix. When overloading, compiler
  needs to tell one from another. They are unary operators.
  when overloading as a member function, no input parameter is needed.
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
  from their meaning, they should not be reinterpreted 
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

## IEE745 Float Point Number Representation

[IEEE745](http://cs.boisestate.edu/~alark/cs354/lectures/ieee754.pdf)
introduction includes binary formats, ranges and examples.
Web based [demo](http://babbage.cs.qc.cuny.edu/IEEE-754/) gives
binary format when texting an real number. Very interesting place to 
learn IEEE745 standard.

It is different from integer number that when diving by an zero, it
does not cause an exception, instead you get an inf. Sample code:
```
 int main()
 {
 
     cout << 1.0 / 0.0 << std::endl;
     cout << -1.0 / 0.0 << std::endl;
     return 0;                                                                                                                                                                                                  
 }   
```

Result:
```
$ ./a.out 
inf
-inf
```

## placement new

It provides a method to place an object to a paticular location in memory 
when calling constructor. It also transfers the responsibility to programmer 
that the memory address passed to "placement new" is big enough to hold the 
object and meet special requirements such that memory alignment. Usually in 
this case, explicitly calling destructor is needed. It is reasonable because
that you take the role of comiler here.

## only destructor declared as private or protected

It can force the class only can be allocate dynamically. 
That is object can only be created by new(). Object 
allocate in such way `Type obj` is forbidden.

## in-class member initializer

[Stroustrup talks](http://www.stroustrup.com/C++11FAQ.html#member-init)

## C++11 new features

+ defaulted and deleted function.To declare a deleted function, you can append the *=delete;* specifier to
  the end of a function declaration. The compiler prohibits the usage of a deleted function.
+ [C++11 deleted function](https://www.ibm.com/developerworks/community/blogs/5894415f-be62-4bc0-81c5-3956e82276f3/entry/deleted_functions_in_c_11?lang=en)
+ [MSDN comments on deleted and defulted functions](https://msdn.microsoft.com/en-us/library/dn457344.aspx)
+ C++11 use delete and defualt key word to instruct compliler default behaviors so as to 
  reduce coding defects.


## other learning materials

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
