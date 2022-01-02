---
layout: post
title: "Python resources collection" 
date: 2016-09-17
---

This post list python summary and related learning materials.

## Python Internal 
- [Source Code Analysis](https://github.com/stormzhang/free-programming-books)
- [Programming Language Pragmatics](http://www.cs.rochester.edu/~scott/pragmatics/)
- [Python 2.7 manual](http://docs.pythontab.com/python/python2.7/modules.html)

## Language Reference

- Home page : [The Python Language Reference](https://docs.python.org/3/reference/index.html)

### Data

- ***Objects***(identity, type and value) are Python’s abstraction for data.

#### Type

- **Type** : Type is an object. Types affect almost all aspects of object behavior. Standard type as following
  - **None** : There is a single object with this value. This object is accessed through the built-in name `None`
  - **NotImplemented** : There is a single object with this value. This object is accessed through the built-in name `NotImplemented`
    - Numeric methods and rich comparison methods should return this value if they do not implement the operation for the operands provided.
  - **Ellipsis** : There is a single object with this value. This object is accessed through the literal `...` or the built-in name `Ellipsis`. 
  - [`numbers.Number`](https://docs.python.org/3/library/numbers.html#numbers.Number) : immutable. Python distinguishes between integers, floating point numbers, and complex numbers.
    - [`numbers.Integral`](https://docs.python.org/3/library/numbers.html#numbers.Integral)
    - [`numbers.Real`](https://docs.python.org/3/library/numbers.html#numbers.Real) ([`float`](https://docs.python.org/3/library/functions.html#float))
    - [`numbers.Complex`](https://docs.python.org/3/library/numbers.html#numbers.Complex) ([`complex`](https://docs.python.org/3/library/functions.html#complex))
  - **Sequences** : These represent finite ordered sets indexed by non-negative numbers
    - Sequences also support **slicing**: `a[i:j]` selects all items with index *k* such that *i* `<=` *k* `<` *j*
    - Some sequences also support “extended slicing” with a third “step” parameter: `a[i:j:k]` selects all items of *a* with index *x* where `x = i + n*k`, *n* `>=` `0` and *i* `<=` *x* `<` *j*.
    - Immutable sequences : 
      - **Strings** : 
      - **Tuples** :
      - **Bytes** :
    - Mutable sequences :
      - **Lists** :
      - **Byte** **Arrays** :
  - **Mappings** :
  - **Set** : It represents **unordered**, finite sets of unique, **immutable** objects
  - **Callable** : to which the function call operation can be applied

#### Internal Type

A few types used internally by the interpreter are exposed to the user.

#### Garbage Collection

- ***GC*** : CPython currently uses a reference-counting scheme with (optional) delayed detection of cyclically linked garbage
  - which collects most objects as soon as they become unreachable
  - but is not guaranteed to collect garbage containing circular references
  - details : [gc page](https://docs.python.org/3/library/gc.html#module-gc)

## Debug Skills
- python module search path `import sys; sys.path`.
- env variable *PYTHONPATH* can  specify python module search path. Contents of this variable will be added into *sys.path*.
- `pp vars(self)`, print *self* objects in beauty format.
- `until`, execute to next line, especially usefull for single loop.
- `j linenumber`, jumpt to linenumber.
- `return`, return current function.


### reference
- [pdb docs](https://docs.python.org/2/library/pdb.html)

## Python Library & Github Repo

- [peuclid doc](https://github.com/ezag/pyeuclid/blob/master/euclid.rst)
- [Vertex Welding by Python](http://prideout.net/blog/?p=46)
- [Fluid Dynamics](http://http.developer.nvidia.com/GPUGems/gpugems_ch38.html)
- https://github.com/TheAlgorithms/Python
- https://github.com/zhiwehu/Python-programming-exercises
- 
