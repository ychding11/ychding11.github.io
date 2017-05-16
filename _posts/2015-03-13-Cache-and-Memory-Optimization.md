---
layout: post
title: "Cache and Memory" 
date: 2015-03-13
---

According to past trend, processor-memory performance gap will become larger,software larger and more complex. So design 
cache-conscious data layout in software design is important.  

## restrict key word 

According to C99 N1256 draft.    

>The intended use of the restrict qualifier (like the register storage class) is to promote optimization,
 and deleting all instances of the qualifier from all preprocessing translation units composing a conforming program does not change its meaning (i.e., observable behavior).

## cache and memory in pbrt book

This topic is listed in pbrt<sup>1065</sup>. The following lists useful materials mentioned in book.

- [What Every Programmer Should Know About Memory](https://people.freebsd.org/~lstewart/articles/cpumemory.pdf)
- Article by Drepper, [link](https://lwn.net/Articles/255364/)

## Reference

- [Memory Optimization GDC2003](http://www.research.scea.com/research/pdfs/GDC2003_Memory_Optimization_18Mar03.pdf)
the paper introduce some basic optimize principles in game programming, for example pointer aliasing avoidance by using restrict,
using memory pool instead of allocating memory from heap, using local varible as much as possible.
- [strict key word usage](http://stackoverflow.com/questions/745870/realistic-usage-of-the-c99-restrict-keyword) The web page use a 
good exmaple to illustrate how strick key world improve performance under certain condition. In addition, lots of links on the page are
also very helpful.
- [Cache Conscious Data Structure](http://research.microsoft.com/en-us/um/people/trishulc/papers/Cache-conscious.pdf) Author intruduce
3 design principles to improve spatial and temporal locality, so as to take advantage of cache and improve performance.
