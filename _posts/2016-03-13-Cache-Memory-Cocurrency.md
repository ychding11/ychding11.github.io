---
layout: post
title: "Cache, Memory and Cocurrency" 
date: 2015-03-13
---

According to past trend, processor-memory performance gap will become larger,software larger and more complex. So design 
cache-conscious data layout in software design is important.  

## restrict key word 

According to C99 N1256 draft.    

>The intended use of the restrict qualifier (like the register storage class) is to promote optimization,
 and deleting all instances of the qualifier from all preprocessing translation units composing a conforming program does not change its meaning (i.e., observable behavior).

## cache and memory 
----------

This topic is listed in pbrt<sup>1065</sup>. It introduces concepts like *cache coherence*, *memory barrier* and 
*MESI coherence protocal* in book <sup>1076</sup>. In this charpter it also gives bad examples causing performance
penalty<sup>1077</sup>. The following lists useful materials mentioned in book.

- [What Every Programmer Should Know About Memory](https://people.freebsd.org/~lstewart/articles/cpumemory.pdf)
- [Article by Drepper](https://lwn.net/Articles/255364/)

## arena based memory allocation
----------

This topic is listed in pbrt<sup>1067</sup>. A well-designed memory management system is a boost to performance.
Allocation requests are always be responsed very quickly and cache miss number is reduced. Because it allocates 
memory from a large continuous memory region. This region is released only when the lifetime of all objects ends.


## atomic operation
-----

In book<sup>1078</sup>, there is a chapter about atomic operation. It is more efficient than mutex, but it only 
available on a very limited memory about 8 bytes. It is always be preferred to mutex in design. With the help of
atomic operation, we can desing *lock free* algorithms to increase cocurrency in programming. std::atomic does 
not support operation on float-pointing data type, see page<sup>1079</sup>. That is because float-point type 
does not support cumulative law, a + b + c != a + (b + c). In concurrency, add operation order is random. 

std::atomic<> support an atomic operation called "compar_exchange_xx(expected_ref, value)". It's a very useful
operation which can be used to realize many other atomic operation. The workflow of "compar & swap" operation is that.

1. compare atomic object *contained value* with *expected*.
2. if match, use *value* to update *containd value*.
3. if mismatch, use *contained value* update *expected*.


### reference
- [lock free programming](http://preshing.com/20120612/an-introduction-to-lock-free-programming/)
- [compar_exchange in c++ std libray](http://www.cplusplus.com/reference/atomic/atomic/compare_exchange_weak/)

## thread local variable
-----


## Reference
----------

- [Memory Optimization GDC2003](http://www.research.scea.com/research/pdfs/GDC2003_Memory_Optimization_18Mar03.pdf),
  the paper introduce some basic optimize principles in game programming, for example pointer aliasing avoidance by using restrict,
  using memory pool instead of allocating memory from heap, using local varible as much as possible.
- [strict key word usage](http://stackoverflow.com/questions/745870/realistic-usage-of-the-c99-restrict-keyword), the web page use a 
  good exmaple to illustrate how strick key world improve performance under certain condition. In addition, lots of links on the page are
  also very helpful.
- [Cache Conscious Data Structure](http://research.microsoft.com/en-us/um/people/trishulc/papers/Cache-conscious.pdf), author introduces
  3 design principles to improve spatial and temporal locality, so as to take advantage of cache and improve performance.
