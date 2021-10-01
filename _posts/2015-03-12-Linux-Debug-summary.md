---
layout: post
title: "Debug summary" 
date: 2016-12-7
---

This post summarizes debugging skills with examples on Linux Platform.

## Core dump
- Check whether core dump is enabled. Using command to check default core file size.
  - `ulimit -c` command is to query the default size of dumped core size. 0 means core dump is not enabled yet.
  -  You must enble core dump functionaliy first.
- Enable core dump. This can be done by setting default core file size to unlimited.
  - Note that the setting is active only for current terminal.
  - `ulimit -c unlimited` command is to set core file size, unlimited means dumped core file size has no limitation.
- Customizing dumped file name. Configure file */proc/sys/kernel/core_pattern*  controls core file name convention.
  - `echo "/tmp/corefiles/core-%e-%p-%t" >> /proc/sys/kernel/core_pattern` set a new pattern for generated core file.
  - To ensure command takes effects, read `cat /proc/sys/kernel/core_pattern` to check the settings. Below is simple list for some wildcard for references.   
- Analyse dumped core file with gdb. Rebuild code with debug info added,for example adding -g compile option for gcc. 
  - Use command to open core file. `gdb a.out /tmp/corefiles/core-a.out-12754-1457789593`
- parameters reference  
	+ *%e*  executable filename (without path prefix) 
	+ *%p*  PID of dumped process, as seen in the PID namespace in which the process resides
	+ *%t*  time of dump, expressed as seconds since the Epoch,1970-01-01 00:00:00 +0000 (UTC)

### reference
- [A core pattern setting](http://man7.org/linux/man-pages/man5/core.5.html) 
- [An introduce to core dump](http://www.cnblogs.com/hazir/p/linxu_core_dump.html)    
- [Debug a runnig process](http://dirac.org/linux/gdb/06-Debugging_A_Running_Process.php)

## x64 Assembly Disassemble
When program crashes, Windows will pop up a dialog to inform user of error. Mostly it is a memory access violation. We need assembly language knowledge to analyze disassembly code. x64 assembly language is different from that of 32-bit version.  Register files and function calling conventions are key to understand disassemble code. 

- C++ non-static member function calling convention:

- Implicit first parameter is *this* pointer transferred by rcx register. 
- Implicit second parameter is used for returning object which is not fit rax register. 
- Usually return value is transferred by *rax* register. But for user-defined complex type, register *rax* is unable to transfer. It requires an implicit parameter to transfer. By default, second parameter is transferred by register *rdx*. 
- In addition, the max number of parameters which can be transferred by register is 4.
  - Because the first four parameters are transferred by register, in function design we should keep this point in mind. Register access is much faster than memory access.

### reference
- [Calling Conventions for different platform C++ compilers](http://www.agner.org/optimize/calling_conventions.pdf) It is a very comprehensive material for reference. It should be referenced at first time when having problems. 
- [MicroSoft document about disassembly code](https://msdn.microsoft.com/en-us/library/windows/hardware/ff538083(v=vs.85).aspx)  has lots of C++ disassembly code examples. Learning such examples does much help to crash issues.
- [C++ this pointer storage](http://stackoverflow.com/questions/16585562/where-is-the-this-pointer-stored-in-computer-memory)     
- [Introduce to x64 assemble under Linux Platform](https://cs.nyu.edu/courses/fall11/CSCI-GA.2130-001/x64-intro.pdf) The paper mainly focus on C Compiler. 
- [LEA instruction explaination](https://courses.engr.illinois.edu/ece390/archive/spr2002/books/labmanual/inst-ref-lea.html) *lea* instruction only calculate effective memory address, no memory access happens.
- [The link](http://stackoverflow.com/questions/1699748/what-is-the-difference-between-mov-and-lea) gives an comparison between *lea* instruction and *mov* instruction.

## Dynamic Shared Library
When loading into memory, program using dynamic shared libraries needs to know dynamic linker location. OS will load dynamic linker into memory, then transfer the control to dynamic linker. It will do 3 things:
	1. determine and load all dependencies,
	2. relocation all addresses in program and dependencies,
	3. initialize program and all dependencies only once.
	In a complex software, it may need to apply topological sort to determine correct dependencies order. 

Symbol relocation is  very expensive. Relocated results are stored in data segment. Hash algorithm is applied in symbol search.
Every shared lib organize its symbols in hash bucket. For every symbol, dynamic linker will loop each dynamic shared lib in current lookup scope.
So the match algorithm is to use resolving symbol's hash value to match hash chain in each lib. Once matched, the algorithm ends.
So multiple definition for a same symbol is ok, the first matched will be used.
The average hash chain length determines effectiveness of dynamic liner's hash algorithms. It is for both for successful match and unsuccessful match. 

Symbol relocation may be delayed to some later time when a symbol is actually used. This is called *lazy relocation process*. linker option *-z now* can cancell this feature. 

### useful tips 
- *-Wl,option* is an gcc option to transfer *option* into linkers.
- *-Wl,-rpath,'$ORIGIN'* option will tell linker to search current directory for needed libraries.
- env variable *LD_LIBRARY_PATH*, control dynamic library search path.
- in *bash*, use `export LD_LIBRARY_PATH=/usr/lib64` to modify its value.
- in *tsh*, use `setenv`.
- `readlink -f filename`, display the full path of file.

### reference
- [How to write shared Libaries](https://www.akkadia.org/drepper/dsohowto.pdf) 
- [This paper](https://cseweb.ucsd.edu/~gbournou/CSE131/the_inside_story_on_shared_libraries_and_dynamic_loading.pdf) introduces debug skills about load dependencies errors. Some of them are very interesting.

## GDB Skills
GDB is an important tool to analyze runtime errors. Compared with MSVC debugger, I found **gdb** is not very friendly to use.

### features wanted
- browser source code quickly.
- set breakpoints quickly.
- display complex object clearly.
- examine memory friendly.

### tips
- list current source files and its full path: `info source`
- list current shared libs(xxx.so) current process loaded: `info files`
- after in gdb mode, gdb command`run argument-list` can run the executables with argument list.
- show or set debugged program's arguments. `show args` and `set args xxxxx`
- disassemble binary code. `disassemble/m $pc-20, $pc+20`
- check current instruction. `print $pc`
- set break points. `break filename:lineno`
- check source file. `layout src` 
+ print the address causing segment fault: `p $_siginfo._sifields._sigfault.si_addr`. When encountering a memory access violation, process will receive a signal from OS to terminate itself. 
+ current instruction. `layout asm` and `ctrl+x+a` quit layout mode.
+ check register value. `info registers`
+ check PC value. `p $pc`, $pc = %rip.
+ check progress memory map. `cat /proc/pid/maps`
+ check type details. `ptype typename`
+ check macro info. `p macro-name` print evaluated value. `macro expand macro-name` print expanded expression. NOTE: add -g3 compiler option to keep macro info presented in program.
+ set source files search path, `dir path-to-source`
+ set breakpoints, `b file::line`
+ disable breakpoints, `disable b` and `disable b no`
+ query breakpoints, `info b`
+ display all thread stack info, `thread apply all bt`
+ display loaded shared libraries, `info shared`
+ display how to run program. `help running`
+ run shell command under gdb context. `shell ls -l`
+ search process name and id. `shell pgrep -l pattern`
+ stop current current process and put it into background. `Ctrl + z`
+ watch memory location `watch/rwatch -l expression`
- running program, `next, step` and `nexti, stepi`

### reference
- [GDB mannual](http://sourceware.org/gdb/current/onlinedocs/gdb/) char extend to int using sign extension.

## GCC extension

### GCC __attribute__

`void __attribute__((optimize("O0"))) func() { }` can assure no optimization 
applied to specified function. Sometimes it is helpful to analyse errors only 
occured in release version. This Gcc extension can also be applied to function template.

## Simple debug Case Summary

1. Parameter *this* is overridden by write operations from within function. 
   - It leads to memory access violation when accessing object member variable. In addition, the bug only occurs on Linux platform. So I trend to believe stack layout is different between Linux and Windows.
2. When an image library parses a psd file. It consumes lots of time and crash the running program. Crash issue maybe caused by memory access violation or stack overflow. Performance issue is always difficult to handle. This time I use debugger to step every statements to dig the bottleneck. At the end I found several parsed elements is extremely large. They control loops and determine memory allocation size. These operations do non-sense things and consume lots of time. 

