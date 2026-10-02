# 🐧 Linux Introduction & Architecture

This is my first documented section of the Kali Linux Fundamentals journey.

The focus of this section is understanding the basic Linux architecture, the role of the kernel and shell, Linux distributions, command structure, and basic system identification.

---

## 1. What is Linux?

Linux is an open-source kernel that manages system resources and provides communication between software and hardware.

The kernel is the core component responsible for managing resources such as:

- CPU
- Memory
- Processes
- Filesystem
- Networking
- Hardware

Basic flow:

```text
User
  ↓
Application / Command
  ↓
Shell
  ↓
Linux Kernel
  ↓
Hardware


```

2. What is the Linux Kernel?

The Linux kernel is the core of the Linux operating system.
It manages system resources and provides an interface between applications and hardware.
Major responsibilities
Process Management
The kernel manages running processes and CPU resources.
Memory Management
The kernel manages RAM and allocates memory to processes.
Filesystem Management
The kernel manages access to files, directories, and storage.
Network Management
The kernel provides networking functionality such as TCP/IP communication and network interface management.
Hardware Management
The kernel communicates with hardware through device drivers and system interfaces.

3. Linux Distribution
Technically, Linux refers to the kernel.
A Linux distribution combines the Linux kernel with:
- System utilities
- Libraries
- Applications
- Package management
- Configuration
- User environment
Examples:
- Ubuntu
- Debian
- Fedora
- Arch Linux
- Kali Linux
- RHEL
Kali Linux
Kali Linux is a Debian-based Linux distribution focused on cybersecurity and security testing workflows.
4. Terminal vs Shell
These two concepts are different.
Terminal
A terminal is the interface/application where commands are entered.
Shell
A shell is a command interpreter that processes commands.
Common shells include:
- Bash
- Zsh
- Fish
Current Shell
The shell on my Kali system was verified using:
echo $SHELL

Output:
/usr/bin/zsh

This shows that Zsh is the current shell.
5. Linux Command Structure
A Linux command commonly follows this structure:
command   option/flag   argument

Example:
ls -l /etc

Where:
ls    → Command
-l    → Option / Flag
/etc  → Argument

Command
Defines the action to perform.
Example:
ls

Option / Flag
Changes or modifies the behavior of a command.
Example:
ls -l

Argument
Specifies the target or input for the command.
Example:
ls /etc

6. Absolute Path
An absolute path starts from the root directory /.
Examples:
/etc/passwd
/var/log
/home/kali

Example:
cat /etc/passwd

The path starts from:
/

and specifies the complete location of the file.
7. Relative Path
A relative path is interpreted from the current working directory.
For example, if the current directory is:
/home/kali

and a file exists at:
/home/kali/notes.txt

it can be accessed using:
cat notes.txt

The complete absolute path would be:
/home/kali/notes.txt

8. Important Linux Path Symbols
Symbol	Meaning
/	Root directory
~	Current user's home directory
.	Current directory
..	Parent directory


/
The root of the Linux filesystem.
~
Represents the current user's home directory.
For the Kali user:
/home/kali

.
Represents the current directory.
..
Represents the parent directory.
🔎 9. System Identification Practical
During this section, I used several commands to identify the Linux environment.
9.1 Kernel Information
Command:
uname -a

Output from my Kali system:
Linux kali 6.12.25-amd64 #1 SMP PREEMPT_DYNAMIC Kali 6.12.25-1kali1 (2025-04-30) x86_64 GNU/Linux

This output provides information about:
- Kernel name
- Hostname
- Kernel version
- Kernel build information
- System architecture
Important information
Kernel:       Linux
Hostname:     kali
Kernel:       6.12.25-amd64
Architecture: x86_64

9.2 System Architecture
Command:
uname -m

Output:
x86_64

This confirms that the system is using the x86_64 architecture.
9.3 Linux Distribution Information
Command:
cat /etc/os-release

Relevant output:
PRETTY_NAME="Kali GNU/Linux Rolling"
NAME="Kali GNU/Linux"
VERSION_ID="2025.2"
VERSION="2025.2"
VERSION_CODENAME=kali-rolling
ID=kali
ID_LIKE=debian

Important information
Distribution: Kali GNU/Linux
Version:      2025.2
Release:      kali-rolling
Based on:     Debian

This demonstrates an important distinction:
Linux Kernel
     ↓
6.12.25

Linux Distribution
     ↓
Kali GNU/Linux 2025.2

Kernel version and distribution version are not the same thing.
🧠 10. Linux Architecture Mental Model
The concepts learned in this section can be summarized as:

                 USER
                   │
                   ▼
          APPLICATION / COMMAND
                   │
                   ▼
                SHELL
              /usr/bin/zsh
                   │
                   ▼
             LINUX KERNEL
               6.12.25
          ┌────────┼────────┐
          ▼        ▼        ▼
       Process   Memory   Network
          │        │        │
          └────────┼────────┘
                   ▼
                HARDWARE

🛡️ 11. SOC Analyst Relevance
Linux fundamentals are important for SOC investigations.
A Linux investigation can involve:
User
 ↓
Process
 ↓
File
 ↓
Network Connection
 ↓
Logs
 ↓
Timeline
 ↓
Incident

Some important Linux locations that will be investigated later include:
/etc
/home
/var/log
/tmp
/proc
/dev

These locations will be covered in detail in later sections of this repository.
🎯 Key Takeaways
1. Linux technically refers to the Linux kernel.
2. The kernel manages system resources and hardware interaction.
3. A Linux distribution combines the kernel with user-space components.
4. Kali Linux is a Debian-based Linux distribution.
5. A terminal provides the interface for entering commands.
6. A shell interprets and processes commands.
7. My current shell is Zsh.
8. Linux commands can contain commands, options, and arguments.
9. Absolute paths start from /.
10. Relative paths depend on the current working directory.
11. Kernel version and distribution version are different.
12. Linux fundamentals are important for SOC investigation.
🚧 Next Topic
Linux Filesystem & Navigation
The next section will cover the Linux filesystem structure and practical navigation using commands such as:
pwd
ls
cd
