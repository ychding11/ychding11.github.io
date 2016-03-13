---
layout: post
title: "Linux Debug Summary" 
date: 2015-03-12
---

## core dump

### check core dump state
using following command to check default core file size.  
`ulimit -c` command is to query the default size of dumped core file size.
0 means core dump is currently not enabled.

### enable core dump
set default core file size to unlimited,the setting is active only in current terminal.  
`ulimit -c unlimited` command set core file size, unlimited indicating that dumped
core file size has no limitation.

### customizing core file name. 
configure file */proc/sys/kernel/core_pattern*  controls core file name convention.   
`echo "/tmp/corefiles/core-%e-%p-%t" >> /proc/sys/kernel/core_pattern`  
set a new pattern for generated core file. To ensure the command takes effects, use command  
`cat /proc/sys/kernel/core_pattern`   
to check. If it is correct the command will return exactly
what you set.  
	
+  **%e**  executable filename (without path prefix) 
+  **%p**  PID of dumped process, as seen in the PID namespace in which the process resides
+  **%t**  time of dump, expressed as seconds since the Epoch,1970-01-01 00:00:00 +0000 (UTC)
	
### Analyse core file
Rebuild code with debug info, add -g option for gcc. use the following command to open core file.

+ **gdb a.out /tmp/corefiles/core-a.out-12754-1457789593**

### reference
- [detailed core pattern setting](http://man7.org/linux/man-pages/man5/core.5.html) 
- [an introduce to core dump](http://www.cnblogs.com/hazir/p/linxu_core_dump.html)
