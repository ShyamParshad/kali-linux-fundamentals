# Linux Users & Groups — SOC Analyst Fundamentals

## Overview

In Linux, users and groups help the operating system identify users and control access to system resources.

Understanding users and groups is important for SOC Analysts because suspicious accounts, unexpected privileges, and unauthorized changes can indicate potential security risks.

## 1. Linux User Accounts

A Linux user account represents an identity on the system.

There are two common categories:

- **Root user:** The superuser with extensive administrative privileges.
- **Normal user:** A regular account with permissions defined by the system configuration.

Each user has a numerical User ID (UID).

### Important Commands

```bash
whoami
who
id
```

- `whoami` — Displays the current username.
- `who` — Displays logged-in user sessions.
- `id` — Displays the UID, GID, and group memberships of the current user.

To inspect a specific account:

```bash
id kali
```

## 2. UID and GID

**UID (User ID):** A numerical identifier assigned to a user account.

**GID (Group ID):** A numerical identifier assigned to a group.

Important points:

- UID `0` normally represents the root identity.
- Normal user accounts often have UIDs starting at `1000` on many Linux distributions.
- GID identifies a group.
- Exact UID and GID allocation depends on the distribution and system configuration.

Example:

```text
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo)
```

This example means:

- Username: `kali`
- UID: `1000`
- Primary GID: `1000`
- `sudo` appears in the user's supplementary groups.

Membership in the `sudo` group may permit administrative commands, depending on the system's sudo configuration. It does not make the account permanently root.

## 3. The `/etc/passwd` File

The `/etc/passwd` file contains account information.

View account entries:

```bash
cat /etc/passwd
```

For a specific account, use:

```bash
getent passwd kali
```

Example entry:

```text
kali:x:1000:1000::/home/kali:/usr/bin/zsh
```

The seven fields are:

| Field | Meaning |
|---|---|
| `kali` | Username |
| `x` | Password information is usually stored in `/etc/shadow` |
| `1000` | UID |
| `1000` | Primary GID |
| Empty | Optional user description |
| `/home/kali` | Home directory |
| `/usr/bin/zsh` | Configured login shell |

## 4. The `/etc/shadow` File

The `/etc/shadow` file stores password hashes and password-aging information on typical Linux installations.

Important points:

- Password hashes are not plaintext passwords.
- Access to this file is restricted because it contains sensitive authentication information.
- Never publish password hashes or sensitive authentication data in a public repository.

Check its permissions without displaying its contents:

```bash
ls -l /etc/shadow
```

## 5. Linux Groups

Groups allow multiple users to share permissions and access rights.

Check group memberships:

```bash
groups
groups kali
```

Example output:

```text
kali : kali adm sudo wireshark
```

The user belongs to the listed groups. The actual permissions granted by each group depend on system configuration and resource permissions.

A group such as `sudo` may allow administrative access. Groups such as `wireshark` may grant access related to packet capture, depending on configuration.

## 6. Login Shells

A shell is a program that interprets commands and helps users interact with the operating system.

Common shells include:

- **Bash:** Bourne Again Shell
- **Zsh:** Z Shell
- **Sh:** A traditional Unix shell interface

Examples of shell paths:

```text
/bin/bash
/usr/bin/zsh
/bin/sh
```

These paths identify executable programs. On some distributions, `/bin` and `/usr/bin` may refer to the same underlying location.

The final field in an `/etc/passwd` entry specifies the configured login shell.

### What is `/usr/sbin/nologin`?

This program normally prevents interactive login for an account.

It is commonly used with service accounts that perform system tasks without needing a normal interactive shell.

Example:

```text
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
```

This entry shows:

- Username: `backup`
- UID: `34`
- Primary GID: `34`
- Home directory: `/var/backups`
- Login shell: `/usr/sbin/nologin`

A `nologin` shell does not necessarily prevent every possible use of the account by system services.

## 7. Understanding `sudo`

The `sudo` command allows an authorized user to execute a command with elevated privileges.

Example:

```bash
whoami
sudo whoami
```

Typical output:

```text
kali
root
```

The first command runs as the current user. The second command runs with root privileges if sudo authorization and authentication requirements are satisfied.

The user's account does not permanently become root.

An authorized administrator may also open a privileged login shell with:

```bash
sudo -i
```

## 8. Practical Investigation

### Practical 1: Inspect an account

```bash
getent passwd kali
id kali
groups kali
```

These commands help identify account details, numerical IDs, and group memberships.

### Practical 2: Investigate a service account

```bash
getent passwd backup
id backup
```

Example findings on my Kali Linux system:

- UID: `34`
- Primary GID: `34`
- Home directory: `/var/backups`
- Login shell: `/usr/sbin/nologin`

These findings are consistent with a typical restricted service account, but the account's purpose and legitimacy should be verified during a real investigation.

## 9. SOC Analyst Use Cases

A SOC Analyst may investigate:

1. Unexpected accounts with UID `0`.
2. Unauthorized membership in privileged groups.
3. Unexpected changes to user IDs or group memberships.
4. Changes to login shells, especially for service accounts.
5. Suspicious account activity correlated with authentication logs and system events.

**Important:** A suspicious username, UID, group, or shell is an investigative clue—not proof of malicious activity. Always validate findings using additional evidence.

## 10. Key Takeaways

- Users represent identities on a Linux system.
- UID identifies a user numerically; GID identifies a group.
- UID `0` normally represents root-level identity.
- `/etc/passwd` contains account information.
- `/etc/shadow` stores password hashes and password-aging data on typical systems.
- `groups` and `id` help inspect group memberships.
- Bash, Zsh, and Sh are shells.
- `/usr/sbin/nologin` normally blocks interactive login.
- `sudo` can provide elevated privileges without permanently changing the user's identity.
- SOC investigations require evidence and verification, not assumptions.
