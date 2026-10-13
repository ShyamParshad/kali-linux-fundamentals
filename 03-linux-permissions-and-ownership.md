# Linux Permissions & Ownership — SOC Analyst Fundamentals

## Overview

Linux permissions control who can read, modify, or execute files and who can access directories.

Understanding permissions and ownership is important for SOC Analysts because incorrect permissions, unexpected ownership changes, and suspicious special permissions can create security risks.

## 1. Understanding Linux Permissions

Linux uses three basic permission types:

| Permission | Symbol | Numeric Value | Meaning |
|---|---|---:|---|
| Read | `r` | 4 | Read a file |
| Write | `w` | 2 | Modify a file |
| Execute | `x` | 1 | Execute a program or script |

Permissions are assigned to three categories:

- **Owner (`u`):** The user who owns the file.
- **Group (`g`):** The file's assigned group.
- **Others (`o`):** Users who do not match the owner or assigned group.

## 2. Reading Permissions with `ls -l`

Command:

```bash
ls -l demo.txt
```

Example output:

```text
-rw-rw-r-- 1 kali kali 0 Oct 10 05:52 demo.txt
```

The permission string is:

```text
-rw-rw-r--
```

Breakdown:

- `-` — Regular file.
- `rw-` — Owner can read and write.
- `rw-` — Group can read and write.
- `r--` — Others can read only.

The owner and group are both `kali`.

## 3. Numeric Permissions

Numeric permissions are calculated by adding the values of `r`, `w`, and `x`.

Examples:

| Numeric | Symbolic | Meaning |
|---:|---|---|
| `7` | `rwx` | Read, write, execute |
| `6` | `rw-` | Read and write |
| `5` | `r-x` | Read and execute |
| `4` | `r--` | Read only |
| `0` | `---` | No permissions |

The three-digit permission format represents owner, group, and others.

Examples:

```text
600 = rw-------
640 = rw-r-----
644 = rw-r--r--
700 = rwx------
755 = rwxr-xr-x
```

## 4. Changing Permissions with `chmod`

The `chmod` command changes file or directory permissions.

### Numeric method

```bash
chmod 600 demo.txt
stat -c '%A %a %U:%G %n' demo.txt
```

Observed output:

```text
-rw------- 600 kali:kali demo.txt
```

Meaning:

- Owner: read and write.
- Group: no permissions.
- Others: no permissions.

### Symbolic method

Symbolic permissions use these operators:

| Symbol | Meaning |
|---|---|
| `u` | Owner/user |
| `g` | Group |
| `o` | Others |
| `a` | All categories |
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set permissions exactly |

Examples:

```bash
chmod g+r demo.txt
chmod g-r demo.txt
chmod u=r demo.txt
chmod o+r demo.txt
```

- `chmod g+r demo.txt` adds group read permission.
- `chmod g-r demo.txt` removes group read permission.
- `chmod u=r demo.txt` sets the owner's permissions to read only.
- `chmod o+r demo.txt` adds read permission for others.

### Practical results

After:

```bash
chmod 600 demo.txt
chmod g+r demo.txt
```

The observed permission was:

```text
-rw-r----- 640 kali:kali demo.txt
```

After:

```bash
chmod g-r demo.txt
```

The permission returned to:

```text
-rw------- 600 kali:kali demo.txt
```

After:

```bash
chmod u=r demo.txt
```

The observed result was:

```text
-r-------- 400 kali:kali demo.txt
```

These practicals demonstrate how symbolic and numeric permissions can be changed and verified.

## 5. Checking Permissions with `stat`

Command:

```bash
stat -c '%A %a %U:%G %n' demo.txt
```

This displays:

- `%A` — Symbolic permissions.
- `%a` — Numeric permissions.
- `%U:%G` — Owner username and group name.
- `%n` — File name.

This command is useful when investigating permissions and ownership during Linux security analysis.

## 6. File Ownership and `chown`

The `chown` command changes file ownership.

Example:

```bash
sudo chown kali:kali demo.txt
ls -l demo.txt
```

This sets the owner to `kali` and the group to `kali`, subject to system permissions.

Observed output:

```text
-r-------- 1 kali kali 0 Oct 10 05:52 demo.txt
```

The ownership changed, but the permissions remained `400`.

**Key difference:**

- `chmod` changes permissions.
- `chown` changes ownership and can also change the assigned group.

## 7. Special Permissions

### SUID — Set User ID

SUID on an executable can cause the program to run with the file owner's effective user ID, subject to execution conditions.

Example command:

```bash
ls -l /usr/bin/passwd
```

Observed output:

```text
-rwsr-xr-x 1 root root 118168 Apr 19 2025 /usr/bin/passwd
```

The `s` in the owner's execute position indicates SUID.

SUID is used by some programs that need controlled elevated privileges to perform specific operations.

### SGID — Set Group ID

SGID on an executable can cause the program to run with the file's effective group ID.

On a directory, SGID commonly causes newly created files and subdirectories to inherit the directory's group.

Example permission pattern:

```text
-rwxr-sr-x
```

The `s` in the group's execute position indicates SGID.

### Sticky Bit

The Sticky Bit is commonly used on shared directories. It generally prevents ordinary users from deleting or renaming files owned by other users in that directory, subject to ownership and privileged-user exceptions.

Example:

```bash
ls -ld /tmp
```

Observed output:

```text
drwxrwxrwt 15 root root 360 Oct 12 21:06 /tmp
```

The final `t` indicates the Sticky Bit.

The numeric permission for `drwxrwxrwt` is `1777`:

- `1` — Sticky Bit.
- `7` — Owner permissions.
- `7` — Group permissions.
- `7` — Others permissions.

## 8. SOC Analyst Use Cases

A SOC Analyst may investigate:

1. Sensitive files with unexpectedly broad read or write permissions.
2. Unexpected ownership changes.
3. Suspicious executables with SUID or SGID enabled.
4. Incorrect permissions on shared directories.
5. Changes to important system files that do not match the expected configuration.

For example, unexpected read access to `/etc/shadow` could expose password hashes. An unusual SUID executable may warrant investigation.

**Important:** A permission or ownership setting alone does not prove malicious activity. Validate findings against the system's expected configuration and supporting evidence.

## 9. Practical Lab Summary

Commands practiced:

```bash
ls -l demo.txt
stat -c '%A %a %U:%G %n' demo.txt

chmod 600 demo.txt
chmod g+r demo.txt
chmod g-r demo.txt
chmod u=r demo.txt

ls -l /usr/bin/passwd
ls -ld /tmp

sudo chown kali:kali demo.txt
```

These exercises were performed on the test file `demo.txt` inside `~/permission-lab`, with system-file commands used for read-only inspection.

## 10. Key Takeaways

- `r`, `w`, and `x` represent read, write, and execute.
- Permissions apply to owner, group, and others.
- Numeric permissions are calculated using `4`, `2`, and `1`.
- `chmod` modifies permissions.
- `chown` changes ownership.
- `stat` helps verify permissions and ownership.
- SUID, SGID, and Sticky Bit add special permission behaviour.
- SOC Analysts should investigate unexpected permission or ownership changes using evidence.

