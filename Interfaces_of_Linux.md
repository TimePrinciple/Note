### The Core Operating System: The `Kernel`:

The term *operating system* is commonly refer to the central software that manages and allocates computer resources (i.e., the CPU, RAM, and devices).

Although it is possible to run programs on a computer without a kernel, the presence of a kernel greatly simplifies the writing and use of other programs, and increases the power and flexibility available to programmers. The kernel does this by providing a software layer to manage the limited resources of a computer.

> The Linux kernel executable typically resides at the pathname `/boot/vmlinuz`.

### Tasks performed by the kernel:

- **Process scheduling**: A computer has one or more central processing unit (CPUs), which execute the instructions of programs. Like other UNIX systems, Linux is a *preemptive multitasking* operating system. *Multitasking* means multiple processes (i.e., running programs) can simultaneously reside in memory and each may receive use of the CPU(s). *Preemptive* means that the rules governing which processes receive use of the CPU and for how long are determined by the kernel process scheduler (rather than by the processes themselves).

- **Memory management**: Physical memory (RAM) remains a limited resource that the kernel must share among processes in an equitable and efficient fashion. Linux employs `virtual memory management`, a technique that confers two main advantages:

  - Processes are isolated from one another and from the kernel, so that one process can't read or modify the memory of another process or the kernel.

  - Only part of a process needs to be kept in memory, thereby lowering the memory requirements of each process and allowing more processes to be held in RAM simultaneously. This leads to better CPU utilization, since it increases the likelihood that, at any moment in time, there is at least one process that the CPU(s) can execute.

- **Provision of a file system**: The kernel provides a file system on disk, allowing files to be created, retrieved, updated, deleted, (CRUD) and so on.

- **Creation and termination of processes**: The kernel can load a new program into memory, providing it with the resources (e.g., CPU, memory, and access to files) that it needs in order to run. Such an instance of a running program is termed a *process*. Once a process has completed execution, the kernel ensures that the resources it uses are freed for subsequent reuse by later programs.

- **Access to devices**: The devices (mice, monitors, keyboards, disk and tape drives, and so on) attached to a computer allow communication of information between the computer of the outside world, permitting input, output, or both. The kernel provides programs with an interface that standardizes and simplifies access to devices, while at the same time arbitrating access by multiple processes to each device.

- **Networking**: The kernel transmits and receives network messages (packets) on behalf of user processes. This task includes routing of network packets to the target system.

- **Provision of a system call application programming interface (API)**: Processes can request the kernel to perform various tasks using kernel entry points known as *system call*. The Linux system call API is the primary topic of this book.

- **Virtual private computer**: Each user can log on to the system and operate largely independently of other users. For example, each user has their own disk storage space (home directory). In addition, users can run programs, each of which gets a share of the CPU and operates in its own virtual address space, and these programs can independently access devices and transfer information over the network. The kernel resolves potential conflicts in accessing hardware resources, so users and processes are generally unaware of the conflicts.

#### Two Modes:

*user mode* and *kernel mode*: Hardware instructions allow switching from one mode to the other. Correspondingly, areas of virtual memory can be marked as being part of *user space* or *kernel space*. When running in user mode, the CPU can access only memory that is marked as being in user space; attempts to access memory in kernel space result in a hardware exception. When running in kernel mode, the CPU can access both user and kernel memory space.

#### Process view versus kernel view:

- Process view: A running system typically has numerous processes. For a process, many thing shappen asynchronously. An executing process doesn't know when it will next time out, which other processes will then be scheduled for the CPU (and in what order), or when it will next be scheduled. The delivery of signals and the occurrence of interprocess communication events are mediated by the kernel, and can occur at any time for a process. Many things happen transparently for a process. A process doesn't know whre it is located in RAM or, in general, whether a particular part of its memory space is currently resident in memory or held in the swap area (a reserved area of disk space used to supplement the computer's RAM). Similarly, a process doesn't know where on the disk drive the files it accesses are being held; it simply refers to the files by name. A process operates in isolation; it can't directly communicate with another process. A process can't itself create a new process or even end its own existence. Finally, a process can't communicate directly with the input and output devices attached to the computer.

- Kernel view: A running system has one kernel that knows and controls everything. The kernel facilitates the running of all processes on the system. The kernel decides which process will next obtain access to the CPU, when it will do so, and for how long. The kernel maintains data structures containing information about all running processes and updates these structures as processes are created, change state, and terminate. The kernel maintains all of the low-level data structures that enable the filenames used by programs to be translated into physical locations on the disk. The kernel also maintains data structures that map the virtual memory of each process into the physical memory of the computer and the swap area(s) on disk. All communication between processes is done via mechanisms provided by the kernel. In response to requests from processes, the kernel creates new processes and terminates existing processes. Lastly, the kernel (in particular, device drivers) performs all direct communication with input and output devices, transferring information to and from user processes as required.

Things such as "a process can create another process", "a process can create a pipe", "a process can write data to a file" and "a process can terminate a by calling *exit()*". The `kernel` mediates all such actions, and these statements are just shorthand for "a process can *request the kernel* create another process", and so forth.

#### The shell

A shell is a special-purpose program designed to read commands types by user and execute appropriate programmes in response to those commands. Such a program is sometimes known as a *command interpreter*. 

#### Users

Every user of the system has a unique *login name* (username) and a corresponding numeric *user ID* (UID). For each user, these are defined by a line in the system *password file*, `/etc/passwd`, which includes:

- *Group ID*: the numeric group ID of the first of the groups of which the user is a member.

- *Home directory*: the initial directory into which the user is placed after logging in.

- *Login shell*: the name of the program to be executed to interpret user commands.

#### Groups

For administrative purpose, in particular, for controlling access to files and other system resources, it is useful to organize users into *groups*. For example, the people in a team working on a single project, and thus sharing a common set of files, might all be made members of the same group. Each group is identified by a single line in the system *group file*, `/etc/group`, which includes:

- *Group name*: the (unique) name of the group.

- *Group ID (GID)*: the numeric ID associated with this group.

- *User list*: a comma-seperated list of login names of users who are members of this group.
