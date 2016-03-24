---
layout: post
title: "Usefull Linux Commands" 
date: 2015-03-16
---
- `pkg-config --static --libs glfw3`  tell you how to link glfw3 lib in correct order  
- `objdump -t a.out` display symbol table of elf file a.out
- `objdump -h a.out` display sections of elf file a.out
- `readelf -p .comment ./a.out` read .comment section info from elf file a.out
- `ls | wc -l` tell how many files in current directory 
- `ls -l | grep ^d` list directory items in current directory
- `lspci | grep -i vga` query graphic cards info   
```
00:02.0 VGA compatible controller: Intel Corporation 3rd Gen Core processor Graphics Controller (rev 09)
```   
You can query driver infomation by lspci command as `lspci -vs 00:02.0`. It can tell you what driver is in use now.   
- `buildcommand 2>&1 | tee build.log` this command can save the build log to a local file at the same time output it 
 to the screen. A very useful command to analyse build errors.
 
## Search and Replace

- `locate -b '\qmake'` find qmake path using exact name. Another equal command is `locate -r /qmake$` here *r* 
does not mean recursive.
- `find /home/ding/ -type f` Find regular files in directory /home/ding. -type option specifies file type. 
f equals to regular file, d equals to directory.  
- `find ./misc/ -exec file '{}' \;` Execute file command to every file in directory ./misc. -exec option 
specifies the command to execute, ';' indicates the ending of command. \ is to escape in case of shell 
interpret it as another meaning. {} stands for the current processing file.   
- `grep -i question -r ./` Search all files recursively in current directory to find lines containing key word *question*.  
Another grep command `grep -i question -rl ./`, it can list all files in current directory containing the key 
word *question*.  
- `sed -i 's/Do_Movement/cameraMovement/g' ./shadow_mapping.cpp` Find and replace Do_Movement with cameraMovement 
in file shadow_mapping.cpp. -i option tells sed to use inplace mode, no temp file created.  
- *sed* and *grep* can cooperate. That is *grep* can suply the file list containing the specified pattern while 
*sed* edit those files one by one. for example: sed -i 's/pattern/newstr/g' `grep -rl pattern ./`.
