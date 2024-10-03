**Mod 1**

  **OS FUNDAMENTALS**
        The 10.0.0.0/8 subnet is used for range management and configuration.
        WARNING: Do not interact with this subnet or it may cause an error in the range.
        There are several files, directories, and processes that are related to normal range operation.
        These items are off-limits and should not be interfered with. If uncovered within the context of an investigation, these items can be safely ignored, as they are not relevant to the investigation.
        systeminit.exe and all files related to this binary
        java.exe listening on port 49999 and 49998
        java.exe communicating over port 5762, 15672, & 27017
        Advanced Message Queuing Protocol (AMQP) listening on port 5672
        Software or files located in C:\Program Files (x86)\Lincoln\
        Software or files located in C:\ProgramData\staging\
        Software or files located in C:\Program Files\Puppet Labs\
        Software or files located in C:\ProgramData\PuppetLabs\
        Software or files located in C:\Program Files (x86)\SimSpace\
        ruby.exe on Windows and ruby on Linux

  **OS architecture**
          two modes of operation: user mode and kernel space
          user mode - code being executed only runs a limited set of safe instructions and cannot directly access any of the systems hardware.
          kernel mode - code being executed by the CPU has unrestricted access to all of the systems hardware.

  **System calls** - a program running in user mode needs to access the systems hardware in order to function. Client applications must ask the OS for help b y making a systemcall.
          User mode process asks for privilege to run something by making a system call.

**library call vs system call**
  The main difference between system and library calls is that system calls are specifically used to ask the OS to perform an action that requires kernel mode execution, and as such, causes a software interrupt in the CPU. 
  Library calls do not necessarily perform any actions within kernel mode, and instead simply abstract away the implementation of a task.
  DLL's / library - multiple applications can use the same library / DLL

  **Protection rings**
  ![image](https://github.com/user-attachments/assets/62430e52-9ebc-49ab-a79e-7e154a02b90e)
  kernel - ring 0
  device drivers - ring 1
  device drivers - ring 2
  applications - ring 3

  when lower priv rings desire access to higher priv, they use a system / library call.

  **Kernel**
  kernel is in charge of:
    process management
    memory management
    file management
    peripheral and input/output device management
    security management

  **process management**
   a program - is a set of human readable instructions that are intended for eventual execution on a computer. programs are often referred to as source code.
   
   an executable - also known as an imagine, is a type of file that contains machine instructions that a CPU can actually execute; these instructions are written in a low-level machine language, like assembly.
   The process of compiling a program involves the translation of the human-readable program source code into corresponding machine instructions, which are then saved as an executable file.
   
   a process - is an instance of a running executable that the kernel has loaded into memory and started executing.
   several processes can be instantiated from the same executable, though they do not need to be in the same state - for example, two separate web browser windows can be open to different websites at the same time.
   the kernel assigns system resources, such as memory, to each process.
   
   a thread - is the smallest unit within a process that can be assigned for execution on the CPU. A process may contain multiple threads, each of which share the pool of system resources that the kernal has allocated to the process.
   in systems that support it, multithreading or multitasking may allow a process to execute multiple threads.

**process structure in memory**
During process creation, the OS allocates a block of memory to be used by that process. All of that process’s execution of code operations occurs within this memory block, which is divided into five main parts: text, data, Block Started by Symbol (BSS), heap, and stack. An additional piece of a process’s memory is reserved for use by the OS, and is used when the OS takes operations to manage that process.

- Text: Contains the machine instructions loaded from the executable. This is the actual set of instructions that the CPU executes.
- Data: Stores any variables that were initialized by the programmer or compiler. The initialization process requires the compiler to know how much memory is needed to store the variable, as well as the initial, non-zero value of the variable.
- BSS: Used to store variables that have been instantiated, but not initialized. This means that the compiler is able to determine how much memory is needed to store a variable, but the program has not set its value, or has set its value explicitly to zero.
- Heap: Used to dynamically allocate memory for variables whose sizes cannot be known until runtime, i.e., the actual execution of the program. This usually occurs when the size of the variable differs each time the program is run, so the compiler cannot assign a chunk of memory to the variable right off the bat. The heap can freely grow and shrink during program execution.
- Stack: Last In, First Out (LIFO) data structure used to store stack frames, which contain information about the current  execution sta te of the process. If a process has multiple threads, each thread is given its own stack. As an example, imagine that a thread is currently executing function1(), which contains a call to another function, function2(). When the call to function2() occurs, the current state of function1() is saved into the stack frame for function1() and placed on top of the stack. Then, a new stack frame is created to initialize function2(), itself being placed on top of the stack. When function2() eventually completes, execution is returned to function1() by loading the saved function1() stack frame, which includes loading the code within function1() that lies directly after the call to function2().

![image](https://github.com/user-attachments/assets/7b04d05e-3b6b-4126-9bd7-7b4190d94a73)

**Lifecycle of a Process**
The OS is responsible for creating, managing, and scheduling processes for execution on the CPU as they proceed throughout their lifecycle.
he management of process execution on a system relies heavily on a process’s state. There are five main states that a process can be in, though specific OSs may use more granular states than those described.

Five main states a process can be in - 

- When in the Created or New state, processes are waiting to be loaded into main memory (Random Access Memory [RAM]). The OS has not yet allocated system resources to the process, or is just beginning to allocate these resources.
- A process in the Ready or Waiting state has been fully loaded and is waiting to be scheduled for execution. Multiple processes are generally in this state at a given time.
- A Running process is one that is currently running on one of the CPU’s hardware cores, in either kernel mode or user mode. Processes scheduled to run in the CPU do so for a short while, before being swapped out with another process that is in the Ready state. A Running process that is swapped out of the CPU generally returns to the Ready state.
- A Blocked process is waiting for an event that is outside of its control to occur. For example, a process that is waiting for an incoming network connection might be in the Blocked state while it waits for the connection to occur. Upon receiving the connection, the OS moves that process from Blocked to Running so it can handle the incoming connection.
- A Terminated process has completed its execution, or was explicitly killed by some other process or system action. Killing a process is a common phrase when discussing process termination.

  **process termination complications**
  
  A terminated process must wait for its parent process to read its exit status before the OS can free its assigned Process Identifier (PID) for use elsewhere.
  Until the parent reads its child’s exit status, the child process continues to exist in the Terminated state.
  The name for such a process is a zombie process. Depending on the specifics of the parent process, a zombie process may continue to exist indefinitely.

  An orphan process is a child process whose parent process has terminated before it. This may lead to the child process being adopted by a new parent process — usually the root process of the system or another process specifically delegated by the OS to adopt orphan processes — but not always.


**Process Identifiers**

When a process is created — also referred to as spawning a process — the OS assigns that process a number in order to identify that process within the system. This number is known as a PID. One key thing to note about PIDs is that they are unique only for currently executing processes within a single system — two different systems may assign the same PIDs to different processes, and a system may reuse a PID if it is not currently being used.

**Daemon Processes**

The root process is responsible for loading every other process necessary for the system to function. Usually, the root process delegates some of these responsibilities by spawning other processes known as daemon processes, or daemons, which manage certain components across the entire OS. For example, a network daemon might handle all network communication for the OS. Daemons usually run in the background and do not require direct interaction with a system user. In fact, daemons generally run continuously, even if no account is logged into the system.


On Windows machines, daemon processes are referred to as services.

**Windows | core processes**

![image](https://github.com/user-attachments/assets/b6dff871-7fe9-442f-beeb-835ca963d29f)

https://rcs08-minio.pcte.mil/portal-bucket/portal/documentation/learning-app-event/8939acb1-deb4-4502-bbab-138332148370/SANS%20Find%20Evil%20-%20Know%20Normal.pdf?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=PCTE-SimSpaceCorp%2F20241002%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241002T172624Z&X-Amz-Expires=86400&X-Amz-SignedHeaders=host&response-content-disposition=inline&X-Amz-Signature=99d0d3332d19e0382d277b527aa3d50ae9537a812a27d3572ad0c22b3a796693

** Core Processes | Linux **

**Runlevels**

In legacy systems that still use the init process, runlevels are used to define this desired state. Standard runlevels are defined between 0–6 — each number defines a different state for the system. For example, runlevel 6 is commonly used to reboot the system.

When the system is booted, the init process loads a single runlevel — usually the default level configured by the system — by referencing that runlevel’s associated /etc/rc#.d/ directory, (where # is replaced with the runlevel’s number). This directory contains a list of links to specific scripts located within /etc/init.d/; these scripts are executed sequentially and are used to start or stop various processes required for the system to boot into a specific runlevel. After the machine has been fully booted, the system’s runlevel can be changed on the fly, causing the system to reference the /etc/rc#.d/ directory for the desired runlevel and begin the process anew. Runlevel scripts can be added by a system administrator in order to further configure the system’s functionality.

**systemd Targets**

Instead of runlevels, systemd loads services that are organized into targets. Though the term target is approximately analogous to the term runlevel, targets and services offer an incredibly large degree of control over the system. A service may be as simple as a one-for-one recreation of a runlevel script, but can include advanced control routines that test for dependencies prior to service execution, such as whether networking is enabled. Runlevels must execute scripts in a linear order, while systemd loads any services in parallel that it can. Additionally, systemd can use sequential targets as a series of checkpoints, which makes sure that the system has entered a predefined state before continuing to load and execute additional targets. Similarly to runlevels, targets can also be changed once the system has booted in order to switch system states.

**Analyzing linux processes**

-  UID column: Illustrates that users are associated with each process, which is also true within Windows systems. The Unix root user, by default, is allowed to access everything within the system, which includes the ability to execute things within kernel mode in the CPU.
-  PPID column: Shows the PPID of all the processes on the system. The red arrows at the bottom show the parent/child relationships between several processes involved in the execution of the ps command.
- CMD column, bracketed items: Items surrounded by brackets indicate that the processes are executing in kernel mode within the CPU. The very first item in this list is kthreadd — the kernel thread daemon — which is responsible for handling the execution of kernel mode threads. It is no surprise that other kernel threads list kthreadd (PID 2) as their parent process!
- CMD column, non-bracketed items: Items not surrounded by brackets indicate processes executing in user mode within the CPU. The highlighted area shows the results of supplying the -H option to the ps command — processes are displayed in a hierarchical relationship, with child processes having their names tabulated underneath their parent process.

**Memory Management**

During process creation, the kernel allocates memory to a process — this memory contains the stack, heap, data, text, and BSS sections as described earlier. The kernel is responsible for managing the usage of most of this memory during the lifecycle of the process. When a process terminates, the kernel is responsible for deallocating any memory that was used by the process, freeing it up to be reallocated to other processes.

** Virtual Memory ** - allows every process to think that it has the full memory capability

Virtual memory is a memory management technique used by modern OSs in order to more effectively manage and secure this key system resource. During process creation, a program and any system libraries it relies on are loaded into the process’s virtual memory. When this process later needs to access or change data held in its virtual memory, it does so via a virtual memory address — the location of the data in its virtual memory. Dedicated hardware known as the Memory Management Unit (MMU) translates these virtual memory addresses into physical memory addresses — actual locations within physical memory such as RAM. To the process, its virtual memory is always present and is always arranged contiguously. 


**Page Files and Swap Files** page file on windows side, swap file on nix side.

Windows stores all inactive memory pages inside a page file named pagefile.sys. The default location of this file is located in the root directory of the disk (i.e., C:\pagefile.sys).

Linux instead uses the term swap space to refer to the locations on the disk where inactive memory pages (also known as swap files) are stored. Two separate swap spaces exist. The swap partition is a special location for swapping operations that is reserved directly by the filesystem — no files other than swap files are allowed to exist within the swap partition. Linux also supports the creation of generic swap files within accessible portions of the filesystem. These generic swap files can be created by a system administrator to allow for more swap space.

Page and swap files are important, since they contain sections of a process’s virtual memory. When performing a forensic or security investigation of a system, these files may serve as a useful evidence source.



**File Management**

Many processes require the ability to read and write files to a long-term storage medium, such as a hard drive. In order to be used by the system, such storage media must be formatted into a filesystem, which defines a format for organizing, storing, and tracking files. To do this, the filesystem tracks some metadata information about each file, in addition to that file’s actual contents. The tracking of this metadata is done within a record for each file present within the filesystem. Additionally, filesystems maintain a directory to quickly look up the record of each file in the filesystem. A filesystem's directory and list of records exist in a reserved portion of the filesystem. Many filesystem implementations exist; they differ in size, speed, security, functionality, and other factors.


**Filesystem Internals**

When the kernel receives a request to access a file, it searches through the filesystem’s directory for the appropriately named file, using the directory to determine the location of that file’s associated record in a record list. From the file’s record, the kernel can retrieve metadata information about the file, which includes the physical location of the file’s raw data held within the computer’s hard drive. Figure 1.1-33 shows how the kernel might look for a file in a filesystem, though the exact implementation can differ wildly depending on the filesystem.


**Metadata in Filesystems**

Filesystem metadata is stored within a file’s record. This metadata may include the file’s physical location on disk, its size, any associated permissions, and a set of timestamps describing when a file was last modified, accessed, or created. Different filesystem implementations keep track of different metadata information.


Filesystem metadata is a fantastic source of information about the system. Later in this lesson, filesystem metadata is used as an evidence source for a simple investigation.


**Virtual Filesystems**

A Virtual Filesystem (VFS) is an abstract layer that lies between client applications and the concrete filesystem. A VFS is usually implemented as a kernel module within the OS kernel, and defines a common language that client applications and filesystems can use to communicate, regardless of the specifics of the underlying filesystem. When a client application wants to interact with a filesystem, it uses a system call to ask the kernel for help; the kernel then safely interacts with the appropriate VFS, which performs one last translation step in order to communicate with the concrete filesystem. This offers a key advantage to software developers: rather than writing a ton of extra code to support many different filesystems, the program simply needs to be able to pass file operation instructions to the kernel via a system call.


**Types of Filesystems**
**Windows Filesystems**

File Allocation Table


File Allocation Table (FAT) is a basic filesystem that uses a specific construct to reference files stored on a disk. FAT comes in several variants, the oldest of which, FAT8, has been around since the 1970s. FAT12, FAT16, and FAT32 are updated versions of the FAT filesystem, and support increasingly larger disk sizes, single file sizes, and longer filenames. FAT12 was used in the very first version of Microsoft Disk Operating System (DOS), and was later replaced with FAT16 and FAT32 in newer Windows OSs. Windows switched away from using the FAT filesystem as a default filesystem with the release of Windows NT 3.1 in 1993.


FAT32 is still used today as the default filesystem for many types of removable media devices, such as flash drives, SD Cards, and external hard disk drives. This is because FAT32 is supported by a wide variety of different systems, which makes it a natural choice for use on removable media storage devices.


Notably, FAT filesystems have no built-in security mechanisms; FAT filesystems do not hold any information about the permissions that should be associated with each file.


**New Technology File System**

New Technology File System (NTFS) is a journaling filesystem originally developed by Microsoft to replace FAT in Windows NT 3.1. NTFS has undergone several revisions during its lifetime, and provides several enhancements to filesystem performance, efficiency, capacity, resilience, and security. NTFS is still the most common filesystem in use on Windows OSs.

Another big reason behind the change from FAT to NTFS is that NTFS natively implements security mechanisms that allow for access controls to be applied directly within the filesystem. NTFS contains two Access Control Lists (ACL), which are associated with each file or directory; the Discretionary Access Control List (DACL) and the System Access Control List (SACL). The DACL is responsible for implementing basic permissions that define which users and/or groups can perform actions, reading, writing, execution, or deletion of a file or folder. The SACL is responsible for determining how the system should audit attempted actions being performed against a file or folder as well as whether those actions succeeded or not. For example, the SACL can be used to log the username of anyone attempting to delete an important file.


**Resilient File System**

Resilient File System (ReFS) is another filesystem developed by Microsoft, intended to be the next generation Windows filesystem to replace NTFS. According to Microsoft, ReFS was designed to maximize data availability, scale efficiently to large data sets across diverse workloads, and provide data integrity with resiliency to corruption. It seeks to address an expanding set of storage scenarios and establish a foundation for future innovations. ReFS was originally released as part of the Windows Server 2012 OS, but has still not seen widespread adoption.


**Index Nodes**

The Linux kernel supports a large variety of filesystems compared to other OSs. This is because of a construct called the index node (inode) that exists as an abstraction layer within the Linux kernel. Inodes are an integral part of the Linux kernel; all file management behavior within Linux OSs deals directly with these inodes, requiring only that the Linux VFS perform small translation steps between the Linux kernel and the underlying concrete filesystem.


The behaviors made available by inodes include the creation of hard links and symbolic links, which themselves can be used in fairly interesting ways.


**Hard Links**

When a regular file is created on a Linux machine, the filesystem creates a new record for the file within its record list, and provides a unique ID number back to the Linux kernel. The kernel uses this ID number as the inode ID; the inode ID number is used by the kernel to perform operations against the file located on the filesystem.


Making a hard link to a file creates a new file as normal, but instead of its own inode, the hard link directly references the inode of a different file. Follow the steps below to explore the properties of hard links.


**Symbolic Links** - adversaries are more likely to manipulate sym links. Sym links reference using name/location, hard links reference using inode.

Symbolic links, also known as symlinks or softlinks, reference a file by that file’s name and location on the filesystem, rather than its inode value. Advantages of symlinks include creating a link to directory locations, and to files or directory locations located on a completely different filesystem. This is in contrast to hard links, which can only reference a file as directory locations do not have their own inode values. Additionally, hard links can only reference files within a single filesystem, since they rely on the filesystem’s internal inode value.


Unfortunately, symlinks do have a drawback; because a symlink does not reference an inode value, moving or deleting the target of the symlink (i.e., the file or directory location that the symlink is referencing) breaks the symlink.


Explore the properties of symlinks by following the steps below. These steps should be performed on the cda-kali-hunt machine after running the cd ~/tmp/ command.

**Network management**
**Sockets**
 A socket is a virtual construct that acts as the endpoint for a communication link on the system. Many sockets can be open on the system at a given time. A client application that is running on the system can request a socket from the kernel, and use system calls to read or write data to the socket.

 **Socket System Calls**
 - bind: The application requests the kernel to bind a previously instantiated socket to a network port or to a local file. Binding to a local file is used for communication within the local system only. The bind syscall must be used if the socket intends to listen for incoming connections, and does not need to be used otherwise.
 - listen: The application puts the socket into a listening state, meaning that the client application using the socket is actively ready to handle incoming connections. When an incoming connection is received, the application must choose to accept the connection or terminate it.
 - accept: The application accepts an incoming connection to a listening socket. This does not affect the listening socket; a new socket object is created to handle the accepted connection. The accept syscall is mainly used by a listening socket wishing to establish a Transmission Control Protocol (TCP) connection; it is not used for User Datagram Protocol (UDP) communications.
 - connect: The application uses the socket to establish a connection with a different listening socket, which may be on the local system or located on some external network. To connect over a network to a socket present on an external system, the socket must reference the external system’s address, (usually an Internet Protocol [IP] address), and the network port that a listening socket is bound to. The connect syscall is mainly used by a connecting socket wishing to make a TCP connection to a listening socket; it is not used for UDP communications.
 - recv or recvfrom: Short for receive. The application reads data from the socket. The recv syscall can be used as a shortcut by certain applications that have already established a connection with another socket; recvfrom is used otherwise.
 - send or sendto: The application sends data over the socket, which is transmitted to the corresponding socket on the other end of the connection. The send syscall is used as a shortcut by certain applications that have already established a connection; sendto is used otherwise.
 - close: The application closes the established connection. This may be performed by the listening socket or the connecting socket, and is mainly needed to close TCP connections; UDP communications do not maintain the concept of a connection, so there is nothing to close.

**helpful network commands**
socket information: netstat -ntu
socket statistics: ss -ntu
nic info: ifconfig


**Additional Kernel Capabilities**
Peripheral and I/O Device Management - Modern computers support many different types of peripherals, such as keyboards, mice, speakers, wireless transceivers, and others. These peripherals are generally known as Input/Output devices, since they ultimately provide either an input stream to a system (keyboards, mice, microphones, etc.), or translate a stream of system output into some other form (audio playback, printing, video display, etc.). 

Security Management - Security management is handled by a kernel component known as the security subsystem or security kernel. This portion of the kernel implements basic security procedures within the system. For example, the security kernel can determine if a particular process should be allowed to read, write, or execute a given file based on the permissions applied to the system. 

Kernel Drivers - As stated earlier, the minimum capability required for a kernel to operate is the ability to facilitate the execution of software instructions on the system’s hardware. Though this suffices for simple systems, most modern computers require additional advanced capabilities such as the ones discussed over the past few tasks. In modern OSs, many of these additional components are implemented as kernel modules or kernel drivers. These components do not exist within the core functionality of the kernel but are instead loaded and unloaded by the core kernel as needed. 

![image](https://github.com/user-attachments/assets/a173ef45-4ac1-47ff-9e44-75d1b3c1d266)

![image](https://github.com/user-attachments/assets/c0910c2d-0629-45ed-9491-8fb2860fdf7e)

![image](https://github.com/user-attachments/assets/491d27fc-3e5a-4192-a38e-b01101c34602)


**linux info**
**Everything is a file**

Unix-like OSs are built with the idea that everything is a file. While this is not true in the literal sense, it speaks to an embedded feature of various Unix-like OSs — many system resources are exposed to the system’s applications via file descriptors. These file descriptors allow other applications to interact with a large variety of different resources in the same way that they would interact with a regular file

**Types of Kernels**
Linux and Windows differ because they vary fundamentally in their approaches to OS architecture. Linux uses a monolithic kernel approach — the entire OS was designed to run in kernel mode. In contrast, Windows uses a hybrid kernel approach — the OS was designed such that many larger components, such as the environment subsystems and OS-integral subsystems, run in user mode. A third approach, known as the microkernel approach, is utilized by some other OSs. The microkernel approach moves as much functionality as possible into user mode execution — except for a very small core kernel. Each approach has distinct advantages and disadvantages, such as faster speeds or more flexibility, though discussing these pros and cons is out of scope for this lesson.

**Windows Registry**

Though it is not included within the Windows architecture diagram, Windows OSs make use of a construct called the registry to store system and user configuration information. The registry is a hierarchical database that stores information in key:value pairs, persists through system reboots by being saved to the system’s hard disk, and quickly referenced by both user mode and kernel mode processes running on the system. Information contained in the registry can be secured with a standard set of permissions; create, read, update, and delete permissions can be assigned by the owner of a particular registry key in order to limit others' access to the key.

The registry is separated into several hives, which contain nested registry keys. Registry keys contain a data type and an associated value.

**Linux Configuration files**
hese files are known as configuration files, and are used similarly to the Windows registry in order to control how a system or application functions. Configuration files are commonly located within the system’s /etc directory (for system-level configurations), within an application’s installation folder (for application-level configurations), or within a user’s home directory (for user-level configurations). Certain applications or system tasks may involve multiple configuration files that are located in several different locations throughout the system.


**Forensic Principals**

**Stages of Digital Forensics**
The digital forensics process can be divided into the following stages: 
- Identification of evidence sources
- Preservation of evidence sources
- Acquisition of data from evidence sources
- Analysis and interpretation of the acquired data
- Presentation of the analysis results


**windows files**

Recall that the Windows OS utilizes the NTFS filesystem. NTFS keeps track of four timestamps for each file record stored in the filesystem’s MFT. Three of these are accessible from the Properties tab.
- File Created: The date on which the file was first created on the filesystem. This timestamp should never change during normal operations, though there are trickier ways to force this time to update.
- File Accessed: The most recent time the file was accessed. Moving, opening, or reading metadata information about a file is considered an access and causes this value to change. Anti-virus scanners and Windows system processes, which frequently interact with files across the entire system, can also trigger this timestamp to update.
- File Modified: The most recent time that the file’s contents were modified. For example, if a text file was opened, had a few characters added to it, and was re-saved, the modified timestamp would update.
- MFT Last Written: The most recent time that the file’s record within the MFT was updated. This timestamp is not included within the standard Windows interface, and necessitates direct inspection of the MFT record, usually via a forensics tool.


**Portable Executable Format**
The Portable Executable (PE) format is a common file format across many types of executable files on Windows systems. The PE format contains a compiled executable program, along with instructions for how the OS should load that program into memory. The most common file types that make use of the PE format are:

- Stand-alone executables (.exe)
- Dynamic-link libraries (.dll)
- Device driver files (.sys)
- Control panel items (.cpl)

**PE Structure**
All PE files begin with the following three sections:
- DOS Header
  DOS was the precursor to the Windows OS. Windows continued to rely on certain portions of DOS until the release of Windows XP, so executable code had to play nice with DOS. The DOS header is still present in all PE files today.
- DOS stub
  A stub is a small program or piece of code that is executed by default when an application's execution begins. For Windows executables that cannot be run on DOS systems, an error message This program cannot be run in DOS mode. is printed.
- PE file header
  This contains the actual start of the PE file, which begins telling the OS how to load the rest of the executable code into memory.


**scripting primer**

This course uses the following scripting languages, which come with their own pros and cons:
- Windows batch files
- PowerShell
- Bash
- Python

windows batch files:

     Windows batch file format is usable on all modern versions of Windows. However, the features offered by batch files can be a bit limited and awkward from a modern scripting perspective. Generally, batch files are used for legacy reasons or for simple tasks, although batch files are fairly common as logon scripts. Simple tasks may be easier to perform or deploy from batch files, however as the complexity of the script grows, the more complicated it can be to create these scripts as batch files. Filenames for batch files end in .bat or .cmd, though the exact execution behavior between these two extensions can differ.

 Powershell:

     PowerShell is a .NET-based, cross-platform scripting solution that has native support for remote execution and is deployed by default on modern versions of Windows, and versions of which are deployable on Unix-like platforms. In addition, its usage can be managed via Group Policy on Windows domains. It contains many features that enable richer scripts, such as full support for object-oriented programming, creation of new cmdlets — small commands or scripts written for PowerShell, which typically return a .NET object for further display or manipulation — for reusable scripting functionality, and many built-in cmdlets for commonly used functions. PowerShell script filenames end in .ps1, however, other files used in PowerShell can have other extensions.

  bash:

      Bash scripting is typically used on Unix-like systems, such as Linux, MacOS, Berkeley Software Distribution (BSD), etc. In addition, Bash can be deployed to Windows systems through Windows Subsystem for Linux (WSL), Cygwin, or other similar tools. While often seen as limited, this method of scripting is available on any system using the Bash shell, and after years of experience, it is used fairly often for system maintenance. Z Shell (ZSH) and other shells on Unix-like systems often have fairly similar scripting environments that may be mostly compatible with Bash scripts. Shell scripts — which Bash is a subset of — typically have a filename ending in .sh, though this is by convention only, and is not required. Adversaries often choose not to use this file extension.

Python:

      Python is a programming language often use in scripting. It is cross-platform, and often installed on Unix-like operating systems as well as being deployed on Windows workstations and servers where it is a dependency. Python is well-supported by programming tools that support multiple languages, and has a rich package system available with pip. Python scripts traditionally end in .py, however, this is not necessary and an adversary can choose not to use this file extension.


**script editing**

- Syntax Highlighting: Changes the color of text to indicate the category of item that is represented by that text.
- Autocompletion: Allows the editor to provide probable completions to the text currently being entered (based upon the syntax of the scripting language in use), which can speed up development.
- Debugging: Allows the pausing of execution of a script or program to view the current state of the environment or manually direct program flow.
- Script Execution via Hotkey: Executes the script being edited via selecting a button or hotkey, allowing for more rapid execution and development.

**conceptualizing a script**
- State the problem clearly. This step is vital to ensure that the correct solution is sought out.
- Determine a big picture solution to the problem.
- Break the solution down into individual steps that can be performed. If unable to do so, reconsider the solution or research as necessary.
- Determine how to perform each step in the target language, adjusting overall flow or structure as required based upon feedback here. For steps with many or complex parts, this may require following this process starting back at the beginning for that particular step.
- Test and deploy the script.
- Clean up and/or revise as necessary.


**Data Types**
- String: This data type consists of individual characters, often laid out sequentially in memory (i.e., string of characters, thus the name). String encoding varies by language or platform, with American Standard Code for Information Interchange (ASCII) and Unicode Transformation Format–8-bit (UTF-8) being fairly common — both use eight bits or a single byte to represent a single human-readable character.
- Integer: This data type corresponds to a numeric, whole number, value. The maximum value that can be stored varies based on the platform and/or language. Many languages also have a long — or similarly named — data type that has a larger possible value; some languages also have a short or byte type that stores smaller values.
- Array: Also known as lists in a scripting context, this collection type can store multiple values. Exact implementations differ by language. Raw binary data is generally handled as an array — or equivalent per language — of bytes, if available.
- Float: Floating point number — a number with a decimal point attached. This data type can store both very small and very large numbers due to its ability to move the decimal point — thus a floating point. Some languages have smaller and larger versions with less or more precision — sometimes called single and double.
- Boolean: Value that represents true or false. Often used to control program flow, such as with if/then statements. Many comparison operators return either true or false.

**Powershell Basics**
**comparison operators**
-eq: Equal — Returns True if the left and right values are equal. 
-ne: Not Equal — Returns True if the left and right values are not equal.-
gt: Greater Than — Returns True if the left value is greater than the right value.
-ge: Greater Than or Equal — Returns True if the left value is greater than or equal to the right value.
-lt: Less Than — Returns True if the left value is less than the right value.
-le: Less Than or Equal — Returns True if the left value is less than or equal to the right value.

**networking review**

![image](https://github.com/user-attachments/assets/4c762783-8134-49b0-bc1c-4d8deb051d10)

switch - moves data along via mac address

display | format of MAC addresses - 

shows an example MAC address table with the addresses changed for simplicity. MAC addresses are represented in a variety of formats for human readability by different applications, but are all identical when processed by the Operating System (OS). Some common formats are xx:xx:xx:xx:xx:xx, xxxx.xxxx.xxxx, xxxx:xxxx:xxxx, and xx.xx.xx.xx.xx.xx.


When a frame reaches a switch, if the DMAC in the frame does not match a known MAC address in the CAM table, the switch forwards a copy of the frame to all ports/paths in order to attempt to reach the addressed device. Layer 2 devices separate collision domains. Collision domains can be compared to communications with a two-way radio where only one person can talk at a time. If more than one person — or device on the physical medium — tries to talk at the same time, the communications collide and is lost for all parties. A DMAC of FFFF:FFFF:FFFF is forwarded to all ports/paths by all layer 2 devices, except the port it was received on. This is called the broadcast address at layer 2, but it should not be confused with broadcasts on layer 3.

**ip network components**
- Network Identifier (ID) (also sometimes known as subnet ID): Portion — # of bits — of an IP address that designates the network on which a host resides — also the base IP address in a network
- Host ID: Portion — # of bits — of an IP address that designates the host within its network
- Subnet mask: Mask that specifies which bits are network (binary one) and which bits are host (binary zero)
- Broadcast address: Last IP address within a network that is reserved to address all hosts in the same network
- Gateway (also known as next-hop): IP address assigned to a layer 3 device — router — that connects multiple networks together and can route packets between them
- Default gateway: Layer 3 device used for routing when there is not a more specific gateway specified in the routing table

  **Public and Private IP Addresses**
  The Internet Assigned Numbers Authority (IANA) is responsible for defining and apportioning IP addresses. IANA apportioned large blocks of IP addresses to Regional Internet Registries (RIR) who then register — or assign — IP address ranges to large organizations. Typically these are registered to very large organizations, governments, and Internet Service Providers (ISP). The owners of the IP address ranges use them as needed for their networks. Due to the shortage of public IPv4 addresses, IANA reserved several network ranges and designated them for private use, and use Network Address Translation (NAT) — or something similar — to communicate with public addresses. This allows a network to have virtually unlimited private hosts and translate them to a much smaller range of public IP addresses for use on the internet. Table 1.3-2 shows some of the reserved and private IP address blocks.

  ![image](https://github.com/user-attachments/assets/4b310b77-85fb-4091-9191-242ab86fb251)

  
![image](https://github.com/user-attachments/assets/8ac911ef-3953-4732-8625-217e39573356)




























 








 
          


      
