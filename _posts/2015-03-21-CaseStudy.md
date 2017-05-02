---
layout: post
title: "Design and debug case study" 
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
  out of bound. How the system distinguish one from another? following disassembly code confused me. 

	00007FFC54FC9D76  mov  rax,qword ptr [this]  just loads this value into rax, not the value pointed by this.
	00007FFC54FC9D87  mov  rcx,qword ptr [rcx]   loads rcx referenced memory into rcx. 

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

## steps of debugging OpenGL lib loading error On Linux

For example:

	libGL error: unable to load driver: swrast_dri.so
	libGL error: failed to load driver: swrast

You can use the following steps to locate the problem.

1. set `export LIBGL_DEBUG=verbose` and run program again. More details will be output.  
```
libGL: screen 0 does not appear to be DRI2 capable
libGL: OpenDriver: trying /usr/lib/dri/tls/swrast_dri.so
libGL: OpenDriver: trying /usr/lib/dri/swrast_dri.so
libGL: dlopen /usr/lib/dri/swrast_dri.so failed (/usr/lib/dri/swrast_dri.so: cannot open shared object file: No such file or directory)
libGL: OpenDriver: trying /usr/lib64/dri/tls/swrast_dri.so
libGL: OpenDriver: trying /usr/lib64/dri/swrast_dri.so
libGL: dlopen /usr/lib64/dri/swrast_dri.so failed (/usr/lib64/dri/swrast_dri.so: undefined symbol: _glapi_tls_Dispatch)
libGL error: unable to load driver: swrast_dri.so
libGL error: failed to load driver: swrast
``` 
It use the software raster, but I have a nvidia graphic card installed. 
2. check lib symbols `readelf -Ws /usr/lib64/dri/swrast_dri.so |grep _glapi_tls`
```
109: 0000000000000000     0 TLS     GLOBAL DEFAULT  UND _glapi_tls_Dispatch 
401: 0000000000000000     0 TLS     GLOBAL DEFAULT  UND _glapi_tls_Context 
```	
3. explore systme log `cat /var/log/Xorg.0.log | grep  "GL"` to check graphics related modules.
4. check GL lib on my 64bit arch, `find /usr/lib64 -name "libGL.so*" `
```
/usr/lib64/libGL.so.1
/usr/lib64/libGL.so.352.63  hardware driver
/usr/lib64/libGL.so   this links to softwar one
/usr/lib64/libGL.so.1.2.0  software driver
```

So I believe system configuration makes mistakes. After reinstall graphic card driver, the problem disapears.

## random crash caused by unintialized local varibles

Following code:

```
 GLint major, minor;
 glGetIntegerv(GL_MAJOR_VERSION, &major);
 glGetIntegerv(GL_MINOR_VERSION, &minor);

```

It try to fetch OpenGL version No by a GL call with two uninitialized variables.
The following code will check the fetched version NO. and decide to whether to init 
other GL external modules. The risk is that when your context does not support GL call to 
`glGetIntegerv`, it leaves params `major` and `minor` unchanged. The uninitialized variable
with random value will make systme behavior unpredictable and very hard to guess failing root cause.

## abs() result in unwanted behavior

Following code:

```
float2 normal(v1.y - v0.y, v0.x - v1.x);
float2 normalAbs(abs(normal.x), abs(normal.y));

```

[abs function](http://www.cplusplus.com/reference/cmath/abs/) accept `int` as input parameter in C runtime library. 
C++ runtime library provides overloads for abs function and called by std::abs(xxx). The risk of the above code is 
that it is not obvious to use which one. The runtime result is that It works fine on windows platform by accepting 
`float` type param, while behavioring strangely on *Linux and Mac* platform by accepting `int` param. In this case 
I prefer to use function `fabs` because the data type is float under current context.

## uncaught exception crashes system

## operator overload leads to unpredicted behavor
