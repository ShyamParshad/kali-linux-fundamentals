# 🐧 Kali Linux Fundamentals

A practical learning journey through Kali Linux and Linux fundamentals, focused on building strong skills for Cybersecurity and SOC Analyst roles.

## 🎯 Goal

The goal of this repository is to build strong Linux fundamentals through practical learning and gradually apply them to cybersecurity and SOC investigations.

The focus is on:

- Understanding Linux concepts
- Hands-on practical learning
- Linux administration
- Security-focused investigation
- SOC Analyst skills

---

## 📚 Linux Learning Roadmap

### 1. Linux Basics & Architecture
- [ ] Linux Basics
- [ ] Linux Architecture
- [ ] Linux Kernel
- [ ] Linux Distribution
- [ ] Terminal & Shell
- [ ] Command Structure

### 2. Linux Filesystem
- [ ] Linux Filesystem Structure
- [ ] `/`
- [ ] `/home`
- [ ] `/etc`
- [ ] `/var`
- [ ] `/var/log`
- [ ] `/tmp`
- [ ] `/usr`
- [ ] `/bin`
- [ ] `/sbin`
- [ ] `/dev`
- [ ] `/proc`
- [ ] `/opt`
- [ ] Absolute & Relative Paths
- [ ] Filesystem Navigation

### 3. Users & Groups
- [ ] Users
- [ ] Groups
- [ ] UID & GID
- [ ] `who`
- [ ] `whoami`
- [ ] `id`
- [ ] `/etc/passwd`
- [ ] `/etc/shadow`

### 4. Permissions & Ownership
- [ ] Read, Write & Execute
- [ ] `chmod`
- [ ] `chown`
- [ ] `chgrp`
- [ ] Numeric Permissions
- [ ] SUID
- [ ] SGID
- [ ] Sticky Bit

### 5. Processes
- [ ] PID & PPID
- [ ] Parent & Child Processes
- [ ] `ps`
- [ ] `top`
- [ ] `htop`
- [ ] `kill`
- [ ] Signals
- [ ] `/proc/<PID>`

### 6. Services
- [ ] systemd
- [ ] `systemctl`
- [ ] Service Status
- [ ] Start / Stop Services
- [ ] Enable / Disable Services

### 7. Logs
- [ ] `/var/log`
- [ ] Authentication Logs
- [ ] System Logs
- [ ] `journalctl`
- [ ] Log Investigation

### 8. File & Text Investigation
- [ ] `find`
- [ ] `grep`
- [ ] `less`
- [ ] `head`
- [ ] `tail`
- [ ] Basic `awk`
- [ ] Basic `sed`

### 9. Linux SOC Investigation
- [ ] Suspicious Process Investigation
- [ ] Suspicious File Investigation
- [ ] Suspicious User Investigation
- [ ] Unusual Service Investigation
- [ ] Authentication Failure Investigation
- [ ] Basic Linux Incident Investigation

---

## 🧪 Practical Learning

Each topic will include:

**Concept → Practical → Output Analysis → Investigation → Documentation**

The goal is to understand **why** commands and concepts work rather than simply memorizing commands.

---

## 🛡️ SOC Focus

Linux skills will be applied to security investigations such as:

- Process investigation
- User investigation
- File investigation
- Permission investigation
- Service investigation
- Authentication investigation
- Log analysis
- Basic incident investigation

---

## 📂 Repository Structure

```text
kali-linux-fundamentals/
│
├── README.md
│
├── Linux-Fundamentals/
│   ├── 01-Linux-Introduction-and-Architecture.md
│   ├── 02-Linux-Filesystem.md
│   ├── 03-Users-and-Groups.md
│   ├── 04-Permissions-and-Ownership.md
│   ├── 05-Processes.md
│   ├── 06-Services.md
│   ├── 07-Logs.md
│   ├── 08-File-and-Text-Investigation.md
│   └── 09-Linux-SOC-Investigation.md
│
└── Labs/
