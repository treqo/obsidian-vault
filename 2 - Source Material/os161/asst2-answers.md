
---
**Question 1** What happens to a thread when it exits (i.e., calls `thread_exit()`)? What about when it sleeps?

When a thread exits, it first detaches from the process, then checks to make sure that the stack hasn't been corrupted. Next, it ensures interrupts are disabled on the current processor. Finally, it changes the threads state to a zombie state and includes a `panic` protection in case it is scheduled or run again (which shouldn't happen since zombie state). 

When it sleeps, it first disables interrupts on the processor, checks the stack to ensure no corruption, changes the thread state to `S_SLEEP`, adds the thread to the list in the tail of the wait channel queue, which is a queue of sleeping threads. Only after being added, the spinlock is released so other threads can access the wait channel. The thread only takes control once wakeup is invoked on it.

**Question 2** What function(s) handle(s) a context switch?

In `thread.c` we have the function `thread_switch`; the function spec details the following "High level, machine-independent context switch code." 

Additionally, we see the following line in `thread_switch`:
```c
/* do the switch (in assembler in switch.S) */
switchframe_switch(&cur->t_context, &next->t_context);
```

and if we go to `./kern/arch/mips/thread/switch.S` we find

```S
/*
 * Assembly language context switch code.
 */
```

So this is the low-level code which handles context switching by doing things like saving registers (the state of the old thread) and switching to the state of the new thread.

**Question 3** What does it mean for a thread to be in each of the possible thread states?

in `thread.h` we can see all the possible states for a thread to be in:
```c
/* States a thread can be in. */
typedef enum {
S_RUN, /* running */
S_READY, /* ready to run */
S_SLEEP, /* sleeping */
S_ZOMBIE, /* zombie; exited but not yet deleted */
} threadstate_t;
```

1. `S_RUN`: This indicates the thread is in a running state. It means the thread is currently running on the CPU core, and actively executing instructions.
2. `S_READY`: This indicates that the thread is currently not running, but is ready to run as soon as the CPU is available and the scheduler picks them to run.
3. `S_SLEEP`: This indicates that the thread is in a sleeping state, which means it currently isn't doing any work, and is in an idle state, waiting to be prompted to run again. 
4. `S_ZOMBIE`: This is a thread that has finished execution but it has yet to be cleaned up by the system. It is no longer runnable, but is simply waiting for the kernel to cleanup so that it can be removed from the system.


**Question 4** What does it mean to turn interrupts off? How is this accomplished? Why is it important to turn off interrupts in the thread subsystem code?

Turning off interrupts means that even if there is an interrupt signal, the interrupt handler won't deal with it until interrupts are turned back on. This is necessary to have when performing actions that needs to be finished and not interrupted, like saving state of registers when context switching. This is accomplished with the function `splhigh` in OS161. Looking at `spl.h` we can see exactly what the function means and what it is for

```c
* Machine-independent interface to interrupt enable/disable.
*
* "spl" stands for "set priority level", and was originally the name of
* a VAX assembler instruction.
*
* The idea is that one can block less important interrupts while
* processing them, but still allow more urgent interrupts to interrupt
* that processing.
* ...
* spl0() sets IPL to 0, enabling all interrupts.
* splhigh() sets IPL to the highest value, disabling all interrupts.
* splx(s) sets IPL to S, enabling whatever state S represents.
*
```

So essentially, `spl` sets the priority level of an operation: if an operation is of high importance, then we use `splhigh` to disable all interrupts, and ensure that the operation completes without getting interrupted.

Hence, turning off interrupts in thread subsystem code is important because it ensures that its operations complete without getting interrupted.

**Question 5** What happens when a thread wakes up another thread? How does a sleeping thread get to run again?

When a thread wakes up another thread, it first invokes `wchan_wakeone`, a function which handles waking up one thread sleeping on a wait channel. From there, the `thread_make_runnable` function is called on the sleeping thread, which subsequently changes the sleeping threads state to `S_READY`, and adds the thread to the tail of the run queue, `c_runqueue`, of a specific cpu. 

The thread gets to run again when it gets removed from the queue and starts running on the cpu, which is a context switch, this is done in the `thread_switch` function and we can see the following code handling thread state transition:

```c
/* Put the thread in the right place. */

switch (newstate) {
// ...
	case S_READY:
	thread_make_runnable(cur, true /*have lock*/);
	break;
// ...
}
```

**Question 6** What function(s) choose(s) the next thread to run?

In the `thread_switch` function, the function handling thread context switches, we see the following lines:

```c
/* Get the next thread ...
*/
curcpu->c_isidle = true;
do {
	next = threadlist_remhead(&curcpu->c_runqueue);
	// ...
} while (next == NULL);

```

Thus, we can see that `threadlist_remhead` is responsible for choosing the next thread to run, by removing it from the head of a cpu's respective `c_runqueue`. Thus, both `thread_switch` and `threadlist_remhead` are functions that choose the next thread to run.

Also, we should consider the scheduler function (unimplemented) `schedule`. It is defined as follows:

```c
/*
* Scheduler.
*
* This is called periodically from hardclock(). It should reshuffle
* the current CPU's run queue by job priority.
*/
void
schedule(void) {
	// ...
}
```

Thus, once implemented, it will have a role in picking the next thread to run since it organizes the run queue, effectively turning it into a priority queue. Hence, it is also a function affecting the next choice of thread.

**Question 7** How does it (do they) pick the next thread?

As stated before, a queue structure is used to hold threads that are ready to run, i.e. in the `S_READY` state. Thus, it selects the next thread to run by removing the head of the run queue, and sets it as the current thread running on the cpu with `curcpu->c_curthread = next; curthread = next;`.

**Question 8** What role does the hardware timer play in scheduling? What hardware independent function is called on a timer interrupt?

Looking into the file `clock.c` we see that the function `hardclock` plays a roll in scheduling, as it performs the `schedule` function in `thread.c` at every 4 hard clocks – Technically, it doesn't have to be 4, as of now, the code base default is `#define SCHEDULE_HARDCLOCKS 4`, but there's a comment that mentions that "These \[macros\] should be tuned along with any work done on the scheduler." Thus, we can see there a pre-defined time where whenever the hardware timer has gone through a certain amount of cycles, it will schedule the threads in the queues (this happens periodically). 

```c
/*
* This is called HZ times a second (on each processor) by the timer
* code.
*/
void
hardclock(void)
{
	/*
	* Collect statistics here as desired.
	*/
	curcpu->c_hardclocks++;
	if ((curcpu->c_hardclocks % MIGRATE_HARDCLOCKS) == 0) {
		thread_consider_migration();
	}
	if ((curcpu->c_hardclocks % SCHEDULE_HARDCLOCKS) == 0) {
		schedule();
	}
	thread_yield();
}
```

In `thread.h` we see, 

```c
/*
* Reshuffle the run queue. Called from the timer interrupt.
*/
void schedule(void);

  

/*
* Potentially migrate ready threads to other CPUs. Called from the
* timer interrupt.
*/
void thread_consider_migration(void);
```

thus we should note that `hardclock` handles timer interrupt, and so, `hardclock` is the hardware independent function called on a timer interrupt.

So to summarize, the hardware timer forces interrupts in threads which allows for context switching and the abstraction of multi-tasking by a cpu (what I mean by that is, to a human it looks like the cpu is doing multiple things at once, but really it is just switching from thread to thread rapidly). These regular interrupts from `hardclock` define when a cpu will 
- `thread_consider_migration`: "Potentially migrate ready threads to other CPUs. Called from the timer interrupt."
- `schedule`: "Reshuffle the run queue. Called from the timer interrupt."
- `thread_yield`: "Cause the current thread to yield to the next runnable thread, but itself stay runnable."

**Question 9** Describe how `wchan_sleep()` and `wchan_wakeone()` are used to implement semaphores.

In `synch.c` we see the `P` function `void P(struct semaphore *sem)` in which `wchan_sleep` is used to put the current thread to sleep until `sem_count` is greater than 0. It also puts the thread on the semaphore's wait channel `sem_wchan`. The reason for using `wchan_sleep` in the `P` function is to block the thread from using the resource until the `semaphore` count is 0, so effectively, the thread will sleep until the resource has been freed.

We also see the `V` function `void V(struct semaphore *sem)` in which `wchan_wakeone` is used to wake up one thread in the semaphore's wait channel `sem_wchan`. It increments the semaphore count, indicating that it has finished using the resource, and the resource is now available for another waiting thread.

**Question 10** How does the implementation of `wchan` ensure that a thread never misses a wakeup signal: that another thread cannot attempt to awaken the first thread just as it is preparing to sleep, but before it is actually placed into the sleep queue?

If we look at the function specs for `wchan` functions, we can see that they use spinlocks in their implementation. With spinlocks, this ensures that a thread never misses a wakeup signal, and race conditions don't occur because the spinlock protects the critical sections where the thread is preparing to sleep or wake up.

When a thread is put to sleep with `wchan_sleep` it is put to sleep while holding a spinlock that protects the wait channel. This ensures that the thread cannot be awakened just as it is preparing to sleep and before it is placed into the sleep queue. Only after the thread is fully asleep (complete the whole process) does it release the spinlock.

when a thread calls `wchan_wakeone` it also holds the same spinlock that protects the wait channel. This ensures that the thread attempting to wake up the sleeping thread will only proceed if the target thread has already been placed in the wait queue.


---
