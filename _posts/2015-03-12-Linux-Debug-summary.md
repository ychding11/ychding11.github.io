---
layout: post
title: "Debug Summary" 
date: 2015-03-12
---

## Core Dump

- Check core dump state. using command to check default core file size. `ulimit -c` command is to query the default size of dumped core file size. 0 means core dump is currently not enabled. You must enble core dump functionaliy first.
- Enable core dump. set default core file size to unlimited using command.Note that the setting is active only in current terminal.
`ulimit -c unlimited` command set core file size, unlimited means that dumped core file size has no limitation.
- Customizing dumped file name. Configure file */proc/sys/kernel/core_pattern*  controls core file name convention.
`echo "/tmp/corefiles/core-%e-%p-%t" >> /proc/sys/kernel/core_pattern` set a new pattern for generated core file. To ensure command takes effects, use command `cat /proc/sys/kernel/core_pattern` to check the settings. Below is an explanations for some wildcard for references.   
- Analyse dumped core file with gdb. Rebuild code with debug info added,for example adding -g compile option for gcc. 
Use following command to open core file. `gdb a.out /tmp/corefiles/core-a.out-12754-1457789593`

For reference.  

+ *%e*  executable filename (without path prefix) 
+ *%p*  PID of dumped process, as seen in the PID namespace in which the process resides
+ *%t*  time of dump, expressed as seconds since the Epoch,1970-01-01 00:00:00 +0000 (UTC)

## x64 Assembly Disassemble

   When program crashes, Windows will pop a dialog informing user of error,
   mostly memory access violation. We need assembly language knowledge to 
   analyse disassembly code. x64 assembly language is different from that of 
   32-bit version. Register files and function calling conventions are key 
   to understand disassemble code. C++ non-static member function calling 
   convention: Implicit first parameter is *this* pointer transferred by 
   rcx register. Implicit second parameter is used for returning object 
   which is not fit rax register. Usually return value is transferred by 
   *rax* register. But for user-defined complex type register *rax* is unable 
   to transfer. It requires an implicit parameter to transfer. By default, 
   second parameter is transferred by register *rdx*. In addition, the max 
   number of parameters which can be transferred by register is 4.

   Because the first four parameters are transferred by register, in function 
   design we should keep this point in mind. Register access is much faster than 
   memory access.

[Calling Conventions for different platform C++ compilers](http://www.agner.org/optimize/calling_conventions.pdf) It is a very
comprehensive material for reference. It should be referenced at first time when having problems. 
[MicroSoft document about disassembly code](https://msdn.microsoft.com/en-us/library/windows/hardware/ff538083(v=vs.85).aspx) 
has lots of C++ disassembly code examples. Learning such examples does much help to crash issues.
[C++ this pointer storage](http://stackoverflow.com/questions/16585562/where-is-the-this-pointer-stored-in-computer-memory)     
[Introduce to x64 assemble under Linux Platform](https://cs.nyu.edu/courses/fall11/CSCI-GA.2130-001/x64-intro.pdf) The paper 
mainly focus on C Compiler. 
[intel LEA instruction explaination](https://courses.engr.illinois.edu/ece390/archive/spr2002/books/labmanual/inst-ref-lea.html) 
*lea* instruction only calculate effective memory address, no memory 
access happens.
The [link](http://stackoverflow.com/questions/1699748/what-is-the-difference-between-mov-and-lea) 
gives an comparison between *lea* instruction and *mov* instruction.


## Dynamic Shared Library

[How to write shared Libaries](https://www.akkadia.org/drepper/dsohowto.pdf) 
Program using dynamic shared libraries need to know dynamic linker location. 
Dynamic linker need to be loaded to memory by OS, then OS transfer control to 
dynamic linker.     Dynamic linker will do 3 things: determine and load all 
dependencies, relocation all addresses in program and dependencies, initialize 
program and all dependencies only once.      In a complex software, dependencies 
need to apply toplogical sort to determine correct order. 

Symbol relocation is a very expensive process.Relocated results are stored in data segment.
Hash algorithm is applied in symbol search. For each resolving symbol, dynamic 
linker will loop each dynamic shared lib in current lookup scope. Every shared lib 
organise its symbols in hash bucket. So the match process is to use resolving symbol's
hash value to match has chain in each lib. Once matched, the process ends. 
So multiple definition for a same symbol is ok, the first matched will be used. 
The average hash chain length both for successfull match and unsuccessful match 
determines effectiveness of dynamic liner's hash algorithms design.

Symbol relocation process may be delayed to some later time when a symbol is 
actually used. This is called *lazy relocation process*. Using *-z now* linker 
option can cancell it.
[This paper](https://cseweb.ucsd.edu/~gbournou/CSE131/the_inside_story_on_shared_libraries_and_dynamic_loading.pdf)
introduces some debug skils about load dependencies errors. Some of them 
are very interesting.

## gdb debuger
A debuger is an important tool to analyse runtime errors.
Compared with MSVC debuger, I found gdb is not so friendly.

- browser source code quickly.
- set breakpoints quickly.
- display complex object clearly.
- eximamine memory friendly.
- `disassemble/m $pc-20, $pc+20`
- `print $pc`
- `break filename:lineno`
- `layout src` 

Only meeting above requirements, a debuger is a good debuger.
When encounter a memory access violation, progress will receive a signal to 
terminate itself. 
+ It need to know which address caused the fault. `p $_siginfo._sifields._sigfault.si_addr`
+ current instruction. `layout asm` and `ctrl+x+a` quit layout mode.
+ check register value. `info registers`
+ check PC value. `p $pc`, $pc = %rip.
+ check progress memory map. `cat /proc/pid/maps`

## gcc __attribute__
`void __attribute__((optimize("O0"))) func() { }` can assure no optimization 
applied to specified function. Sometimes it is helpful to analyse errors only 
occured in release version. Even if func is a template.

### reference
- [Detailed core pattern setting](http://man7.org/linux/man-pages/man5/core.5.html) 
- [An introduce to core dump](http://www.cnblogs.com/hazir/p/linxu_core_dump.html)    
- The [page](http://dirac.org/linux/gdb/06-Debugging_A_Running_Process.php)
  tells the story about how to debug a running process with gdb. Use gdb help command
  `help running`.



