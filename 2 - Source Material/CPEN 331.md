**Type:** [[course]]  
**Topics:** #computer-engineering   
**Date:** 2024-08-30  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- 

---

# TODO

- catch up on [Readings](https://people.ece.ubc.ca/~os161/class-site/calendar.html) and lectures

# Synchronization Patterns

Using Atomic Instructions for Mutual Exclusion

```c
bool CAS(int old, void *p, int new) {
	if(*p != old) // Compare the value at location p with the expected old value
		return false;        // If not equal, return false (indicating failure)
	*p = new;               // If equal, set *p to new (atomic swap)
	return true;            // Return true (indicating success)
}
```

```ad-summary
title: CAS
Compare and swap
implemented in hardware
- `old`: old value at location `p`
- `*p`: Location you want to modify
- `new`: update to new value at location `p` for next time
```

Typical use of CAS to implement locks:

```c
bool lockObtained = false;
int locked;

retry:
/* Spin the lock looks taken */
	while(lock -> locked) ;

/* */
locked = lock->locked;
if(locked)
	goto retry;

lockObtained = CAS(locked, &lock->locked, 1);

if(!lockObtained)
	goto retry;
```
To Update 
```c

```

# Scheduling 

![[Screenshot 2024-10-01 at 11.39.59 AM.png]]


# [Assignment 1](https://sites.google.com/view/cpen331fall2024/assignments/assignment-1) Notes

### Step 1 – OS/161 Installation

```ad-note
title: How does `OS/161` run?
We are running `OS/161` **not** on real hardware, but on a *hardware simulator*, `System/161`.

A hardware simulator is a software tool that mimics the behavior of actual hardware. It allows the user to run tests, debug and develop software without the need for purchasing physical hardware.
```
### Step 4 – Copy some output from git commands into your submit file
1. First of all, let's make sure you can run System/161. In a shell, invoke sys161 with your newly built kernel (you should know how to do that if you have complete step 1) and copy the output of that command into your submit file.

Question 1: In the book chapters and in class you were introduced to the mechanisms used to transfer control between user processes and the operating system. Tell us where we can find the first line of OS/161 code that is executed when a trap occurs. Then tell us where control gets transferred to from that point. What about an interrupt? How does that differ?
	

Question 2: Making a system call, such as write, ultimately leads to a trap. Find the code in OS/161 that invokes system calls from user programs and causes traps. In which file and on which lines did you find this code?

Question 3: Why do you suppose there are libc functions in the "common" part of the source tree (common/libc) as well as in userland/lib/libc?

Below is a brief overview of the organization of the source tree, and a description of what goes where.

- configure -- top-level configuration script; configures the OS/161 distribution and build system. It does not configure the operating system kernel, which is a slightly different kind of configuration.
    

Question 4: Name two things that configure configures. What might invalidate that configuration and make you need/want to rerun it?

- Makefile -- top-level makefile; builds the OS/161 distribution, including all the provided utilities, but does not build the operating system kernel.
    
- common/ -- code used both by the kernel and user-level programs, mostly standard C library functions.
    
- kern/ -- the kernel source code.
    
- kern/Makefile -- Once again, there is a Makefile. This Makefile installs header files but does not build anything.
    
- kern/arch/ -- This is where architecture-specific code goes. By architecture-specific, we mean the code that differs depending on the hardware platform on which you're running. There are two directories here: mips which contains code specific to the MIPS processor and sys161 which contains code specific to the System/161 simulator.
    
- kern/arch/mips/conf/conf.arch -- This tells the kernel config script where to find the machine-specific, low-level functions it needs (throughout kern/arch/mips/*).
    
- kern/arch/mips/include/ -- This folder and its subdirectories include files for the machine-specific constants and functions.
    

Question 5: What are some of the details which would make a function "machine dependent"? Why might it be important to maintain this separation, instead of just putting all of the code in one function?

- kern/arch/mips/* -- The other directories contain source files for the machine-dependent code that the kernel needs to run. A lot of this code is in assembler and will seem very low level, but understanding how it all fits together will help you immensely on Assignment 2.
    

Question 6: How large is a trapframe? Why?

- kern/arch/sys161/conf/conf.arch -- Similar to mips/conf/conf.arch.
    
- kern/arch/sys161/include -- These files are include files for the System161-specific hardware, constants, and functions. machine-specific constants and functions.
    
- kern/compile -- This is where you build kernels. See below.
    

Question 7: Under what circumstances should you re-run the kern/conf/config script?

Question 8: Under what circumstances should you run bmake depend in kern/compile/DUMBVM?

Question 9: Under what circumstances should you run bmake or bmake install in kern/compile/DUMBVM?

- kern/dev -- This is where all the low level device management code is stored. Unless you are really interested, you can safely ignore most of this directory.
    
- kern/fs -- This is where the actual file system implementations go. The subdirectory sfs contains a simple default file system. You will augment this file system as part of Assignment 4, so we'll ask you more questions about it then. The subdirectory semfs contains a special-purpose file system that provides semaphores to user-level programs. We may ask you more questions about this later on, after we discuss in class what semaphores are.
    
- kern/include -- These are the include files that the kernel needs. The kern subdirectory contains include files that are visible not only to the operating system itself, but also to user-level programs. (Think about why it's named "kern" and where the files end up when installed.)
    
- kern/lib -- These are library routines used throughout the kernel, e.g., arrays, kernel printf, etc. Note: You can use these data structures as you implement your assignments in CS161. We strongly encourage you to look around and see what we've provided for you.
    
- kern/main -- This is where the kernel is initialized and where the kernel main function is implemented.
    

Question 10: When you booted your kernel, you found that there were several commands that you could issue to experiment with it. Explain exactly where and what you would have to do to add a command that printed out, "Hello world!"

- kern/proc -- This is where process support lives. You will write most of the code that goes here during Assignments 4 and 5.
    
- kern/synchprobs -- This is the directory that contains/will contain the framework code that you will need to complete assignment 1. You can safely ignore it for now.
    
- kern/syscall -- This is where you will add code to create and manage user level processes. As it stands now, OS/161 runs only kernel threads; there is no support for user level code. In Assignments 4 and 5, you'll implement this support.
    
- kern/thread -- Threads are the fundamental abstraction on which the kernel is built (do not forget to look back at header files!)
    
- kern/vfs -- The vfs is the "virtual file system." It is an abstraction for a file system and its existence in the kernel allows you to implement multiple file systems, while sharing as much code as possible. The VFS layer is the file-system independent layer. You will want to go look atvfs.h and vnode.h before looking at this directory.
    
- kern/vm -- This directory is fairly vacant. In Assignments 6 and 7, you'll implement virtual memory and most of your code will go in here.
    
- man/ -- the OS/161 manual ("man pages") appear here. The man pages document (or specify) every program, every function in the C library, and every system call. You will use the system call man pages for reference in the course of assignment 2. The man pages are HTML and can be read with any browser.
    
- mk/ -- fragments of makefiles used to build the system.
    
- userland/ -- user-level libraries and program code
    
- userland/bin/ -- all the utilities that are typically found in /bin, e.g., cat, cp, ls, etc. The things in bin are considered "fundamental" utilities that the system needs to run.
    

Question 11: Why do we need to include these in your OS/161 distribution? Why can't you just use the standard utilities that are present on the machine on which you're working?

- userland/include/ -- these are the include files that you would typically find in /usr/include (in our case, a subset of them). These are user level include files; not kernel include files.
    
- userland/lib/ -- library code lives here. We have only two libraries: libc, the C standard library, and hostcompat, which is for recompiling OS/161 programs for the host UNIX system. There is also a crt0 directory, which contains the startup code for user programs.
    

Question 12: When a user program exits, what is done with the program's return value?

- userland/sbin/ -- this is the source code for the utilities typically found in /sbin on a typical UNIX installation. In our case, there are some utilities that let you halt the machine, power it off and reboot it, among other things.
    
- userland/testbin/ -- this is the source code for the test programs found in /testbin in the installed OS/161 tree. You will want to examine this directory closely and be aware of what's available here, as many of these test programs will be useful during the course of the semester.
    

Now that you've perused the source tree, here is the last question.

Question 13: Imagine that you wanted to add a new system call. List all the places that you would need to modify/add code. Then review your answers to questions 7-9 and note which of those actions you need to take in order to test the new system call.

---

Question 1: In the book chapters and in class you were introduced to the mechanisms used to transfer control between user processes and the operating system. Tell us where we can find the first line of OS/161 code that is executed when a trap occurs. Then tell us where control gets transferred to from that point. What about an interrupt? How does that differ?

The mechanism used to transfer control between user processes and the operating system is known as a **trap**. 

The first line of OS/161 code that is executed when a trap occurs can be found in `kern/arch/mips/locore/exception-mips1.S`. The line

```
j common_exception
nop
```

appears for both `UTLB exception handler` and `General exception handler`. This transfers control to `common_exception` which is the handler for all traps; and from there, control is transferred to the C function `mips_trap` in `kern/arch/mips/locore/trap.c`, which is used to process and identify the type of trap.

Looking through the `mips_trap` function, we see the comment 

```c
/* Interrupt? Call the interrupt handler and return. */
```

So we can see that through the trap handling, it checks to see if the trap was caused by an interrupt; if so, then there is a separate interrupt handler. This leads to the final question how interrupts differ from traps. Well for starters, the OS handles traps and interrupts differently. Interrupts have a special interrupt handler, while other traps are processed in the rest of `mips_trap`. We see it check what trap it is in steps: syscall, vm_fault, fatal fault. So the main difference is that general traps are handled differently than interrupts.

Question 2: Making a system call, such as write, ultimately leads to a trap. Find the code in OS/161 that invokes system calls from user programs and causes traps. In which file and on which lines did you find this code?

As touched upon in the last question we know that making a system call, like `write`, ultimately leads to `kern/arch/mips/locore/trap.c`, to the general trap handling function `mips_trap`. Specifically, for `syscall` traps we have this segment of code:

```c
/* Syscall? Call the syscall handler and return. */
if (code == EX_SYS) {
	/* Interrupts should have been on while in user mode. */
	KASSERT(curthread->t_curspl == 0);
	KASSERT(curthread->t_iplhigh_count == 0);
	DEBUG(DB_SYSCALL, "syscall: #%d, args %x %x %x %x\n",
	tf->tf_v0, tf->tf_a0, tf->tf_a1, tf->tf_a2, tf->tf_a3);
	syscall(tf);
	goto done;
}
```

which indicates that it will transfer control to a `syscall handler`. Specifically, the `syscall` function (or handler) which takes in the `trapframe` struct as an input gets invoked at line 224.

Question 3: Why do you suppose there are libc functions in the "common" part of the source tree (common/libc) as well as in userland/lib/libc?

First, let's look at what's in either of the `libc` directories. 

```sh
# ls in common
arch   printf stdlib string
# ls in userland
Makefile arch     stdio    stdlib   string   syscalls time     unix
```

Notice that the list is not the same for both. `libc` in `common` contains C library code which is shared between kernel and userland. This means the functions in their are something needed by **both** the kernel and the user. Placing these functions useful for both user and kernel is to reduce redundancy (no repeated code). The functions in userland are specific to user-level programs, and the kernel has no use for them. So overall, this set up of `libc` is to reduce redundancy, and help keep the kernel lightweight, ensuring it only has everything it needs (and not more).

Question 4: Name two things that configure configures. What might invalidate that configuration and make you need/want to rerun it?

Two things that configure configures:
1. The location of the OS161 build tree: The configure scripts set up environment variables such as `OSTREE`, which defines the default path where OS/161 will install its build tree.
2. System and machine configurations: the script detects the current system information (e.g., CPU architecture, system libraries, and compilers) and passes these to the makefiles, which ensures that the build process is tailored to the specific environment.

Things that would make you want to rerun it:
1. consider working on OS161 on a different system. Obviously we would need to re run the configuration for the current system as the information so that OS161 is compatible.
2. Another reason to re run it would be if you make any changes to the build environment, so that the build settings can be updated accordingly (ex: changing the location of the build tree).

Question 5: What are some of the details which would make a function "machine dependent"? Why might it be important to maintain this separation, instead of just putting all of the code in one function?

Well, let's consider how different machines process information. High-level code will be converted to low-level assembly language. The low-level assembly will be machine dependent, (ARM, MIPS, etc.) and things like endianness, size of primitives (for the system), number of registers, which register is used as the link register – all these things are machine dependent. So it's really important to maintain this separation between machine-dependent and independent code. Isolating between machine-dependent and independent code allows for the system to be portable to different machines. Also, when switching machines, we only need to re run the configuration, the machine-dependent part of the code, which saves us time since we only need to recompile a portion rather than the whole code base.

Question 6: How large is a trapframe? Why?

in `kern/arch/mips/include/kern/trapframe.h` we can see how the `trapframe` struct is defined:

```c
struct trapframe {
	uint32_t tf_vaddr; /* coprocessor 0 vaddr register */
	uint32_t tf_status; /* coprocessor 0 status register */
	uint32_t tf_cause; /* coprocessor 0 cause register */
	uint32_t tf_lo;
	uint32_t tf_hi;
	uint32_t tf_ra; /* Saved register 31 */
	uint32_t tf_at; /* Saved register 1 (AT) */
	uint32_t tf_v0; /* Saved register 2 (v0) */
	uint32_t tf_v1; /* etc. */
	uint32_t tf_a0;
	uint32_t tf_a1;
	uint32_t tf_a2;
	uint32_t tf_a3;
	uint32_t tf_t0;
	uint32_t tf_t1;
	uint32_t tf_t2;
	uint32_t tf_t3;
	uint32_t tf_t4;
	uint32_t tf_t5;
	uint32_t tf_t6;
	uint32_t tf_t7;
	uint32_t tf_s0;
	uint32_t tf_s1;
	uint32_t tf_s2;
	uint32_t tf_s3;
	uint32_t tf_s4;
	uint32_t tf_s5;
	uint32_t tf_s6;
	uint32_t tf_s7;
	uint32_t tf_t8;
	uint32_t tf_t9;
	uint32_t tf_k0; /* dummy (see exception-mips1.S comments) */
	uint32_t tf_k1; /* dummy */
	uint32_t tf_gp;
	uint32_t tf_sp;
	uint32_t tf_s8;
	uint32_t tf_epc; /* coprocessor 0 epc register */
};
```

So the `trapframe` struct has 37 `uint32_t` fields – each one representing a register – 37 times 32 bits is 37 times 4 bytes = 148 bytes. The trapframe is this size in order to save the state of the processor when a trap occurs (it does this by saving the value in all registers). This allows the system to restore the state after the trap handling. The trap frame is also imperative when processing in `mips_trap`, as it needs information on the registers to find out what caused the trap.

Question 7: Under what circumstances should you re-run the kern/conf/config script?

`kern/conf/config` script should be re-run whenever you modify the kernel configuration, change architectures, add new source files, switch between kernel builds, or need to fix configuration issues.

Question 8: Under what circumstances should you run bmake depend in kern/compile/DUMBVM?

`bmake depend` should be run when modifying header files, adding or removing source files, switching kernel configurations, or making changes that affect file dependencies.

Question 9: Under what circumstances should you run bmake or bmake install in kern/compile/DUMBVM?

`bmake` should be run after modifying kernel code (C files), changing configuration, or updating dependencies. `bmake install` should be run after compiling the kernel to install the new version and prepare it for booting in the simulator.

Question 10: When you booted your kernel, you found that there were several commands that you could issue to experiment with it. Explain exactly where and what you would have to do to add a command that printed out, "Hello world!"

I will outline the steps I took below:
1. First we locate the code for the kernel menu commands. This is in the file `kern/main/menu.c`
2. Next we write a new command function for helloworld; following the convention of the other commands, I called it `cmd_helloworld`
3. Then we add the command to the `cmdtable[]` array so that the kernel recognizes the command, as well as `opstable[]` so the users are aware of the command when `?o` is typed.
4. Finally, we need to rebuild the kernel since we changed some source code.

end result:

```sh
OS/161 kernel menu
    [?o] Operations menu                [khgen] Next kernel heap generation
    [?t] Tests menu                     [khdump] Dump kernel heap
    [kh] Kernel heap stats              [q] Quit and shut down

Operation took 0.481954360 seconds
OS/161 kernel [? for menu]: ?o

OS/161 operations menu
    [s]       Shell                     [cd]      Change directory
    [p]       Other program             [pwd]     Print current directory
    [mount]   Mount a filesystem        [sync]    Sync filesystems
    [unmount] Unmount a filesystem      [panic]   Intentional panic
    [bootfs]  Set "boot" filesystem     [q]       Quit and shut down
    [pf]      Print a file              [helloworld]  Print Hello world

Operation took 0.947934320 seconds
OS/161 kernel [? for menu]: helloworld
Hello world!
Operation took 0.017312320 seconds
OS/161 kernel [? for menu]:
```

Question 11: Why do we need to include these in your OS/161 distribution? Why can't you just use the standard utilities that are present on the machine on which you're working?

My thinking is that OS/161 is running in a virtual environment called System/161. We want to isolate it from using any host utilities and features, so as to replicate the experience of building our own OS, which is why we need to include these utilities in our OS/161 distribution. So, to achieve this isolation, OS/161 needs to have its own set of utilities (like libraries and programs), rather than relying on the host system's utilities.

Question 12: When a user program exits, what is done with the program's return value?

When a user program in OS/161 exits, its return value is passed to the kernel via the `exit()` system call. Looking at the file `userland/lib/crt0/mips/crt0.S` there's a helpful comment:

```c
/*
* Now, we have the return value of main in v0.
*
* Move it to s0 (which is callee-save) so we still have
* it in case exit() returns.
*
* Also move it to a0 so it's the argument to exit.
*/
```

The return value, stored in the `v0` register after `main()` completes, is moved to the `a0` register and passed as an argument to `exit()`. The kernel then stores this return value, also called the `exit status`. With this exit status, it's usually used to indicate how a program terminated (success, failure, seg fault, etc.).

Question 13: Imagine that you wanted to add a new system call. List all the places that you would need to modify/add code. Then review your answers to questions 7-9 and note which of those actions you need to take in order to test the new system call.

Steps:
1. Define a unique system call number as a macro in `src/kern/include/kern/syscall.h`
2. Add a function prototype for the system call in `src/kern/include/syscall.h`
3. Add the new system call to the system call handler in `src/kern/arch/mips/syscall/syscall.c`
4. Create a new file in `src/kern/syscall` with your `newsyscall.c` function.

Once that is done, we need to
1. rerun the config script using 
```sh
cd kern/conf
./config DUMBVM
```

2. run `bmake depend` because a new file has been added
3. rebuild the kernel with the syscall with
```
bmake
bmake install
```

## step 7

Question 14: What is the name of the very first function that executes when OS161 starts up?

the very first function that executes when OS161 starts up is `__start () at ../../arch/sys161/main/start.S:54`

Question 15: What is the very first assembly instruction that executes?
The assembly instruction `addiu sp, sp, -24` at line 54 of `start.S`

Question 16: Set the breakpoints in the kernel function that shows the menu and in the kernel main function. Now tell GDB to display all the breakpoints that were set and copy the output to your submit file.

Please note that my breakpoint location in `menu.c` might be different since I implemented the `helloworld` command.

```sh
(gdb) info break
Num     Type           Disp Enb Address    What
1       breakpoint     keep y   0x800139e4 in kmain
                                           at ../../main/main.c:211
2       breakpoint     keep y   0x80014a50 in menu
                                           at ../../main/menu.c:714
```

Question 17: Briefly describe what happens between the beginning of the execution and the invocation of the kernel main function.

using `disassemble __start` we can see the assembly up until `kmain`.

```s
(gdb) disassemble __start
Dump of assembler code for function __start:
   0x8002d730 <+0>:	addiu	sp,sp,-24
   0x8002d734 <+4>:	sw	ra,20(sp)
   0x8002d738 <+8>:	lui	s0,0x8003
   0x8002d73c <+12>:	addiu	s0,s0,23728
   0x8002d740 <+16>:	move	a1,a0
   0x8002d744 <+20>:	move	a0,s0
   0x8002d748 <+24>:	jal	0x80001d30 <strcpy>
   0x8002d74c <+28>:	nop
   0x8002d750 <+32>:	move	a0,s0
   0x8002d754 <+36>:	jal	0x80001de0 <strlen>
   0x8002d758 <+40>:	nop
   0x8002d75c <+44>:	add	t0,s0,v0
   0x8002d760 <+48>:	addi	t0,t0,1
   0x8002d764 <+52>:	addi	t0,t0,4095
   0x8002d768 <+56>:	li	t1,-4096
   0x8002d76c <+60>:	and	t0,t0,t1
   0x8002d770 <+64>:	addi	t0,t0,4096
   0x8002d774 <+68>:	move	sp,t0
   0x8002d778 <+72>:	lui	at,0x8003
   0x8002d77c <+76>:	sw	t0,16960(at)
   0x8002d780 <+80>:	addiu	sp,sp,-24
   0x8002d784 <+84>:	sw	zero,20(sp)
   0x8002d788 <+88>:	lui	a0,0x8000
   0x8002d78c <+92>:	lui	a1,0x8003
   0x8002d790 <+96>:	addiu	a1,a1,-12656
   0x8002d794 <+100>:	lui	a2,0x8003
   0x8002d798 <+104>:	addiu	a2,a2,-12648
   0x8002d79c <+108>:	sub	a2,a2,a1
   0x8002d7a0 <+112>:	jal	0x80001850 <memmove>
   0x8002d7a4 <+116>:	nop
   0x8002d7a8 <+120>:	lui	a0,0x8000
   0x8002d7ac <+124>:	ori	a0,a0,0x80
   0x8002d7b0 <+128>:	lui	a1,0x8003
   0x8002d7b4 <+132>:	addiu	a1,a1,-12648
   0x8002d7b8 <+136>:	lui	a2,0x8003
   0x8002d7bc <+140>:	addiu	a2,a2,-12640
   0x8002d7c0 <+144>:	sub	a2,a2,a1
---Type <return> to continue, or q <return> to quit---
   0x8002d7c4 <+148>:	jal	0x80001850 <memmove>
   0x8002d7c8 <+152>:	nop
   0x8002d7cc <+156>:	jal	0x8002ce80 <mips_flushicache>
   0x8002d7d0 <+160>:	nop
   0x8002d7d4 <+164>:	jal	0x8002d134 <tlb_reset>
   0x8002d7d8 <+168>:	nop
   0x8002d7dc <+172>:	li	s7,0
   0x8002d7e0 <+176>:	li	t0,0xff00
   0x8002d7e4 <+180>:	mtc0	t0,c0_sr
   0x8002d7e8 <+184>:	mtc0	zero,c0_context
   0x8002d7ec <+188>:	lui	gp,0x8004
   0x8002d7f0 <+192>:	addiu	gp,gp,-16048
   0x8002d7f4 <+196>:	jal	0x800139d0 <kmain>
   0x8002d7f8 <+200>:	move	a0,s0
   0x8002d7fc <+204>:	lui	a0,0x8003
   0x8002d800 <+208>:	addiu	a0,a0,16144
   0x8002d804 <+212>:	jal	0x80012bc8 <panic>
   0x8002d808 <+216>:	nop
   0x8002d80c <+220>:	j	0x8002d7fc <__start+204>
   0x8002d810 <+224>:	nop
End of assembler dump.
```

Thus we can trace and see that the following things happen:

1. At the beginning of `__start`, we see it does a low-level system setup, including stack alignment, copying memory, and setting up kernel data structures.
2. Next, the function calls `mips_flushicache()` and `tlb_reset()` to flush the instruction cache and reset the TLB. 
3. The `jal kmain` instruction is where we jump to `kmain`, which is where higher-level kernel initialization begins.

Question 18: What is the assembly language instruction that calls the kernel main function?

`jal kmain`, as indicated in the last question.

Question 19: Step through the boot() code to find out what functions are called during early initialization. Paste the gdb output that shows you what these functions are.

Using GDB we can see some functions that are called during early initialization is
- `kprintf`
- `ram_bootstrap`
- `proc_bootstrap`
- `thread_bootstrap`
- `hardclock_bootstrap`
- `vfs_bootstrap`
- `kheap_nextgeneration`

```sh
Breakpoint 8, boot () at ../../main/main.c:99
99		kprintf("\n");
(gdb) next
100		kprintf("OS/161 base system version %s\n", BASE_VERSION);
(gdb) next
101		kprintf("%s", harvard_copyright);
(gdb) next
102		kprintf("\n");
(gdb) next
104		kprintf("Put-your-group-name-here's system version %s (%s #%d)\n",
(gdb) next
106		kprintf("\n");
(gdb) next
109		ram_bootstrap();
(gdb) next
110		proc_bootstrap();
(gdb) next
111		thread_bootstrap();
(gdb) next
112		hardclock_bootstrap();
(gdb) next
113		vfs_bootstrap();
(gdb) next
114		kheap_nextgeneration();
(gdb) next
117		kprintf("Device probe...\n");
(gdb) next
```

Question 20: Set a breakpoint in thread_bootstrap(). Once you hit that breakpoint, at the very first line of that function, attempt to print the contents of the bootcpu variable. Copy the output into the submit file.

```sh
(gdb) b thread_bootstrap
Breakpoint 1 at 0x8001fad8: file ../../thread/thread.c, line 357.
(gdb) continue
Continuing.

Breakpoint 1, thread_bootstrap () at ../../thread/thread.c:357
357		cpuarray_init(&allcpus);
(gdb) print *bootcpu
Cannot access memory at address 0x80000
(gdb)
```

As we can see, we get `Cannot access memory at address 0x80000` when trying to print the contents immediately.

```sh
$1 = {c_self = 0x8003af00, c_number = 0, c_hardware_number = 0,
  c_curthread = 0x8003bf80, c_zombies = {tl_head = {
      tln_prev = 0x0, tln_next = 0x8003af1c, tln_self = 0x0},
    tl_tail = {tln_prev = 0x8003af10, tln_next = 0x0,
      tln_self = 0x0}, tl_count = 0}, c_hardclocks = 0,
  c_spinlocks = 0, c_isidle = false, c_runqueue = {tl_head = {
      tln_prev = 0x0, tln_next = 0x8003af44, tln_self = 0x0},
    tl_tail = {tln_prev = 0x8003af38, tln_next = 0x0,
      tln_self = 0x0}, tl_count = 0}, c_runqueue_lock = {
    splk_lock = 0, splk_holder = 0x0}, c_ipi_pending = 0,
  c_shootdown = {{ts_placeholder = 0} <repeats 16 times>},
  c_numshootdown = 0, c_ipi_lock = {splk_lock = 0,
    splk_holder = 0x0}}
```

Question 22: Print the allcpus array before the boot() function is executed. Paste the output.

`0 cpus`

Question 23: Print again the same array after the boot() function is executed. Paste the output.  

```sh
1 cpus
cpu 0:
$1 = {c_self = 0x8003af00, c_number = 0, c_hardware_number = 0,
  c_curthread = 0x8003bf80, c_zombies = {tl_head = {
      tln_prev = 0x0, tln_next = 0x8003af1c, tln_self = 0x0},
    tl_tail = {tln_prev = 0x8003af10, tln_next = 0x0,
      tln_self = 0x0}, tl_count = 0}, c_hardclocks = 0,
  c_spinlocks = 0, c_isidle = false, c_runqueue = {tl_head = {
      tln_prev = 0x0, tln_next = 0x8003af44, tln_self = 0x0},
    tl_tail = {tln_prev = 0x8003af38, tln_next = 0x0,
      tln_self = 0x0}, tl_count = 0}, c_runqueue_lock = {
    splk_lock = 0, splk_holder = 0x0}, c_ipi_pending = 0,
  c_shootdown = {{ts_placeholder = 0} <repeats 16 times>},
  c_numshootdown = 0, c_ipi_lock = {splk_lock = 0,
    splk_holder = 0x0}}
```

---

### High-Level Plan

1. **Data Structures for VM System**:
    
    - **Core Map or Frame Table**: To keep track of physical memory usage on a page-by-page basis. This replaces the old `ram_stealmem()` approach. The core map will store information about which frames are free, which frames correspond to which virtual pages, and possibly metadata like dirty bits or reference counts.
    - **Page Table**: Each address space will need a page table that records where each virtual page of that process is located—either in memory (and at which frame) or on disk (and at which swap offset). Unlike dumbvm, you will not have a simple base+offset calculation; you must do lookups per page.
    - **Swap Mechanism**: Pages can be evicted to disk. You may want a bitmap or structure to track which disk slots are used to store swapped-out pages.
2. **Kernel Initialization**:
    
    - **vm_bootstrap()**: Initialize your core map (frame table). This involves:
        - Using `ram_getsize()` and `ram_getfirstfree()` to find the range of physical memory available.
        - Dividing this memory into pages and setting up a core map structure that marks which pages are free or allocated.
3. **Allocating and Freeing Pages**:
    
    - Replace `alloc_kpages()` and `free_kpages()` with implementations that:
        - Use the core map to find free physical pages.
        - Mark pages as allocated and return a kernel virtual address to them.
        - On `free_kpages()`, mark pages as free again.
    - Initially, just ensure that `kmalloc()` and `kfree()` (which rely on `alloc_kpages()` and `free_kpages()` for large allocations) work correctly. Get the `km1-km4` tests passing first. This ensures basic memory allocation is stable and returning memory works.
4. **On-Demand Paging and Page Tables**:
    
    - Instead of allocating all pages for a process at load time, allocate them on-demand during `vm_fault()`:
        - When a TLB miss or page fault occurs, determine if the page is in memory, on disk, or needs to be allocated fresh (e.g., if it's a stack page that has just grown).
        - If the page is not in memory, you must:
            1. Find a free frame (or evict one if necessary).
            2. If the page is from a segment that should initially be loaded from the executable file, read it in from the ELF file or zero it if it's a BSS area.
            3. If the page was previously swapped out, read it from the swap disk.
            4. Update the page table entry to record the frame number and its state.
            5. Load a TLB entry.
5. **TLB Management**:
    
    - Implement `vm_fault()`: On a TLB miss, check the process's page table.
        - If the page is in memory, just write a TLB entry.
        - If not in memory, follow the steps above to bring it in.
    - Handle `VM_FAULT_READONLY`: If a write occurs to a read-only page, you must address that (possibly by marking the page read-write if the ELF segment allows it).
    - Implement TLB shootdowns if you evict a page that might still have a TLB entry in another CPU’s TLB (in a multi-processor setting). For single-core environments, a full TLB flush on context switch and selective invalidations is sufficient.
6. **Eviction Policy**:
    
    - Decide how to pick a victim page to evict when memory is full. Common strategies:
        - FIFO: Evict the oldest loaded page.
        - Clock/Second-Chance: Evict a page that has not been referenced recently (would require reference bits).
        - LRU approximation: Similar to Clock, as real LRU is complex.
    - Implement the chosen algorithm by:
        - Keeping a circular list or pointer into your frame table.
        - On eviction, if a page is dirty and has no valid backing on disk, write it to swap space first.
        - Update the page table entry of the evicted page to reflect that it is now on disk and not in memory.
        - Invalidate its TLB entry (TLB shootdown).
7. **Adding `sbrk()` for Dynamic Memory**:
    
    - `sbrk()` changes the size of the heap region.
    - Implement logic in `as_define_region()` and possibly maintain a special region in the address space for the heap.
    - When `sbrk()` extends the heap, adjust the address space metadata to represent the new virtual address range. Actual physical pages for the heap are allocated only when accessed (on page fault).
    - Ensure that `malloc()` (in user space) now works properly using `sbrk()`.
8. **Fork Support (`as_copy`)**:
    
    - Implement `as_copy()` to duplicate the address space:
        - Create a new address space and replicate its regions.
        - For each virtual page currently in memory, allocate a new frame in the child and copy the contents. (For this assignment, you are not required to implement copy-on-write.)
        - Update the child's page table accordingly.
    - Make sure that after `as_copy()`, both the parent and child can run without interfering with each other’s data.
9. **Swapping to Disk**:
    
    - Open a vnode for the raw disk device (e.g., `lhd0raw:`).
    - Develop a mapping scheme that maps pages to slots on disk. A simple approach:
        - Keep a free list of swap slots.
        - When you evict a page, choose a free slot and write it out.
        - To bring it back in, read from that slot.
    - Ensure synchronization while doing I/O (disk I/O should be performed carefully, consider using locks).
10. **Testing and Debugging**:
    
    - Use `dbflags = DB_VM` in GDB to turn on VM subsystem debugging messages.
    - Be cautious with `kprintf()` inside `vm_fault()`, as it can cause re-entrancy and complexity. Prefer using the debugger, breakpoints, and `print` commands in GDB.
    - Use `os161-addr2line` to map kernel instruction addresses to source code lines for debugging exceptions.
    - Use the `CHECKBEEF` mode in kmalloc for detecting invalid memory writes.

### Incremental Approach

**Stage 1:**

- Implement a core map.
- Make `alloc_kpages()` and `free_kpages()` return memory properly to the free pool.
- Test with `km1-km4` until stable.

**Stage 2:**

- Implement a basic page table and on-demand allocation for pages (no swapping yet).
- Ensure that TLB misses for new pages allocate frames as needed.
- Add `sbrk()` and ensure user programs that use `malloc()` run correctly.

**Stage 3:**

- Implement TLB eviction when TLB is full.
- Implement address space copying (`as_copy()`) so that `fork()` works.

**Stage 4:**

- Add swap disk management and page eviction policy.
- Test with more memory-intensive programs and ensure that swapping works (pages get evicted and reloaded correctly).

**Stage 5:**

- Evaluate and tune your replacement algorithms and TLB handling for performance.
  
  Virtual Address -> TLB Check
  If hit: return physical address immediately
  If miss: continue to page table lookup

Virtual Address -> Page Table Lookup
  If valid: add to TLB and return physical address
  If invalid: page fault handler is called
    - Allocate physical frame from frame table
    - Load page from disk/zero-fill
    - Update page table entry
    - Retry the access

Frame Table: Tracks physical memory frames
- One entry per physical frame
- Used by OS to manage physical memory allocation
- Helps implement page replacement
- Global resource for whole system

Page Table: Maps virtual to physical addresses
- One page table (or set of tables) per process
- Used for address translation
- Tracks permissions and status per virtual page


Main responsibilities of an OS

Virtualization. Abstraction of detail. A user program can access files in the same way regardless of the disk model and implementation.

Protection. Making sure each user can only access their own programs, their own files, etc.

Resource management. Allowing for proper sharing of physical resources (memory) things like threads and processes 

Each running program is represented as a process in the kernel. The process has its own state (program counter, registers, memory open files,) a process runs for a while, then the system is interrupted by a timer interrupt. The kernel performs a context switch, saves the state of the process to memory. Loads the state of the new process to the cpu.

- Same virtual addresses would always map to different physical addresses

processor loads a virtual address which gets translate to physical address (MMU)


```c
/*

* Structure for open files.

*

* This is pretty much just a wrapper around a vnode; the important

* additional things we keep here are the open mode and the file's

* seek position.

*

* Open files are reference-counted because they get shared via fork

* and dup2 calls. And they need locking because that sharing can be

* among multiple concurrent processes.

*/

struct openfile {

struct vnode *of_vnode;

int of_accmode; /* from open: O_RDONLY, O_WRONLY, or O_RDWR */

  

struct lock *of_offsetlock; /* lock for of_offset */

off_t of_offset;

  

struct spinlock of_reflock; /* lock for of_refcount */

int of_refcount;

};
```

multiple processes may be writing to the same openfile which is why we need a reference count for an open file and an offset to coordinate with multiple processes writing to the same file (fork and dup2) 



