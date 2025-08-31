# Processes, threads, frames, stack, heap

## Processes and threads

A **process** is an instance of an executing program.

A **process** will have one or more **threads**.

Each **process** has its own memory space. Communication between processes is termed Inter Process Communication (IPC).

Each **thread** has access to its own **stack** and **registers**.

In Linux, **processes** and **threads** are both **tasks** and what distinguishes them are their "context of execution" (COE).

!!! quote "Quotes"

    A **process** is an operating system abstraction that groups together multiple resources:

    - An address space
    - One or more threads
    - Opened files
    - Sockets
    - Semaphores
    - Shared memory regions
    - Timers
    - Signal handlers
    - Many other resources and status information

    [Linux kernel labs](https://linux-kernel-labs.github.io/refs/heads/master/so2/lec3-processes.html#overview-of-process-resources)

    A **thread** is the entity within a process that can be scheduled for execution. All threads of a process share its virtual address space and system resources. In addition, each thread maintains exception handlers, a scheduling priority, thread local storage, a unique thread identifier, and a set of structures the system will use to save the thread context until it is scheduled. The thread context includes the thread's set of machine registers, the kernel stack, a thread environment block, and a user stack in the address space of the thread's process. Threads can also have their own security context, which can be used for impersonating clients.

    [Microsoft](https://learn.microsoft.com/en-us/windows/win32/procthread/about-processes-and-threads)
    
    In some operating systems, such as GNU/Linux and Solaris, a single program may have more than one **thread** of execution. The precise semantics of threads differ from one operating system to another, but in general the threads of a single program are akin to multiple processes—except that they share one address space (that is, they can all examine and modify the same variables). On the other hand, each thread has its own registers and execution stack, and perhaps private memory.
    
    [GDB](https://sourceware.org/gdb/current/onlinedocs/gdb#Threads)

#### References

[Linus Torvalds](https://www.evanjones.ca/software/threading-linus-msg.html)

[Linux Kernel Labs](https://linux-kernel-labs.github.io/refs/heads/master/so2/lec3-processes.html#processes-and-threads)

[Site24x7](https://www.site24x7.com/learn/linux/linux-threads.html)

[Applied programming](https://applied-programming.github.io/Operating-Systems-Notes/2-Process-Management/)

[Procedure Call Standard for the Arm® 64-bit Architecture (AArch64)](https://github.com/ARM-software/abi-aa/blob/main/aapcs64/aapcs64.rst#threads-and-processes)

