
**Question 1** What are the ELF magic numbers?

>The ELF magic numbers - a unique identifier for the OS to understand that this is an ELF file - are: 
>
>`ELFMAG0` with a value of `0x7f` (equivalent to the `DEL` character)
>`ELFMAG1` with a value of `E` (note that `'E'` is equivalent to `0x45`)
>`ELFMAG2` with a value of `L`
>`ELFMAG3` with a value of `F`
>
>These ELF magic numbers are placed in a sequence at the beginning of every ELF file, so that the kernel can identify the file as an ELF file.

Looking in the file header file `elf.h` we can find the following lines which confirm this.

```c
/* For e_ident[EI_MAG0..3] */
#define ELFMAG0 0x7f
#define ELFMAG1 'E'
#define ELFMAG2 'L'
#define ELFMAG3 'F'
```

In `kern/syscall/loadelf.c` we find the function `load_elf` which loads an ELF executable from user space into the current address space.

```c
/*
* Load an ELF executable user program into the current address space.
*
* Returns the entry point (initial PC) for the program in ENTRYPOINT.
*/
int
load_elf(struct vnode *v, vaddr_t *entrypoint)
{

	...
	
	/*
	* Check to make sure it's a 32-bit ELF-version-1 executable
	* for our processor type. If it's not, we can't run it.
	*
	* Ignore EI_OSABI and EI_ABIVERSION - properly, we should
	* define our own, but that would require tinkering with the
	* linker to have it emit our magic numbers instead of the
	* default ones. (If the linker even supports these fields,
	* which were not in the original elf spec.)
	*/
	
	if (eh.e_ident[EI_MAG0] != ELFMAG0 ||
		eh.e_ident[EI_MAG1] != ELFMAG1 ||
		eh.e_ident[EI_MAG2] != ELFMAG2 ||
		eh.e_ident[EI_MAG3] != ELFMAG3 ||
		eh.e_ident[EI_CLASS] != ELFCLASS32 ||
		eh.e_ident[EI_DATA] != ELFDATA2MSB ||
		eh.e_ident[EI_VERSION] != EV_CURRENT ||
		eh.e_version != EV_CURRENT ||
		eh.e_type!=ET_EXEC ||
		eh.e_machine!=EM_MACHINE) {
		return ENOEXEC;
	}
	
	...

}
```

So in fact we can see that whenever we load a potential ELF file, it checks to see if the first 4 bytes are the ELF magic numbers before proceeding with loading.

**Question 2** What is the difference between `UIO_USERISPACE` and `UIO_USERSPACE`? When should one use `UIO_SYSSPACE` instead?

> `UIO_USERISPACE`, `UIO_USERSPACE` and `UIO_SYSSPACE` are possible flags for the `uio_segflg` field in the `uio` struct. `UIO_USERISPACE` is used to indicate that the data transfer a user process's **instruction space**, where executable code resides. Thus, the `UIO_USERISPACE` flag is used when loading an executable into memory. `UIO_USERSPACE` is a flag that indicates the data transfer is from or to the user process's data space, typically used when moving data regions within user memory. `UIO_SYSSPACE` is when the source or destination of the data transfer is within kernel memory. This is an important flag because we must ensure a clear divide from kernel and user space, mainly for security reasons. 

First we take a look at the header file `uio.h` to see if we can get some useful information from the comments. Mainly, let's understand what `uio` is and how it generally operates.

```c
/* Source/destination. */
enum uio_seg {
	UIO_USERISPACE, /* User process code. */
	UIO_USERSPACE, /* User process data. */
	UIO_SYSSPACE, /* Kernel. */
};

struct uio {
	...
	enum uio_seg uio_segflg; /* What kind of pointer we have */
	...
};
```

By reading the comments and the `uio` struct, first we must understand that in terms of this implementation, a `uio` is a data structure used by the kernel to assist with the movement of data within the kernel and user space. The struct includes the data in the `struct iovec *uio_iov` field, and the other fields contain other useful information for the kernel. Most importantly (and related to this question), we see a field called `enum uio_seg uio_segflg` which tells us whether the source or destination is in the system (kernel) space or user space, but for user space we have `UIO_USERISPACE` and `UIO_USERSPACE`. The comments help us but don't tell us the full picture; `UIO_USERISPACE` refers to user process code, or executables (with instructions, hence the `I` in the macro). `UIO_USERSPACE` tells us that this `vnode` or data is some other form of data (non-executable). 

It's clear to see this with the following code from the function `load_segment` – a function that loads a file (data) into a segment of a virtual address as well as on disk – in the `syscall/loadelf.c` file.

```c
static
int
load_segment(struct addrspace *as, struct vnode *v,
		off_t offset, vaddr_t vaddr,
		size_t memsize, size_t filesize,
		int is_executable)
{
	...
	u.uio_segflg = is_executable ? UIO_USERISPACE : UIO_USERSPACE;
	...
}
```

Finally in `lib/uio.c` we have the `uiomove` function, the functionality of it is given in the code block below (and taken from `uio.h`)

```c
/*
* Copy data from a kernel buffer to a data region defined by a uio struct,
* updating the uio struct's offset and resid fields. May alter the iovec
* fields as well.
*/
int
uiomove(void *ptr, size_t n, struct uio *uio)
{
	...
	
	if (uio->uio_segflg==UIO_SYSSPACE) {
		KASSERT(uio->uio_space == NULL);
	}
	
	...
	
	switch (uio->uio_segflg) {
		case UIO_SYSSPACE:
			result = 0;
			if (uio->uio_rw == UIO_READ) {
				memmove(iov->iov_kbase, ptr, size);
			}
			else {
				memmove(ptr, iov->iov_kbase, size);
			}
			iov->iov_kbase = ((char *)iov->iov_kbase+size);
			break;
	
		case UIO_USERSPACE:
		case UIO_USERISPACE:
	
			if (uio->uio_rw == UIO_READ) {
				result = copyout(ptr, iov->iov_ubase,size);
			}
			else {
				result = copyin(iov->iov_ubase, ptr, size);
			}
			if (result) {
				return result;
			}
			iov->iov_ubase += size;
			break;
			default:
				panic("uiomove: Invalid uio_segflg %d\n",
					(int)uio->uio_segflg);
	}
	...
}
```

**Question 3** Why can the `struct uio` that is used to read in a segment be allocated on the stack in `load_segment()` (i.e., where does the memory read actually go)?

well if we look at load_elf that's where the actual load happends of an ELF file.

**Question 4** In `runprogram()`, why is it important to call `vfs_close()` before going to usermode?

> It's important to call `vfs_close` before going to usermode for a couple of reasons; for one, having an open file in the kernel takes resources and memory. Thus, if we don't use `vfs_close`, this will result in a memory, as there will be an open file in the kernel (and eventually the kernel's memory will fill up). Moreover, the user process won't have the privilege to close the kernel resource, which is why it must be done BEFORE going to usermode.

If we look into the semantics of `vfs_close` we notice it uses `VOP_DECREF`. Once the refrence count to the `vnode` hits 0, `VOP_RECLAIM` is called to reclaim the resource consumed by the vnode.

```c
/* Does most of the work for close(). */
void
vfs_close(struct vnode *vn)
{
        /*
         * VOP_DECREF doesn't return an error.
         *
         * We assume that the file system makes every reasonable
         * effort to not fail. If it does fail - such as on a hard I/O
         * error or something - vnode.c prints a warning. The reason
         * we don't report errors up to or above this level is that
         *    (1) most application software does not check for close
         *        failing, and more importantly
         *    (2) we're often called from places like process exit
         *        where reporting the error is impossible and
         *        meaningful recovery is entirely impractical.
         */

        VOP_DECREF(vn);
}
```

```c
/*
 * Decrement refcount.
 * Called by VOP_DECREF.
 * Calls VOP_RECLAIM if the refcount hits zero.
 */
void
vnode_decref(struct vnode *vn)
{
	...

        if (destroy) {
                result = VOP_RECLAIM(vn);
                if (result != 0 && result != EBUSY) {
                        // XXX: lame.
                        kprintf("vfs: Warning: VOP_RECLAIM: %s\n",
                                strerror(result));
                }
        }
}
```

**Question 5** What function forces the processor to switch into usermode? Is this function machine dependent?

> The function that forces the processor to switch to usermode is `mips_usermode`. This function is machine dependent since it's specifically for the MIPS architecture.

A bit more detail about the whole process:

In `enter_new_process` we see a call to `mips_usermode` at the end of the function, passed with some specific values in the trapframe (args, stack pointer, etc.).

```c
/*
 * enter_new_process: go to user mode after loading an executable.
 *
 * Performs the necessary initialization so that the user program will
 * get the arguments supplied in argc/argv (note that argv must be a
 * user-level address) and the environment pointer env (ditto), and
 * begin executing at the specified entry point. The stack pointer is
 * initialized from the stackptr argument. Note that passing argc/argv
 * may use additional stack space on some other platforms (but not on
 * mips).
 *
 * Unless you implement execve() that passes environments around, just
 * pass NULL for the environment.
 *
 * Works by creating an ersatz trapframe.
 */
void
enter_new_process(int argc, userptr_t argv, userptr_t env,
                  vaddr_t stack, vaddr_t entry)
{
        struct trapframe tf;

        bzero(&tf, sizeof(tf));

        tf.tf_status = CST_IRQMASK | CST_IEp | CST_KUp;
        tf.tf_epc = entry;
        tf.tf_a0 = argc;
        tf.tf_a1 = (vaddr_t)argv;
        tf.tf_a2 = (vaddr_t)env;
        tf.tf_sp = stack;

        mips_usermode(&tf);
}
```

In `mips_usermode` we see it prepares the processor to go into user mode and calls the assembly routine `asum_usermode`. The assembly routine then jumps to `exception_return`  which transitions the processor to user mode.

```c
/*
 * Function for entering user mode.
 *
 * This should not be used by threads returning from traps - they
 * should just return from mips_trap(). It should be used by threads
 * entering user mode for the first time - whether the child thread in
 * a fork(), or into a brand-new address space after exec(), or when
 * starting the first userlevel program.
 *
 * It works by jumping into the exception return code.
 *
 * mips_usermode is common code for this. It cannot usefully be called
 * outside the mips port, but should be called from one of the
 * following places:
 *    - enter_new_process, for use by exec and equivalent.
 *    - enter_forked_process, in syscall.c, for use by fork.
 */
void
mips_usermode(struct trapframe *tf)
{
	...
        /*
         * This actually does it. See exception-*.S.
         */
        asm_usermode(tf);
}
```

```c
/*
 * Code to enter user mode for the first time.
 * Does not return.
 *
 * This is called from mips_usermode().
 * Interrupts on this processor should be off.
 */

   .text
   .globl asm_usermode
   .type asm_usermode,@function
   .ent asm_usermode
asm_usermode:
   /*
    * a0 is the address of a trapframe to use for exception "return".
    * It's allocated on our stack.
    *
    * Move it to the stack pointer - we don't need the actual stack
    * position any more. (When we come back from usermode, cpustacks[]
    * will be used to reinitialize our stack pointer, and that was
    * set by mips_usermode.)
    *
    * Then just jump to the exception return code above.
    */

   j exception_return
   addiu sp, a0, -16            /* in delay slot */
   .end asm_usermode
```

 **Question 6** In what file are `copyin` and `copyout` defined? `memmove`? Why can't `copyin` and `copyout` be implemented as simply as `memmove`?



**Question 7** What (briefly) is the purpose of `userptr_t`?



**Question 8** What is the numerical value of the exception code for a MIPS system call?



**Question 9** How many bytes is an instruction in MIPS? (Answer this by reading `syscall()` carefully, not by looking somewhere else.)



**Question 10** Why do you "probably want to change" the implementation of `kill_curthread()`?



**Question 11** What would be required to implement a system call that took more than 4 arguments?



**Question 12** What is the purpose of the `SYSCALL` macro?



**Question 13** What is the MIPS instruction that actually triggers a system call? (Answer this by reading the source in `userland/lib/libc/arch/mips`, not looking somewhere else.)



**Question 14** After reading `syscalls-mips.S` and `syscall.c`, you should be prepared to answer the following question: OS/161 supports 64-bit values; `lseek()` takes and returns a 64-bit offset value. Thus, `lseek()` takes a 32-bit file handle (arg0), a 64-bit offset (arg1), a 32-bit whence (arg2), and needs to return a 64-bit offset value. In `void syscall(struct trapframe *tf)` where will you find each of the three arguments (in which registers) and how will you return the 64-bit offset?



**Question 15** As you were reading the code in `runprogram.c` and `loadelf.c`, you probably noticed how the kernel manipulates the files. Which kernel function is called to open a file? Which macro is called to read the file? What about to write a file? Which data structure is used in the kernel to represent an open file? 



**Question 16** What is the purpose of `VOP_INCREF` and `VOP_DECREF`?

## GPT answers for reference

**Question 2**

Your understanding of the `uio` struct and its role in data transfer is on the right track. Let me address your specific questions and clarify some of these concepts.

### Data Transfer Context

The `uio` struct and the related flags (`UIO_USERISPACE`, `UIO_USERSPACE`, `UIO_SYSSPACE`) are indeed used for data transfer between different parts of the system, particularly for managing memory and handling system calls. This is crucial in the OS for moving data between kernel space and user space, which must be done carefully to maintain system integrity and security.

### User Space and Kernel Space

- **User Space**: This is the memory space where user applications run. It's divided into:
    - **Instruction Space (`UIO_USERISPACE`)**: This is where the executable code of user programs resides. It contains the compiled machine instructions (not assembly but already compiled to machine code) that the CPU will execute.
    - **Data Space (`UIO_USERSPACE`)**: This includes all the data that the user process works with—like heap, stack, and other variables. This is where runtime data such as variables, buffers, and other dynamically allocated memory reside.

The two flags, `UIO_USERISPACE` and `UIO_USERSPACE`, differentiate between code and data. For example:

- **`UIO_USERISPACE`** is used when loading or copying parts of a user executable (e.g., the text segment of a process), which is the instruction code that the CPU executes.
- **`UIO_USERSPACE`** is used for copying other types of user process data (like stack, heap, or global variables).

### Kernel Space (`UIO_SYSSPACE`)

- **Kernel Space**: This is the memory region where the kernel and kernel services run. The kernel manages all system operations and hardware communication, and kernel memory is generally protected from direct access by user processes.
- The `UIO_SYSSPACE` flag is used when the data transfer is happening entirely within the kernel (e.g., copying data between kernel buffers or managing kernel data).

### File System Transfers

The `uio` struct can be used in different scenarios beyond just loading executable code. It is often used in **file system operations**, such as reading and writing files. For example, when a user process requests to read a file, the kernel might need to transfer data from the disk into user space. In these cases, the `uio` struct provides the necessary metadata to manage the transfer, such as:

- **Pointer to data blocks** (`uio_iov`)
- **Size of data** (`uio_resid`)
- **Address space** (`uio_space`)

### Executable Code in Memory

Executable code is typically stored as **machine code** in memory, which is what the CPU directly understands and executes. When the kernel loads an executable file, it reads segments from the file (e.g., using `load_segment`) and writes them to memory. These segments may include:

- **Text segment**: Contains executable instructions.
- **Data segment**: Contains initialized global and static variables.
- **BSS segment**: Contains uninitialized global and static variables.

The text segment is what we refer to as **instruction space**, while the data and BSS segments are part of **data space**.

### Separation Between Instruction and Data Space

Yes, user space can be conceptually divided into **instruction space** (where the executable code lives) and **data space** (for runtime data like stack, heap, etc.). This separation is reflected by having different flags (`UIO_USERISPACE` vs. `UIO_USERSPACE`) to indicate whether the data transfer concerns instructions or data.

### Kernel Space

Kernel space, as you mentioned, is simply **kernel memory**, which is used for the kernel code and kernel data structures. There is no further categorization akin to instruction and data space because kernel code and data operate in a unified privileged memory region.

### Summary

- **`UIO_USERISPACE`** is used for transferring executable code into user process memory.
- **`UIO_USERSPACE`** is for other user data (heap, stack, etc.).
- **`UIO_SYSSPACE`** is for transfers involving kernel memory.

This distinction is vital for ensuring the correct handling of memory regions, which have different access permissions and use cases depending on whether they are user code, user data, or kernel memory.



