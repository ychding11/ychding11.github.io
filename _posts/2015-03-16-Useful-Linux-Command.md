---
layout: post
title: "Usefull Linux Commands" 
date: 2015-03-16
---
- `pkg-config --static --libs glfw3`  tell you how to link glfw3 lib in correct order  
- `locate -b '\qmake'` find qmake path using exact name
- `objdump -t a.out` display symbol table of elf file a.out
- `objdump -h a.out` display sections of elf file a.out
- `ls | wc -l` tell how many files in current directory 
- `ls -l | grep ^d` list directory items in current directory
- `lspci | grep -i vga` query graphic cards info   
```
00:02.0 VGA compatible controller: Intel Corporation 3rd Gen Core processor Graphics Controller (rev 09)
```   
You can query driver infomation by lspci command as `lspci -vs 00:02.0`. It can tell you what driver is in use now.   
- `buildcommand 2>&1 | tee build.log` this command can save the build log to a local file at the same time output it 
 to the screen. A very useful command to analyse build errors.


