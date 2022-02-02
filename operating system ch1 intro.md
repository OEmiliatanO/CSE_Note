---
tags: CSE
---
# operating system ch1 intro
---
> [TOC]
---
## basic os concept
### the os as an extended machine
operating system shields the programmer from the disk hardware and presents a simple file-oriented interface, it also conceals a lot of unpleasant business concerning interrupts, timers, memory management, and other low-level features.  
In each case, the abstraction offered by the operating system is simpler and easier to use than that offered by the underlying hardware.  
In this view, the function of the operating system is to present the user with the equivalent of an __extended machine__ or __virtual machine__ that is easier to program than the underlying hardware.

### the os as a resource manager
The concept of the operating system as primarily providing its users with a convenient interface is a top-down view.  
An alternative, bottom-up, view holds that the operating system is there to manage all the pieces of a complex system.
In the alternative view, the job of the operating system is to provide for an orderly and controlled allocation of the processors, memories, and I/O devices among the various programs competing for them.

### system call
The interface between the operating system and the user programs is defined by the set of "extended instructions" that the operating system provides.  
These extended instructions have been traditionally known as __system calls.__  

### processes
A process is basically a program in execution.   
Associated with each process is its __address space__, a list of memory locations from some minimum (usually 0) to some maximum, which the process can read and write.  
The address space contains the executable program, the program's data, and its stack.  
Also associated with each process is some set of registers, including the program counter, stack
pointer, and other hardware registers, and all the other information needed to run the program.  

When a process is suspended temporarily, it must later be restarted in exactly the same state it had when it was stopped. This means that all information about the process must be explicitly saved somewhere during the suspension.   
In many operating systems, all the information about each process, other than the contents of its own address space, is stored in an operating system table called the __process table__, which is an array (or linked list) of structures, one for each process currently in existence.  

If a process can create one or more other processes (usually referred to as __child processes__) and these processes in turn can create child processes, we quickly arrive at the process tree structure of __process tree__.  
![](https://i.imgur.com/ph2R6kF.png)  

### files
As noted before, a major function of the operating system is to hide the peculiarities of the disks and other I/O devices and present the programmer with a nice, clean abstract model of device-independent files.  
System calls are obviously needed to create files, remove files, read files, and write files. Before a file can be read, it must be opened, and after it has been read it should be closed, so calls are provided to do these things.  

To provide a place to keep files, MINIX 3 has the concept of a __directory__ as a way of grouping files together. Directory entries may be either files or other directories. This model also gives rise to a hierarchythe file systemas shown in below.  
![](https://i.imgur.com/GVAZHGt.png)  

Every file within the directory hierarchy can be specified by giving its __path name__ from the top of the directory hierarchy, the __root directory__. Such absolute path names consist of the list of directories that must be traversed from the root directory to get to the file, with slashes separating the components.  

At every instant, each process has a current working directory, in which path names not beginning with a slash are looked for.  

Files and directories in MINIX 3 are protected by assigning each one an 11-bit binary protection code. The protection code consists of three 3-bit fields: one for the owner, one for other members of the owner's group, one for everyone else, and 2 bits we will discuss later.  
Each field has a bit for read access, a bit for write access, and a bit for execute access. These 3 bits are known as the __rwx bits__.  
For example, the protection code rwxr_x__x means that the owner can read, write, or execute the file, other group members can read or execute (but not write) the file, and everyone else can execute (but not read or write) the file.  

*For a directory (as opposed to a file), x indicates search permission. A dash
means that the corresponding permission is absent (the bit is zero).  

Before a file can be read or written, it must be opened, at which time the permissions are checked. If access is permitted, the system returns a small integer called a file descriptor to use in subsequent operations. If the access is prohibited, an error code (1) is returned.  

Another important concept in MINIX 3 is the __mounted__ file system.  
To provide a clean way to deal with removable media (CD-ROMs, DVDs, floppies, Zip drives, etc.), MINIX 3 allows the file system on a CD-ROM to be attached to the main tree.  
Before the ```mount``` call, the root file system, on the hard disk, and a second file system, on a CD-ROM, are separate and unrelated.  
the ```mount``` system call allows the file system on the CD-ROM to be attached to the root file system wherever the program wants it to be. as the picture below.
![](https://i.imgur.com/dUljBjp.png)
*If directory b had originally contained any files they would not be accessible while the CD-ROM was mounted, since /b would refer to the root directory of drive 0.  

Another important concept in MINIX 3 is the special file. Special files are provided in order to make I/O devices look like files.  

Two kinds of special files exist: __block special files__ and __character special files__.  
Block special files are normally used to model devices that consist of a collection of randomly addressable blocks, such as disks.  
Similarly, character special files are used to model printers, modems, and other devices that accept or output a character stream.  
By convention, the special files are kept in the /dev directory. For example, /dev/lp might be the line printer.  

The last feature is __pipe__.  
A pipe is a sort of _pseudofile_ that can be used to connect two processes, as shown below.  
![](https://i.imgur.com/w6zKzyN.png)

### shell
__shell__ is the MINIX 3 command interpreter.(命令解析器)  
Although it is not part of the operating system, it makes heavy use of many operating system features and thus serves as a good example of how the system calls can be used.  
It is also the primary interface between a user sitting at his terminal and the operating system.  

## system call
### system call for process management
we will start the topic from ```fork```.  
fork is the only way to create a new process in MINIX 3. It creates an exact duplicate of the original process, including all the file descriptors, registers, etc... After the fork, the original process and the copy (the parent and child) go their separate ways.  
The fork call returns a value, which is zero in the child and equal to the child's process identifier or PID in the parent.  

In most cases, after a fork, the child will need to execute different code from the parent.  
Consider the shell. It reads a command from the terminal, forks off a child process, waits for the child to execute the command, and then reads the next command when the child terminates. To wait for the child to finish, the parent executes a ```waitpid``` system call, which just waits until the child terminates (any child if more than one exists).  
*waitpid can wait for a specific child, or for
any old child by setting the first parameter to 1.  

When waitpid completes, the address pointed to
by the second parameter, _statloc_ , will be set to the child's exit status (normal or abnormal
termination and exit value).  

the code outlines how ```fork``` is used by the shell:  
```c=
#define TRUE 1

while (TRUE){ /* repeat forever */
    type_prompt(); /* display prompt on the screen */
    read_command(command, parameters); /* read input from terminal */
    
    if (fork() != 0){ /* fork off child process */
        /* Parent code. */
        waitpid(1, &status, 0); /* wait for child to exit */
    } else {
        /* Child code. */
        execve(command, parameters, 0); /* execute command */
    }
}

```
> explaination:  
> When a command is typed, the shell forks off a new
process. This child process must execute the user command. It does this by using the execve system call, which causes its entire core image to be replaced by the file named in its first parameter.  
> 
> [execve linux man page](https://man7.org/linux/man-pages/man2/execve.2.html):
> execve() executes the program referred to by pathname.  This causes the program that is currently being run by the calling process to be replaced with a new program, with newly initialized stack, heap, and (initialized and uninitialized) data segments.  

----
the next is ```brk``` and ```sbrk```.  
Processes in MINIX 3 have their memory divided up into three segments: the __text segment__ (i.e.,
the program code), the __data segment__ (i.e., the variables), and the __stack segment__.  
The stack grows upward as needed, but expansion of the data segment is done explicitly by using a system call, ```brk``` , which specifies the new address where the data segment is to end.  

As a convenience for programmers, a library routine ```sbrk``` is provided that also changes the size of the data segment, only its parameter is the number of bytes to add to the data segment.

The next process system call is ```getpid```. It just returns the caller's PID.  
Remember that in fork , only the parent was given the child's PID. If the child wants to find out its own PID, it must use getpid.  

The ```getpgrp``` call returns the PID of the caller's process group. ```setsid``` creates a new session and sets the process group's PID to the caller's. Sessions are related to an optional feature of POSIX, job control , which is not supported by MINIX 3 and which will not concern us further.  

The last process management system call, ```ptrace``` , is used by debugging programs to control the program being debugged. It allows the debugger to read and write the controlled process' memory and manage it in other ways.  

### system call for signaling
Although most forms of interprocess communication are planned, situations exist in which unexpected communication is needed.  

For example, if a user accidently tells a text editor to list the entire contents of a very long file, and then realizes the error, some way is needed to interrupt the editor. In MINIX 3, the user can hit the CTRL-C key on the keyboard, which sends a __signal__ to the editor. The editor catches the signal and stops the print-out.  
Signals can also be used to report certain traps detected by the hardware, such as illegal instruction or floating point overflow. Timeouts are also implemented as signals.  

When a signal is sent to a process that has not announced its willingness to accept that signal, the process is simply killed without further ado.

To avoid this fate, a process can use the ```sigaction``` system call to announce that it is prepared to accept some signal type, and to provide the address of the signal handling procedure and a place to store the address of the current one. After a ```sigaction``` call, if a signal of the relevant type is generated (e.g., by pressing CTRL-C), the state of the process is pushed onto its own stack, and then the signal handler is called.  
When the signal handling procedure is done, it calls ```sigreturn``` to continue where it left off before the signal.  

Signals can be blocked in MINIX 3. A blocked signal is held pending until it is unblocked. It is not delivered, but also not lost.  
The ```sigprocmask``` call allows a process to define the set of blocked signals by presenting the kernel with a bitmap.  
It is also possible for a process to ask for the set of signals currently pending but not allowed to be delivered due to their being blocked. The ```sigpending``` call returns this set as a bitmap.  
Finally, the ```sigsuspend``` call allows a process to atomically set the bitmap of blocked signals and suspend itself.  

Instead of providing a function to catch a signal, the program may also specify the constant ```SIG_IGN``` to have all subsequent signals of the specified type ignored, or ```SIG_DFL``` to restore the default action of the signal when it occurs.  
The default action is either to kill the process or ignore the signal, depending upon the signal. 

As an example of how ```SIG_IGN``` is used, consider what happens when the shell forks off a background process as a result of  
```
command &
```

It would be undesirable for a SIGINT signal (generated by pressing CTRL-C) to affect the background process, so after the ```fork``` but before the ```exec``` , the shell does  
```sigaction(SIGINT, SIG_IGN, NULL);```  
(```SIGINT``` is interrupt signal)  
and  
```sigaction(SIGQUIT, SIG_IGN, NULL);```  
(```SIGQUIT``` is same as ```SIGINT``` except it makes a core dump of the process killed)  
to disable the ```SIGINT``` and ```SIGQUIT``` signals.  
more detail of [POSIX signal](https://en.wikipedia.org/wiki/Signal_(IPC)#POSIX_signals).  

what if we want to enforce terminating a process?  
The ```kill``` system call allows a process to signal another process (provided they have the same UID unrelated processes cannot signal each other).  
By sending signal 9 (```SIGKILL```), which cannot be caught or ignored, to a background process, that process can be killed.  


### system call for file magnagement
To create a new file, the creat call is used. Its parameters provide the name of the file and the protection mode:  
```fd = creat("abc", 0751)```  
the command creates a file called abc with mode 0751 octal. (rwxr_x__x in rwx bits)  

```creat``` not only creates a new file but also opens it for writing, regardless of the file's mode. The file descriptor returned, fd , can be used to write the file.  
If a creat is done on an existing file, that file is truncated to length 0, provided, of course, that the permissions are all right.  
*```creat``` is equivalent to ```open``` with flags equal to O_CREAT|O_WRONLY|O_TRUNC.  

To read or write an existing file, the file must first be opened using ```open```.  
The file descriptor returned can then be used for reading or writing. Afterward, the file can be closed by ```close```, which makes the file descriptor available for reuse on a subsequent ```creat``` or ```open```.  

The most heavily used calls are undoubtedly ```read``` and ```write```.  

Although most programs read and write files  sequentially, for some applications programs need to be able to access any part of a file at random.  
The ```lseek``` call changes the value of the position pointer, so that subsequent calls to read or write can begin anywhere in the file.  
```lseek``` has three parameters: the first is the file descriptor for the file, the second is a file position, and the third tells whether the file position is relative to the beginning of the file, the current position, or the end of the file.  
The value returned by lseek is the absolute position in the file after changing the pointer.

For each file, MINIX 3 keeps track of the file mode (regular file, special file, directory, and so on), size, time of last modification, and other information. Programs can ask to see this information via the ```stat``` and ```fstat``` system calls.  
Both calls provide as the second parameter a pointer to a structure where the information is to be put: 
[stat structure](https://man7.org/linux/man-pages/man2/lstat.2.html)  

When manipulating file descriptors, the ```dup``` call is occasionally helpful.  
Consider, for example, a program that needs to close standard output (file descriptor 1, or ```STDOUT_FILENO``` defined in unistd.h), substitute another file as standard output, call a function that writes some output onto standard output, and then restore the original situation.  

The solution is:  
```c=
int cpfd_stdout = dup(STDOUT_FILENO); // copy the orignal stdout entry
close(STDOUT_FILENO);                 // release the stdout  entry
int fd = dup(another_file);           // duplicate, and redirect the \
                                         file descriptor 1
/* do something... */
close(fd);                        // release the entry
fd = dup(cpfd_stdout);            // restore the orignal stdout
```
> [dup() linux man page](https://man7.org/linux/man-pages/man2/dup.2.html): The new file descriptor number is guaranteed to be the lowest-numbered file descriptor that was unused in the calling process.  

we can use ```dup2``` to do the same thing.  
```int dup2(int oldfd, int newfd)``` will close newfd,if need, and then adjust so that it now refers to the same open file description as oldfd.  
the return value is newfd.
```c=
int cpfd_stdout = dup(STDOUT_FILENO);
int fd = dup2(another_file, STDOUT_FILENO);
/* do something... */
fd = dup2(cpfd_stdout, fd);
```
----

Interprocess communication in MINIX 3 uses pipes.  

When a user types  
```cat file1 file2 | sort```  
the shell creates a pipe and arranges for standard output of the first process to write to the pipe, so standard input of the second process can read from it.  

The ```pipe``` system call creates a pipe and returns two file descriptors, one for writing and one for reading.  

the call is ```pipe(&fd[0])```  
where fd is an array of two integers and ```fd[0]``` is the file descriptor for reading and ```fd[1]``` is the one for writing.  
Typically, a ```fork``` comes next, and the parent closes the file descriptor for reading and the child closes the file descriptor for writing (or vice versa), so when they are done, one process can read the pipe and the other can write on it.  

the code below depicts a skeleton procedure that create two processess, with the output of the first
one piped into the second one:  
```c=
void pipeline(char* process1, char* process2) /* pointers to program names */
{
    int fd[2];
    pipe(&fd[0]); /* create a pipe */
    
    if (fork() != 0) {
        /* The parent process executes these statements. */
        close(fd[0]); /* process 1 does not need to read from pipe */
        close(STDOUT_FILENO); /* prepare for new standard output */
        dup(fd[1]); /* set standard output to fd[1] */
        close(fd[1]); /* this file descriptor not needed any more */
        
        execl(process1, process1, 0);
    } else {
        /* The child process executes these statements. */
        close(fd[1]); /* process 2 does not need to write to pipe */
        close(STDIN_FILENO); /* prepare for new standard input */
        dup(fd[0]); /* set standard input to fd[0] */
        close(fd[0]); /* this file descriptor not needed any more */
        
        execl(process2, process2, 0);
    }
}
```

### system call for directory management
The first two calls, ```mkdir``` and ```rmdir```, create and remove empty directories, respectively. The next call is ```link``` . Its purpose is to allow the same file to appear under two or more names, often in different directories.  
Sharing a file is not the same as giving every team member a private copy, because having a shared file means that changes that any member of the team makes are instantly visible to the other members, which imply there's only one file.  

it's a illustration below for the system call ```link("/usr/jim/memo", "/usr/ast/note");```  
![](https://i.imgur.com/vPUxRKz.png)  
Understanding how ```link``` works will probably make it clearer what it does. Every file in UNIX has a unique number, its i-number, that identifies it.  
This inumber is an index into a table of __i-nodes__, one per file, telling who owns the file, where its disk blocks are, and so on.  
A directory is simply a file containing a set of (i-number, ASCII name) pairs.  

the ```mount``` system call allows two file systems to be merged into one.  
By executing the mount system call, the CD-ROM file system can be attached to the root file system, as shown below:  
![](https://i.imgur.com/Fye9gjq.png)  

After the ```mount``` call, a file on CD-ROM drive 0 can be accessed by just using its path from the root directory or the working directory, without regard to which drive it is on.  
In fact, second, third, and fourth drives can also be mounted anywhere in the tree.  
When a file system is no longer needed, it can be unmounted with the ```umount``` system call.  

MINIX 3 maintains a __block cache__ of recently used blocks in main memory to avoid having to read them from the disk if they are used again quickly.  
If a block in the cache is modified (by a write on a file) and the system crashes before the modified block is written out to disk, the file system will be damaged.  
To limit the potential damage, it is important to flush the cache periodically, so that the amount of data lost by a crash will be small.  
The system call ```sync``` tells MINIX 3 to write out all the cache blocks that have been modified since being read in. When MINIX 3 is started up, a program called _update_ is started as a background process to do a ```sync``` every 30 seconds, to keep flushing the cache.  

Two other calls that relate to directories are ```chdir``` and ```chroot``` . The former changes the working directory and the latter changes the root directory.  

### system calls for protection
In MINIX 3 every file has an 11-bit mode used for protection. the lower nine of these bits are the readwrite-execute bits for the owner, group, and others. The ```chmod``` system call makes it possible to change the mode of a file.  
The other two protection bits are the S_ISGID (set-group-id) and S_ISUID (set-user-id) bits.  
as shown below:  

| mode    | octal | description                |
| ------- | ----- | -------------------------- |
| S_ISUID | 04000 | execute in the owner's UID |
| S_ISGID | 02000 | execute in the file's GID  |
| S_ISVTX | 01000 | only the owner or root can remove or move this file if the flag set on (this property wasn't supported by MINIX 3 until 2010, so the book say "11-bit") |
| S_IRUSR, S_IREAD| 00400| owner readable|
| S_IWUSR, S_IWRITE|00200| owner writeable|
| S_IXUSR, S_IEXEC| 00100| owner executable|
| S_IRGRP | 00040 | group readable             |
| S_IWGRP | 00020 | group writeable            |
| S_IXGRP | 00010 | group executable           |
| S_IROTH | 00004 | else readable              |
| S_IWOTH | 00002 | else writeable             |
| S_IXOTH | 00001 | else executable            |

When any user executes a program with the S_ISUID bit on, for the duration of that process the user's effective UID is changed to that of the file's owner.  
This feature is heavily used to allow users to execute programs that perform superuser only functions, such as creating directories.  

When a process executes a file that has the S_ISUID or S_ISGID bit on in its mode, it acquires an effective UID or GID different from its real UID or GID.  
It is sometimes important for a process to find out what its real and effective UID or GID is.  
so four library routines are needed to extract the proper information: ```getuid``` , ```getgid``` , ```geteuid``` , and ```getegid```.  
The first two get the real UID/GID, and the last two the effective ones.  

Ordinary users cannot change their UID, except by executing programs with the ```S_ISUID``` bit on, but the superuser has another possibility: the ```setuid``` system call, which sets both the effective and real UIDs. ```setgid``` sets both GIDs.  
The superuser can also change the owner of a file with the ```chown``` system call.  

The last two system calls in this category can be executed by ordinary user processes. The first one, ```umask``` , sets an internal bit mask within the system, which is used to mask off mode bits when a file is created.  

After the call  
```umask(022);```  
the mode supplied by ```creat``` and ```mknod``` will have the 022 bits masked off before being used. Thus the call  
```creat("file", 0777);```
will set the mode to 0755 rather than 0777.  
Since the bit mask is inherited by child processes, if the shell does this umask just after login, none of the user's processes in that session will accidently create files that other people can write on.  

When a program owned by the root has the ```S_ISUID``` bit on, it can access any file, because its effective UID is the superuser.  
Frequently it is useful for the program to know if the person who called the program has permission to access a given file. If the program just tries the ```access```, it will always succeed, and thus learn nothing.

What is needed is a way to see if the access is permitted for the real UID. The ```access``` system call provides a way to find out.  
```int access(const char *pathname, int mode);``` checks whether the calling process can access the file.  
The mode parameter is 4 to check for read access, 2 for write access, and 1 for execute access. Combinations of these values are also allowed. (just like rwx-bit)  
For example, with mode equal to 6, the call returns 0 if both read and write access are allowed for the real ID; otherwise -1 is returned.  
With mode equal to 0, a check is made to see if the file exists and the directories leading up to it can be searched.

## os structure
### monolithic system
The structure is that there is no structure. The operating system is written as a collection of procedures, each of which can call any of the other ones whenever it needs to.  

To construct the actual object program of the operating system when this approach is used, one first compiles all the individual procedures, or files containing the procedures, and then binds them all together into a single object file using the system linker. In terms of information hiding, there is essentially noneevery procedure is visible to every other procedure.  

Even in monolithic systems, however, it is possible to have at least a little structure.  
The services (system calls) provided by the operating system are requested by putting the parameters in well defined places, such as in registers or on the stack, and then executing a special trap instruction known as a __kernel call__ or __supervisor call__.  

This instruction switches the machine from user mode to kernel mode and transfers control to the operating system. (Most CPUs have two modes: kernel mode, for the operating system, in which all instructions are allowed; and user mode, for user programs, in which I/O and certain other instructions are not allowed.)  


for example:  
```count = read(fd, buffer, nbytes);```  
![](https://i.imgur.com/ljaMIn5.png)

In preparation for calling the read library procedure, which actually makes the ```read``` system call, the calling program first pushes the parameters onto the stack, as shown in steps 13 in Fig. above.  
The first and third parameters are called by value, but the second parameter is passed by reference, meaning that the address of the buffer (indicated by &) is passed, not the contents of the buffer.  
Then comes the actual call to the library procedure (step 4). This instruction is the normal procedure call instruction used to call all procedures.

The library procedure, possibly written in assembly language, typically puts the system call number in a place where the operating system expects it, such as a register (step 5).  
Then it executes a ```trap``` instruction to switch from user mode to kernel mode and start execution at a fixed address within the kernel (step 6).  
The kernel code that starts examines the system call number and then dispatches to the correct system call handler, usually via a table of pointers to system call handlers indexed on system call number (step 7).  
At that point the system call handler runs (step 8).  
Once the system call handler has completed its work, control may be returned to the user-space library procedure at the instruction following the trAP instruction (step 9).  
This procedure then returns to the user program in the usual way procedure calls return (step 10).
To finish the job, the user program has to clean up the stack, as it does after any procedure call (step 11).  

This organization suggests a basic structure for the operating system:  
1. A main program that invokes the requested service procedure.
2. A set of service procedures that carry out the system calls.
3. A set of utility procedures that help the service procedures.

In this model, for each system call there is one service procedure that takes care of it. The utility procedures do things that are needed by several service procedures, such as fetching data from user programs.  
This division of the procedures into three layers is shown below.  
![](https://i.imgur.com/4u7ppaL.png)  

and that induce the next system:

### layered systems
A generalization of the monolithic system is to organize the operating system as a hierarchy of layers, each one constructed upon the one below it.  

![](https://i.imgur.com/pxxSRS1.png)  
The system had 6 layers.  
Layer 0 dealt with allocation of the processor, switching between processes when interrupts occurred or timers expired. Above layer 0, the system consisted of sequential processes, each of which could be programmed without having to worry about the fact that multiple processes were running on a single processor. In other words, layer 0 provided the basic multiprogramming of the CPU.  

Layer 1 did the memory management. It allocated space for processes in main memory and on a 512K word drum(an obsolete memory device) used for holding parts of processes (pages) for which there was no room in main memory.  
Above layer 1, processes did not have to worry about whether they were in memory or on the drum; the layer 1 software took care of making sure pages were brought into memory whenever they were needed.  

Layer 2 handled communication between each process and the operator console.  
Above this layer each process effectively had its own operator console.  

Layer 3 took care of managing the I/O devices and buffering the information streams to and from them.  
Above layer 3 each process could deal with abstract I/O devices with nice properties, instead of real devices with many peculiarities.  

Layer 4 was where the user programs were found. They did not have to worry about process, memory, console, or I/O management.  

The system operator process was located in layer 5.  

A further generalization of the layering concept was present in the MULTICS system.  
Instead of layers, MULTICS was organized as a series of concentric rings, with the inner ones being more privileged than the outer ones. When a procedure in an outer ring wanted to call a procedure in an inner ring, it had to make the equivalent of a system call, that is, a TRAP instruction whose parameters were carefully checked for validity before allowing the call to proceed.  
Although the entire operating system was part of the address space of each user process in MULTICS, the hardware(Pentium) made it possible to designate individual procedures (memory segments, actually) as protected against reading, writing, or executing.  

### virtual machines
This system, originally called CP/CMS and later renamed VM/370 (Seawright and MacKinnon, 1979), was based on a very astute observation: a timesharing system provides  
1. multiprogramming  
2. an extended machine with a more convenient interface than the bare hardware.  

The essence of VM/370 is to completely separate these two functions.  

The heart of the system, known as the __virtual machine monitor__, runs on the bare hardware and does the multiprogramming, providing not one, but several virtual machines to the next layer up.  
However, unlike all other operating systems, these virtual machines are not extended machines, with files and other nice features.  
Instead, they are exact copies of the bare hardware, including kernel/user mode, I/O, interrupts, and everything else the real machine has.  

Because each virtual machine is identical to the true hardware, each one can run any operating system that will run directly on the bare hardware.  
Different virtual machines can, and frequently do, run different operating systems. Some run one of the  escendants of OS/360 for batch or transaction processing, while others run a single-user, interactive system called CMS(Conversational Monitor System) for timesharing users.  

When a CMS program executes a system call, the call is trapped to the operating-system in its own virtual machine, not to VM/370, just as it would if it were running on a real machine instead of a virtual one.  
CMS then issues the normal hardware I/O instructions for reading its virtual disk or whatever is needed to carry out the call.  
These I/O instructions are trapped by VM/370, which then performs them as part of its simulation of the real hardware.  

By making a complete separation of the functions of multiprogramming and providing an extended machine, each of the pieces can be much simpler, more flexible, and easier to maintain.  

Several virtual machine implementations are marketed commercially.  
For companies that provide web-hosting services, it can be more economical to run multiple virtual machines on a single fast server (perhaps one with multiple CPUs) than to run many small computers, each hosting a single Web site.  
VMWare and Microsoft's Virtual PC are marketed for such installations. These programs use large files on a host system as simulated disks for their guest systems. To achieve efficiency they analyze guest system program binaries and allow safe code to run directly on the host hardware, trapping instructions that make operating system calls.  

Another are a where virtual machines are used, but in a somewhat different way, is for running Java programs.  
When Sun Microsystems invented the Java programming language, it also invented a virtual machine (i.e., a computer architecture) called the __JVM (Java Virtual Machine)__.  
The Java compiler produces code for JVM, which then typically is executed by a software JVM interpreter. The advantage of this approach is that the JVM code can be shipped over the Internet to any computer that has a JVM interpreter and run there.  

Another advantage of using JVM is that if the  interpreter is implemented properly, which is not completely trivial, incoming JVM programs can be checked for safety and then executed in a protected environment so they cannot steal data or do any damage.  

### exokernels
With VM/370, each user process gets an exact copy of the actual computer.  
With virtual 8086 mode on the Pentium, each user process gets an exact copy of a different computer.  
Going one step further, researchers at M.I.T. built a system that gives each user a clone of the actual computer, but with a subset of the resources (Engler et al., 1995; and Leschke, 2004).  
Thus one virtual machine might get disk blocks 0 to 1023, the next one might get blocks 1024 to 2047, and so on.  

At the bottom layer, running in kernel mode, is a program called the __exokernel__. Its job is to allocate resources to virtual machines and then check attempts to use them to make sure no machine is trying to use somebody else's resources.  
Each user-level virtual machine can run its own operating system, as on VM/370 and the Pentium virtual 8086s, except that each one is restricted to using only the resources it has asked for and been allocated.  

The advantage of the exokernel scheme is that it saves a layer of mapping. The exokernel need only keep track of which virtual machine has been assigned which resource.

### client-server Model
A trend in modern operating systems is to take this idea of moving code up into higher layers even further and remove as much as possible from the operating  system, leaving a minimal __kernel__.  
The usual approach is to implement most of the operating system functions in user processes. To request a service, such as reading a block of a file, a user process (now known as the __client process__) sends the request to a __server process__, which then does the work and sends back the answer.  

![](https://i.imgur.com/UGnStTK.png)
In this model, all the kernel does is handle the communication between clients and servers.  
By splitting the operating system up into parts, each of which only handles one facet of the system, such as file service, process service, terminal service, or memory service, each part becomes small and manageable.  
Furthermore, because all the servers run as user-mode
processes, and not in kernel mode, they do not have direct access to the hardware. As a consequence, if a bug in the file server is triggered, the file service may crash, but this will not usually bring the whole machine down.  

Another advantage of the client-server model is its adaptability to use in distributed systems.  
If a client communicates with a server by sending it messages, the client need not know whether the message is handled locally in its own machine, or whether it was sent across a network to a server on a remote machine.  
As far as the client is concerned, the same thing happens in both cases: a request was sent and a reply came back.  
![](https://i.imgur.com/pI5npQj.png)  

The picture painted above of a kernel that handles only the transport of messages from clients to servers and back is not completely realistic.  
Some operating system functions (such as loading commands into the physical I/O device registers) are difficult, if not impossible, to do from userspace programs.  

There are two ways of dealing with this problem.  
One way is to have some critical server processes (e.g., I/O device drivers) actually run in kernel mode, with complete access to all the hardware, but still communicate with other processes using the normal message mechanism.  

The other way is to build a minimal amount of __mechanism__ into the kernel but leave the __policy__ decisions up to servers in user space.  
For example, the kernel might recognize that a message sent to a certain special address means to take the contents of that message and load it into the I/O device registers for some disk, to start a disk read.  
In this example, the kernel would not even inspect the bytes in the message to see if they were valid or meaningful; it would just blindly copy them into the disk's device registers. (Obviously, some scheme for limiting such messages to authorized processes only must be used.)