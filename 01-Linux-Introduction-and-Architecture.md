# 🐧 Linux Basics & Filesystem 

## 🎯 Objective

This section documents my learning and practical work on Linux fundamentals as part of my **SOC Analyst → Cybersecurity Roadmap**.

The goal was to understand the Linux filesystem, navigate the system, understand processes, and perform basic process investigation using `/proc`.

---

# 1. Linux Architecture

Linux can be understood as a system where the **kernel acts as the core** between hardware and software.

The kernel manages important system resources such as:

- CPU
- Memory
- Processes
- Devices
- Filesystem
- Networking

For SOC analysis, understanding how Linux manages processes, files, users, and logs is important for investigation.

---

# 2. Linux Filesystem

Linux uses a hierarchical filesystem structure.

The top-level directory is:

```bash
/
```

This is called the **root directory**.

Everything in the Linux filesystem exists under `/`.

---

## `/`

The root directory is the starting point of the Linux filesystem hierarchy.

Example:

```text
/
├── home
├── etc
├── var
├── tmp
├── usr
├── bin
├── sbin
├── dev
├── proc
└── opt
```

---

## `/home`

Contains home directories of normal users.

Example:

```text
/home/user
/home/shyam
```

Users normally store their personal files and directories here.

---

## `/root`

The home directory of the **root user**.

Important distinction:

```text
/home       → normal users' home directories
/root       → root user's home directory
```

---

## `/etc`

Contains system and application **configuration files**.

Examples of configuration information can include:

- User configuration
- Service configuration
- Network configuration
- System settings

---

## `/var`

Contains data that changes frequently during system operation.

Examples include:

- Logs
- Caches
- Spools
- Application data
- Runtime-related data

### `/var/log`

A particularly important directory for SOC analysts.

It contains system and application logs.

Examples can include:

- Authentication-related logs
- System logs
- Application logs

---

## `/tmp`

Used for temporary files and temporary data.

Temporary files may be created by applications and processes during operation.

From a security perspective, `/tmp` can sometimes be relevant during investigation because attackers may also use temporary directories.

---

## `/usr`

Contains many user-space programs, libraries, and other software resources.

Important:

> `/usr` is NOT the directory containing user account information.

---

## `/bin`

Contains essential executable programs/commands.

---

## `/sbin`

Contains important system administration executables.

---

## `/dev`

Contains device files/interfaces representing devices that the Linux system can interact with.

---

## `/proc`

`/proc` is a **virtual filesystem** that exposes live information about running processes and the Linux kernel/system.

For example:

```text
/proc/1777/
```

represents information associated with process ID `1777`.

This is especially useful for process investigation.

---

## `/opt`

Used for optional/additional software packages and applications.

---

# 3. Filesystem Navigation

## `pwd`

`pwd` means:

```bash
Print Working Directory
```

It shows the directory you are currently in.

Example:

```bash
pwd
```

---

## `ls`

Lists files and directories in the current location.

```bash
ls
```

---

## `cd`

Used to change directories.

Example:

```bash
cd /home
```

---

## `cd ..`

Moves to the parent directory.

```bash
cd ..
```

---

## `cd /`

Moves directly to the root directory.

```bash
cd /
```

---

## `cd ~`

Moves to the current user's home directory.

```bash
cd ~
```

---

# 4. Absolute vs Relative Paths

## Absolute Path

An absolute path starts from `/` and specifies the complete location.

Example:

```text
/home/shyam/Documents/file.txt
```

---

## Relative Path

A relative path is interpreted from the current working directory.

Example:

```text
Documents/file.txt
```

---

# 5. Programs vs Processes

One important concept I learned was the difference between a **program** and a **process**.

### Program

A program is software/instructions stored on the system.

Example:

```text
Chrome
Python
Firefox
```

when they are installed but not currently executing.

### Process

When a program is executed, the operating system creates a **running instance** of that program called a process.

Example:

```text
Program → Chrome
          ↓
       Running
          ↓
       Process
```

---

# 6. PID — Process ID

Every running process is assigned a **Process ID (PID)**.

Example:

```text
PID = 5000
```

PID allows us to identify a specific running process.

---

# 7. PPID — Parent Process ID

PPID means **Parent Process ID**.

It tells us the PID of the process that created/started the current process.

Example:

```text
Parent Process
PID = 2500
      ↓
Child Process
PID = 7000
PPID = 2500
```

Therefore:

```text
Parent → 2500
Child  → 7000
```

Understanding parent-child relationships is important during SOC investigations.

---

# 8. Threads

A thread is an execution unit inside a process.

A single process can contain multiple threads.

Example:

```text
Process
├── Thread 1
├── Thread 2
├── Thread 3
└── Thread 4
```

We also observed Linux kernel threads such as:

```text
[kworker/...]
[kthreadd]
```

These are not automatically malicious.

> A process or thread should not be considered malicious simply because of its name. Context and behavior must be investigated.

---

# 9. Process States

Linux processes can have different states.

| State | Meaning |
|---|---|
| `R` | Running / Runnable |
| `S` | Sleeping |
| `T` | Stopped |
| `Z` | Zombie |
| `I` | Idle kernel thread |

### Zombie Process

A zombie process has finished execution but still has an entry in the process table because its parent has not yet collected its exit status.

---

# 10. `ps` Command

`ps` stands for **process status**.

It displays information about processes.

Example:

```bash
ps
```

Example output:

```text
PID   TTY      TIME     CMD
2282  pts/0    00:00:00 zsh
2342  pts/0    00:00:00 ps
```

Important columns:

- PID → Process ID
- TTY → Associated terminal/session
- TIME → CPU time consumed
- CMD → Command/program

The `ps` process appears in the output because running `ps` itself creates a process.

---

# 11. `ps aux`

A more detailed process listing can be obtained with:

```bash
ps aux
```

Important columns:

| Column | Meaning |
|---|---|
| USER | User context of process |
| PID | Process ID |
| %CPU | CPU utilization |
| %MEM | Memory utilization |
| VSZ | Virtual memory size |
| RSS | Resident memory |
| TTY | Associated terminal |
| STAT | Process state/flags |
| START | Start time |
| TIME | CPU time |
| COMMAND | Command/program |

---

# 12. VSZ

**VSZ — Virtual Memory Size**

VSZ represents the virtual memory space associated with a process.

Important:

> VSZ does not mean the exact amount of physical RAM currently being used.

---

# 13. RSS

**RSS — Resident Set Size**

RSS represents the amount of process memory currently resident in physical RAM.

Example:

```text
RSS = 12000 KB
```

means approximately 12 MB of that process's memory is resident in RAM.

### SOC Perspective

High RSS can be a useful investigation clue, but:

> **High memory usage alone does NOT prove that a process is malicious.**

An analyst needs additional context.

---

# 14. TTY

TTY represents the terminal/session associated with a process.

Example:

```text
TTY = pts/0
```

means the process is associated with that terminal.

If:

```text
TTY = ?
```

it generally means the process does not have an associated controlling/interactive terminal.

---

# 15. `/proc/<PID>/`

Linux provides process-specific information under:

```text
/proc/<PID>/
```

For example:

```text
/proc/5000/
```

contains live information related to process `5000`.

Some important entries studied:

```text
/proc/<PID>/status
/proc/<PID>/cmdline
/proc/<PID>/exe
/proc/<PID>/cwd
```

---

# 16. `/proc/<PID>/status`

Example:

```bash
cat /proc/5000/status
```

This provides information about a process.

Important fields include:

```text
Name:
State:
Pid:
PPid:
VmRSS:
Threads:
```

### Meaning

- `Pid` → Current process ID
- `PPid` → Parent process ID
- `State` → Current process state
- `VmRSS` → Resident memory in RAM
- `Threads` → Number of threads in the process

Example:

```text
Pid:       4500
PPid:      1200
State:     S
VmRSS:     25000 kB
Threads:   5
```

Interpretation:

- Process ID = 4500
- Parent PID = 1200
- Process is sleeping
- About 25,000 KB resident memory
- Process has 5 threads

---

# 17. `/proc/<PID>/cmdline`

Used to inspect the command and arguments associated with starting a process.

Example:

```bash
cat /proc/7000/cmdline
```

This can help an analyst understand how a process was launched.

---

# 18. `/proc/<PID>/exe`

Represents the executable associated with the process.

Example:

```bash
readlink -f /proc/7000/exe
```

Possible output:

```text
/usr/bin/python3
```

This tells us which executable the process is running.

---

# 19. `/proc/<PID>/cwd`

`cwd` means:

**Current Working Directory**

Example:

```bash
readlink -f /proc/8000/cwd
```

Possible output:

```text
/home/user/project
```

This tells us the process's current working directory.

---

# 20. Practical Linux Process Investigation

I performed a practical investigation using a real Linux process.

### Process investigated

```text
PID = 1777
Process = zsh
```

I checked:

```bash
cat /proc/1777/status
```

Important information observed:

```text
Name   → zsh
State  → Sleeping
Pid    → 1777
PPid   → 1740
VmRSS  → 5880 KB
```

I also checked the command/executable:

```bash
cat /proc/1777/cmdline
```

Result:

```text
/usr/bin/zsh
```

Then:

```bash
readlink -f /proc/1777/exe
```

Result:

```text
/usr/bin/zsh
```

And the current working directory:

```bash
readlink -f /proc/1777/cwd
```

Result:

```text
/usr/bin
```

### Investigation Summary

```text
PID        → 1777
Process    → zsh
State      → Sleeping
PPID       → 1740
VmRSS      → 5880 KB
Executable → /usr/bin/zsh
CWD        → /usr/bin
```

---

# 21. SOC Investigation Mindset

One important lesson from this practical work:

> **A suspicious indicator is not automatically malicious.**

For example, high RSS does not automatically mean malware.

An analyst should investigate context such as:

```text
Process
   ↓
PID / PPID
   ↓
Parent process
   ↓
Command / Arguments
   ↓
Executable
   ↓
Working Directory
   ↓
Memory / State
   ↓
Network activity
   ↓
Overall behavior
```

The goal is to understand **what the process is doing and whether its behavior is actually suspicious**.

---

# 🎯 Phase 2.1 Completion

## Linux Basics & Filesystem

**Status: ✅ COMPLETE**

Topics completed:

- Linux Architecture
- Linux Filesystem
- Filesystem Navigation
- Processes
- PID / PPID
- Parent & Child Processes
- Threads
- Process States
- `ps`
- `ps aux`
- VSZ
- RSS
- TTY
- `/proc`
- `/proc/<PID>/status`
- `/proc/<PID>/cmdline`
- `/proc/<PID>/exe`
- `/proc/<PID>/cwd`
- Basic Linux Process Investigation

---

## 🚀 Next Phase

### Phase 2 — Linux Users & Groups

Topics:

- Users
- Groups
- UID
- GID
- `who`
- `whoami`
- `id`
- `/etc/passwd`
- `/etc/shadow`
- Linux user investigation
- SOC use cases

