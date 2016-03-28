---
layout: post
title: "Linux Debug Summary" 
date: 2015-03-12
---

## core dump

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

### reference
- [Detailed core pattern setting](http://man7.org/linux/man-pages/man5/core.5.html) 
- [An introduce to core dump](http://www.cnblogs.com/hazir/p/linxu_core_dump.html)
- [How to write shared Libaries](https://www.akkadia.org/drepper/dsohowto.pdf)
