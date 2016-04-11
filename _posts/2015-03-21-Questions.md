---
layout: post
title: "Questions" 
date: 2015-03-21
---
- Function call loop can lead stack overflow, How can we detect it in runtime?   For example 
A-->B-->C-->A.   
- C++11 introduces lamba expression, what is the advantage of the feature? Does it make program 
run faster than before or use less memory? Does this expression debug friendly? Watch varible,
set breakpoints, evaluate expression is more convenient?   
- Access violation at address XXXX is a popular runtime error. Address 0x00000 always means 
null pointer access, Address 0x12345AB or something like that maybe indicate array index is 
out of bound. How the system distinguish one from another?
- Following disassembly code confused me. 

```
00007FFC54FC9D76  mov  rax,qword ptr [this]  the instruction just loads this value into rax, not the value pointed by this.
00007FFC54FC9D87  mov  rcx,qword ptr [rcx]   the instruction loads rcx referenced memory into rcx. 
```

Question: From perspective of assembly language, is *this* an address?	



