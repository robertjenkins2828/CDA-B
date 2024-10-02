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
    

          


      
