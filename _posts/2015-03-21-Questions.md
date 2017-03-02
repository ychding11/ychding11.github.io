---
layout: post
title: "Questions" 
date: 2016-11-24
---

The post summarizes some software bugs and how to debug thme. It also talk about some ideas about software design.

## Infinite Function call loop

  Infinite Function Call Loop can lead to stack overflow, How can we detect it in runtime?   
  For example,A-->B-->C-->A. In other case, a very deep function call may also produce
  the same problem. It is more an design issue than a bug. In our design, for example,
  evaluating value in a DAG we should add such detection to avoid crash.

- C++11 introduces lamba expression, what is the advantage of the feature? Does it make program 
  run faster than before or use less memory? Does this expression debug friendly? Watch varible,
  set breakpoints, evaluate expression is more convenient?   

## Access Violation

  Access violation at address XXXX is a popular runtime error. Address 0x00000 always means 
  null pointer access, Address 0x12345AB or something like that maybe indicate array index is 
  out of bound. How the system distinguish one from another?

- Following disassembly code confused me. 

```
00007FFC54FC9D76  mov  rax,qword ptr [this]  the instruction just loads this value into rax, not the value pointed by this.
00007FFC54FC9D87  mov  rcx,qword ptr [rcx]   the instruction loads rcx referenced memory into rcx. 
```

## Question: From perspective of assembly language, is *this* an address?	

- What is the purpose of a protected or private destructor except that it prevents
  class instanced? Singleton pattern is not included.

- How can I disassemble a function from .o obj file? `objdump -d` just disassemble all. 
  How can I prevent a code snippet in a function from being optimized?

- Suppose that I want to design a set of APIs to draw geometry shape such as point,
  lines, quads, triangles and so on. A common way to do that is to create a base 
  class to define interfaces with pure virtual functions and leave implementation 
  to derived classes. It sounds fine. In some cases, we may encouter problems. For 
  example, we want APIs to support both float and double precision. Because virtual 
  function can not be a template, so there will be two sets of APIs and they are only 
  different in data type. That means you have duplicate code in your design. Any good 
  ideas to solve this problems? 

## How does a c++ compiler know a virtual funcation is called

  `pBasepter->f()` How compiler knows f() is a virtual function or
  an ordinary member function ?
 
## When is static class member destroyed

  Suppose designing a class which has static member variables. When do these static members
  destroyed? If they resides in a shared lib, lib unload operation can lead these variables 
  to destroy. Can any other things make these static members partially destroyed?

## delete[] operation causes crash issue.

```
delete[] mPtr;
```
The statement cause a segment fault error on Linux platform. But on Windows, it works fine.
I am sure mPtr is allocated by new[] operator and no double delete operation to mPtr pointer.
So what is the root cause?  Sometimes this kind of dynamic memory issue is very difficult.
It is unwise to analyse it by simply reviewing code line by line. We need debug tools help us,
for example, valgrind.
At last I found it is not the delete[] operation cause the problem. It is caused by memory
copy operation on dynamic heap where destination and source have different sizes. But the 
problems is not visible immediately. It appears at a later time when excute statements 
*delete []mPtr*

## assert() cause segment fault when program exits

It is an common error in huge software development that assert() may inform user with a message by a dialog.
Sometimes GUI moudle destroyed earlier than the depended module,So it will cause segments fault issue.

## badly designed public interface will cause desaster

public interface is the key of module design.For example *int ASimpleTexture::SizeInMemory(EMemoryScope scope).*
It is very unwise to use int to represent memory size. size_t is an obvious much better choice. Signed int can 
represent very limited memory size in 64 bit times.It is also uncompatible with standard libary which uses sizt_t
to respresent memory size in very common public interface such as *memcpy()*. Some day when you find the defects
you will found it is widely used in your code at the same time. There is no rescue other than to reconstruct your
code.

