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

# Chapter 2 

* There are many different unix shells, but all of them derrive from the Bourne Shell (/bin/sh)
* Commands that run in the shell could be both other programs or things that are built into the shell
* There was something on wildcards and I/O streams, go back and write about those.
* Unix processes use I/O streams to read and write data
* Processes read data from input streams and write data to output streams.
* Streams are very flexible. The source of an input stram could be a file, a device, a termianl window, or even the output stream from another process.
* The reason the cat command adopts an interactive behaviour when you do not specify an input filename is because it reads from the standard input stream provided by the kernel, rather than a stream connected to a file. In this case, stdin is connected to the terminal where you run cat.

### Shell globbing
* Can match simple patterns to file and directory names
* The shell matches arguments containing globs to  filenames, substitutes the filenames for those arguments, and then runs the revised command line.
    * The substitution is reffered to as expansion because the shell substitues all matching filenames for a simplified expression
* If no file matches a glob, the bassh shell performs no expansion, and the command runs with literal characters such as *.
* The shell performs expansions before running commands, and only then. Therefore, if a * makes it to a command without expanding, the shell won't do anything more with it. It is up to the command to decide what it wants to do.

### Intermediate UNIX commands
* grep
    * Prints lines from a file or input stream that match an expression
    * Very useful for operating on multiple files at once since it prints the filename in addition to the amthcing line
    * Use -i option for case insensitive matches
    * Use -v option for invert search (printing all of the lines that dont match)
    * More powerful varient exists called egrep, which is basically just grep -E
    * Understands regular expressions, which are more powerful than wildcard style patterns, adn they have a different syntax.
    * Regular expressions:
        * .* matches any number of characters, inclduign none liek the * in globs and wildcards
        * .+ matches any one or more characters
        * . matches exactly one arbitrary character.
* diff
    * See the difference between two text files
* find and locate
    * Finds files that are somewhere in the difrectory tree.
    * Accepts special pattern matching characters such as *, but you must enclose them in single quotes.
    * Most systes also have a locate command for finding files. Rather than searching for a file in real time, locate searches an index that the system builds periodically. Searching with locate is much faster than find, but if the file you're looking for is newer then the index, locate won't find it
    * Format for find: find dir -name file -print
* head and tail
    * Allow you to quicklu view a portion of a file or stream of data.
    * By default, head displays the first 10 lines of a file, and tail displays the last 10 lines. 
    * To change the number fo lines to display, use the -n option, where n is the number of lines you want to see (for example, head -5 /etc/passwd). 
    * To print lines starting at line n, use tail +n
* sort
    * Quickly puts the lines of a text file in alphanumeric order.
    * if the file's line start with numbers and you want to sort in numerical order, use the -n option.
    * The -r option reverses the order of the sort
## Changing your password and shell
* Use the passwd command to change your password
* Change your shell with the chsh command
## Dot files
* Configuration files/directories
* Shell globs don't match dot files unless you explicity use patterns such as .*
    * However, you can run into problems with globs because .* matches . and .. (the current and parent directories). Yoiu may wish to use a pattern such as .[^.]* or .??* to get all dot files escept he current and parent directories
## Environment and Shell Variables
* shell can store temporary variables, called shell variables, containing the values of text strings.
* Shell variables are very useful for keeping track of values in scripts, and some shell varaibles control the way she shell behaves
* TO assign a value to a shell variable, use the equal sign
    * Ex: STUFF=blah
    * Do not put any spaces.
* An environment variable is like a shell variable, but it si not specific to the shell
* The main difference between environment and shell variables is tha tth eoperating system passes all of your shell's enviromnment variables to programs that the shell runs, whereas shell varaibles cannot be accessed in the commands that you run.
* You assign environment variables with the shell's export command.
    * Ex: STUFF=blah && export STUFF
* Child processes inherit environment variables from their parent
* Many programs read from environment variables for configuration options.
### Getting online help
    * Use man -k to search a manual page by keyword
    * Manual pages are referenced by numbered sections
    * When someone refers to a manual page, they often put the  number in parentheses, like ping(8)
    * By default, man displays the first page that it finds, but you can select a manual page by section
        * man <section> <keyword>
    * Some time ago, the GNU Project decided that it didn't like the manual pages very much and switches to another format called info (or textinfo)
        * Often this documentation goes further than tuypical man pages, but it can be more complex
    ### Standard Error
    * You can send stderr to the same place as stdout by using 2>&1
    
    ## Standard Input Redirection
    * You will ocassioaly urn into a program that requires stdin redirection, but must Unix commands accept filenames as arguments so this is not very common
        * head < /proc/cpuinfo is the same as head /proc/cpuinfo

### Understanding Input Redirection
* When addressing errors, always address the first error first

### Common Errors
* Permission denied
    * You get this error when you attempt to read or write to a file or directory that you're not allowed to access (you have insufficient privileges)
    * Also shown when you try to execute a file that does not have the execute bit set
* Operation not permitted
    * Happens when you try to kill a proces that you don't own
* Segmentation fault, Bus error
    * A segmentation fault essentially menas that the person who wrote the prgram that you just ran screwed up somewhere. The program tried to access a part of memory that it was not allowed to touch, and the OS killed it. 
    * Similarly, bus error means the program tried  to access memory in a way that it shouldn't have.
* When you get one of these errors, you might be giving th eprogram some input tha it did not exepect. 
    * In rare cases, it might be faulty memory hardware

### Listing and Manipulating Processes
* Each process (a running program) has a process ID. 
* For a quick listing of running processes, just run ps on the command line
* The fields are as follows
    * PID - the pprocess ID
    * TTY The terminal where the process is running
    * STAT The process status--that is, what the process is doing and where its memory resides. For example, S means sleeping and R means running. 
    * COMMAND This one might seem obvious as the command used to run the program, but be aware that a process can change this field from its origin value. Furthermore, the shell can perform glob expasion, and this field will reflect the expanded command instead of what you entered into the prompt.
    * ps command has many options and you can specify the options in 3 different styles--Unix, BSD< and GNU
    * Many people find the BSD style to be most comfortable (perhaps because it involves less typing), so that is what is used in the book
        * ps x <-- Show all of your running processes
        * ps ax <-- show all processes on the system, not just th eones that you own
        * ps u <-- Include more detailed information on processes
        * ps w <-- Show full command names, not just what fits the line
        * As with other commands, you can combine the options (ex: ps aux)
    * To check a specific process, add its PID to the argument list of the ps command

### Process Termination
* To terminate a pprocess, you send it a singal--a message to a process from the kernel--with the kill command
* In most cases you do: kill pid
* If you for example want to freeze a process instead of terminating it, you can use kill -STOP pid
* A stopped process is still in memory, ready to pick p where it left off.
* Use the CONT signal to continue the running of the process again.
* Using CTRL-C to termiante a process that is running  is the same as using kill to end the process with the INT (interrupt) signal
* The kernel gives most processes a chance to clean up after themselves upon recieving signals (with the singal handler mechansim). However some processes may choose nonterminating action in response to a signal, get wedged int he act of trying to handle it, or simply ignore it.
    * If this happens, you can send the kill signal. KILL cannot be "handled" in fact, the OS does not even give the process a chance. THe kernel just terminates the process and forcibly removes it from memory. Use this method only as a last resort.
* You can also use numbers with the kill command, becasue each number maps to a signal. THis may be quicker to write
    * Use kill -l to get a list of all of the signals.

### Job Control
* shells support job control, a wayt o send TSTP (similar to STOP) and CONT signals to progrmas by using various keystrokes and commands. 
* Allows you to suspend and switch between programs that you are using
