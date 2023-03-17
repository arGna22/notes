# How Linux Works 

* A Linux system has three main levels.
    * Hardware is at the base: The physical, electronic components that make up a computer.  These include, the processor, main memory, disks, and network port.
    * Linux Kernel: The next level up. The core of the operating system. The kernel is the software residing in memory that tells the CPU where to look for its next task. Acting as a mediator, the kernal manages the hardware, especially main memory and is thye primary interface between the hardware and any running program.
    * User space: At the top. The running programs that the kernal manages collectively make up the user space. A more specific term for process is user process, regardless of whether or not a user directly interacts with the process. 
    
### How kernel vs user processes run
* Kernel runs in kernal mode
    * unrestricted access to processor and main memory
    * Memory area that the kernel can access is known asthe kernel space
* User processes run in user mode
    * Restricts access to a small subset of memory and safe CPU operations
    * Userspace refers to the part of main memory that user processes can access.
    * I think all processes run in the user space, could be wrong.

* User processes are restricted for reasons of safety. If your web browser crashes for example, the consequences are limited and can be cleaned up by the kernel.
* However, different processes possess different permissions and with the correct permissions, a user process gone haywire might be able to do serious damage to a system, but there are safeguards.
* The kernal can run kernel threads, which look much like processes, but have access to kernel space.

### Understanding main memory
* Most important hardware
* Big storage area for 1s and 0s. 
    * Each slot for a 0 or 1 is called a bit
* Where the running kernel and processes reside.
    * The kernel and processes are just big vcollectiosn of bits.
* All I/O from peripheral devices flows through main memory
* A CPU is just an operator on memory
    * Reads its instructions and data from emory and writes data back to memory.
* The term state is often used in reference to processes, the kernel, and other parts of the computer system.
    * strictly speaking, a state is a particualr arrangement of bits .
    * Since processes for example easily consist of millions of biuts in memory, it is often easier to use abstract terms when talking aboiut sates, you describe what something has done or is doing at the moment
    * Becasue state is commonly referred to in abstract terms, the term image refers to a particular physical arragement of bits.

### Kernel
* Nearly everything the kernel does revolves around main memory
* Tasked with splitting memory into many subdividsions nd it must maintain certain state information about those subdivisions at all times. 
* Each process gets its own share of memory, and the kernel must ensure that each proces keeps to its own share.
* Responsible for managing tasks in 4 general system areas.
    * Processs: Responsible for determining which processes are allowe to use the  CPU.
    * Memory: The kernel needs to keep track of all memory--what is currently allocated to a particular process, what might be shared between processes, and what is free.
    * Device drivers: The kernel acts as an interface between hardware (such as a disk) and processes. It's usually the kernel's job to operate the hardware.
    * System calls and support: Processes usualyl use systemcalls to communicate with the kernel

### Process Management
* The kernel is responsible for context switching
* Process management describes the starting, pausing, resuming, and terminating of processs.
* On a modern computer system, processes appear to run simultaneously, but that is an illusion. The processes behind certain applications such as a web browser and spreadsheet typiclly do not run at the same time.
* Each process with use a CPU for a small fraction of a second, then another process takes a turn and so on.
    * This is known as a context switch, the act of a process giving up control of the CPU to another process
* Each piece of time that a process has control of the CPU is known as a time slice and provides a process enough time to perform a significant computation.
* Time slices are so small, humans cannot percieve them.
* An example where a process is running in user mode, but its time slice is up.
    * The CPU (the actual hardware interrupts the current process based onan interal timer, switches into kernel mode, and hands control back to the kernel.
    * Kernel records current state of CPU and memory, which will be essential tpo resuming the process that was interrupted.
    * Kernel performs any tasks that might have come up during the preceeding time slice (such as collecting data from I/O operations)
    * Kenrel is now ready to let another process run. The kernel analyzes the list of processes that are ready to run and chooses one.
    * The kernel prepares the memory for this new process and then prepares the cpu.
    * The kernel tells the CPU how long the timeslice forthe new process will last. 
    * The kernel switches the CPU into user mode and hands control of the CPU to the process.
* Context switch answers the important question of when the kernel runs. It runs between process time slices (during a context switch).

### Memory Management
* Kernel must manage memory during context switch, which can be a complex job.
*  The following conditions must hold:
    *  Kernel must have its own private area in memory that user processes can't access
    *  Each process needs its own section of memory
    *  One user process may not access the private memory of another process.
    *  User processes can share memory
    *  Some memory in user processes can be read-only
    *  The system can use more memory than is physically present by using disk space as auxiliary
*  Modern CPUs contain an MMU (Memory Management Unit) that enables a memory access scheme known as virutal memory
*  While using virtual memory, a process does directly access the mrmory by its physical location in the hardware. Instead, the kernel sets up the process as if it had an entire machine to itself.
*  When the process accesses some of its memory,  the MMU intercepts the access and uses a memory address map to translate the memory address from the process POV into an actual physical memory location on the machine.
*  The kernel initializes, maintians and alters this address map. For example, there is one map that the MMU uses, so during a context switch, the kernel must switch the address map to the incoming process from the outgoing process.

### Drivers and Device Management
 * Kernel's role with devices is relatively simple.
 * A device is typically on accessible in kerenl mode because improper access (such a sa user process asking to tuen the power off could crash the machine.
 * Different devices rarey have the same programming interface, even if the devices perform thesame task.
    * For this, we have device drivers to provide a  uniform interface for user processes in order to simplify a software developer's job.

### System calls and support
* Syscalls perform specific tasks that user processes cannot do well or at all
* For example, the act of opening, reading, and writing to files.
* All new processes on a linux system start as a result of fork()  (fork() tells the kernel to make a nearly identical copy of the process).
* When a program calls an exec function, the kernel loads and starts the prorgam, replacing the current process.
* A sysscall as an interaction between a process and the kernel
* The kernel also supports user processes with features other than traditional syscalls, most common of which are pseudodevices, which look like devices to user processes, but are implementeds purely in software.  This means they dont technically need to be in the kernel, but they are usually there for practical reasons.
* Technically, a user process must use a syscall to open a pseduodevice, so processes can't completely avoid syscalls

### User Space
* Most real action happens here
* Although all processes are esentially equal from the kernel's POV, they all perform different tasks, they perform different tasks for users and can be put into a hierarchy
* A process is simply a state or image in memory
* User processses make up the environment that you interact with

## Users
* The Linux kernel supports the traditional concept of a unix user, an entity that can run processes and own files
* A user is most often associated with a username.
* Kernel does not manage the usernames, it identifies users by numeric identifiers known as user ids 
* Users exist primarily to support permissions and boundaries.
* Every user-space process has an owner and is said to run as the owner
* A user may terminate or modify the behaviour of its own processes (within certain limits), but it cannot interfere with other user's processes
* A Linux system normally has a number of users, including the ones that correspond to real human beings
* The root user is special in that it is an exception to the previously stated rules. For this reason, it is known as the superuser.
* A person who can operate as root is said to have root access. 
* Anyone with root access is considered to be an administrator on a traditional unix system.
* Operating as root can be dangerous. It can be difficult to identify and correct mistakes because the system will allow you to do anything, even if it is harmful to the system. For this reason, system desginers try to make root access as unncessary as possible 
* As powerful as the root user is, it still runs in the OS's usermode, not kernel mode.
* Groups are sets of users . The primary purpose of groups is to allow a user to share file access to other members of a group.
