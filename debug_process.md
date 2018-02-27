
## Compile using Eclipse 
========================

-- --

## Flash and Debug using OpenOCD 
================================
Launch OpenOCD which opens a 
	telnet tcp port number 444 
and a 
	gdb tcp port number 3333

*Launching telnet is optional as all the OpenOcd command lines can be launched via gdb.*

*This is done by adding the word 'monitor' before the command line.*

### Launch and connect

Term 1 :	
```bash
cd ~/workspace/STM32Toolchain/openocd/scripts
../bin/openocd -f board/st_nucleo_f4.cfg
```

Term 2 : Launching telnet is optional !
```bash
telnet localhost 4444
```

Term 3 : 
```bash
arm-none-eabi-gdb
(gdb) target remote localhost:3333 /*connect to the remote target*/
```

### Flash firmware 

Term 3 : 
```bash
	(gdb) monitor reset init
	(gdb) monitor flash write_image erase <path to the .elf file>
	(gdb) monitor reset halt 
```
    or simply 
	`(gdb) monitor reset `

Term 2 : 
```bash
	> reset init
	> flash write_image erase "<path to the .elf file>"
	> reset halt 
	or simply 
	> reset 
```

### Debugging

**Cheat Sheat**
quit (q) quit gdb

run(r) launch the program. *This is not supported in nucleo remote debug*

break,watch,clear,delete(b,w,cl,d) put a breakpoint, watch a variable

step,next,continue(s,n,c) 

print,backtrace,list(p,bt,l) print a variable value, print the execution stack, print the current code location

#### Attach the elf file 
```bash 
(gdb) file *path to the .elf file*
```
**example**
```bash 
(gdb) file /home/alikara/workspace/hello-nucleo/Debug/hello-nucleo.elf
```

#### Simple Breakpoint

**At some line**

```bash 
(gdb) break sourcefile:linenumber
```
**example**
```bash 
(gdb) break /home/alikara/workspace/hello-nucleo/src/main.c:93
(gdb) continue
```

#### Print a variable

**The variable should be in the scope**

```bash 
(gdb) print var_name
```
**example**
```bash 
(gdb) continue 
(gdb) print seconds
(gdb) continue
```

#### Perform an action at a breakpoint

**The variable should be in the scope**

```bash 
	(gdb) commands breakpoint_number
```
**example to print a variable online**
```bash 
b ../src/main.c:95
Breakpoint 4 at 0x80011ba: file ../src/main.c, line 95.
(gdb) commands 4
Type commands for breakpoint(s) 4, one per line.
End with a line saying just "end".
>print a
>print seconds
>continue
>end 
(gdb) c
Continuing.

Breakpoint 4, main (argc=<optimized out>, argv=<optimized out>) at ../src/main.c:96
96	      ++seconds;
$10 = 43
$11 = 43

Breakpoint 4, main (argc=<optimized out>, argv=<optimized out>) at ../src/main.c:96
96	      ++seconds;
$12 = 44
$13 = 44

Breakpoint 4, main (argc=<optimized out>, argv=<optimized out>) at ../src/main.c:96
96	      ++seconds;
$14 = 45
$15 = 45

```








