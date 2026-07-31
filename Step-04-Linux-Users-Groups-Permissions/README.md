# Step 04 — Linux Users, Groups & Permissions

> Professional Linux user, group, ownership, and permission management for Backend Development, DevOps, and System Administration.

---

# Overview

Linux is a **multi-user operating system**, where every file, directory, process, and resource belongs to a specific **user** and **group**.

Understanding users, groups, permissions, and privilege management is one of the most important Linux skills for:

- Backend Development
- DevOps Engineering
- Linux Administration
- Cloud Computing
- Docker
- Kubernetes
- Server Management
- Security

This chapter focuses on how Linux identifies users, manages groups, controls ownership, and secures access to files and directories.

---

# Learning Objectives

After completing this step, you should be able to:

- Understand Linux user identities
- Identify the current logged-in user
- Understand UID and GID
- Understand primary and supplementary groups
- Create and manage users
- Create and manage groups
- Add/remove users from groups
- Understand ownership
- Understand Linux permission concepts
- Manage file ownership
- Work safely with administrative privileges
- Understand Linux permission calculations
- Understand default permissions using umask

---

# Prerequisites

Before starting this step, the following should already be completed:

- ✅ Ubuntu Professional Setup
- ✅ Essential Development Tools
- ✅ Git & GitHub Setup
- ✅ SSH Configuration
- ✅ Linux Fundamentals

---

# Linux User Model

A simplified Linux security model looks like this:

```

                Linux
                   │
        ┌──────────┴──────────┐
        │                     │
     Users                 Groups
        │                     │
        └──────────┬──────────┘
                   │
            Files & Directories
                   │
             Linux Permissions

```

Every file and directory in Linux has:

- Owner (User)
- Group
- Permission

Linux checks permissions in this order:

```

Owner
   ↓
Group
   ↓
Others

```

---

# 4.1 — Linux User Identity

Linux uniquely identifies every user using:

- Username
- User ID (UID)
- Group ID (GID)
- Home Directory
- Login Shell

---

## Current Logged-in User

### Command

```bash
whoami
```

### Purpose

Displays the username of the currently logged-in user.

### Example

```text
afroza
```

---

## Display Complete User Information

### Command

```bash
id
```

### Purpose

Displays:

- UID
- GID
- Primary Group
- Supplementary Groups

### Example

```text
uid=1000(afroza)
gid=1000(afroza)
groups=1000(afroza),27(sudo),...
```

---

## Display User Groups

### Command

```bash
groups
```

### Purpose

Displays all groups that the current user belongs to.

Example:

```text
afroza adm sudo users plugdev ...
```

---

## Display Environment Variables

### Commands

```bash
echo $USER
echo $HOME
```

### Purpose

| Variable | Description |
|----------|-------------|
| `$USER` | Current username |
| `$HOME` | Home directory |

Example

```text
USER = afroza

HOME = /home/afroza
```

---

## Linux User Database

Linux stores local user account information inside:

```text
/etc/passwd
```

Useful commands:

```bash
head /etc/passwd

grep "^$USER:" /etc/passwd

getent passwd afroza
```

---

### /etc/passwd Format

```
username:x:UID:GID:comment:home:shell
```

Example

```
afroza:x:1000:1000:Afroza:/home/afroza:/bin/bash
```

| Field | Description |
|--------|-------------|
| Username | Login name |
| x | Password placeholder |
| UID | User ID |
| GID | Primary Group ID |
| Comment | User information (GECOS) |
| Home | User home directory |
| Shell | Default login shell |

---

## Important Security Note

Modern Linux systems **do not store passwords inside `/etc/passwd`.**

The `x` indicates that password-related information is stored securely elsewhere.

---

# 4.2 — User Management

Linux administrators manage users using dedicated commands.

---

## Create User

### Command

```bash
sudo adduser <username>
```

Example

```bash
sudo adduser devtest
```

Creates:

- New user
- Home directory
- Primary group
- Login shell
- Password

---

## Modify User

### Command

```bash
sudo usermod
```

Common examples:

```bash
sudo usermod -aG developers username

sudo usermod -g developers username

sudo usermod -s /bin/bash username
```

---

## Delete User

```bash
sudo deluser username
```

Delete user and home directory:

```bash
sudo deluser --remove-home username
```

---

## User Management Workflow

```

Create User
      │
      ▼
Modify User
      │
      ▼
Assign Groups
      │
      ▼
Verify
      │
      ▼
Delete (if required)

```

---

# 4.3 — Linux Groups

Groups allow multiple users to share permissions without assigning permissions individually.

Example:

```

Developers Group

├── Afroza
├── Rahim
├── Karim

```

---

## Primary Group

Every Linux user has **one** primary group.

Example

```
User

Afroza

↓

Primary Group

afroza
```

The primary group is usually assigned automatically when a user is created.

---

## Supplementary Groups

A user can belong to **multiple** supplementary groups.

Example

```
Primary Group

afroza

Supplementary Groups

sudo
adm
users
plugdev
lxd
```

This allows users to receive additional permissions without changing their primary group.

---

## Create Group

```bash
sudo groupadd devopspractice
```

---

## Display Group Information

```bash
getent group devopspractice

grep "^devopspractice:" /etc/group
```

---

## Add User to Group

Using **usermod**

```bash
sudo usermod -aG devopspractice afroza
```

Using **gpasswd**

```bash
sudo gpasswd -a afroza devopspractice
```

---

## Remove User from Group

```bash
sudo gpasswd -d afroza devopspractice
```

---

## Delete Group

```bash
sudo groupdel devopspractice
```

---

# Real DevOps Use Cases

| Task | Command |
|------|---------|
| New developer joins | `adduser` |
| Create DevOps team | `groupadd` |
| Add developer to DevOps team | `usermod -aG` |
| Remove developer from a team | `gpasswd -d` |
| Delete unused team | `groupdel` |

---

# Key Takeaways (Part 1)

- Linux is a multi-user operating system.
- Every user has a unique UID.
- Every user has one primary group.
- A user can belong to multiple supplementary groups.
- `/etc/passwd` stores user account information.
- `adduser`, `usermod`, and `deluser` manage users.
- `groupadd`, `groupdel`, and `gpasswd` manage groups.
- Groups simplify permission management in multi-user environments.

---

➡️ **Part 2 covers:**

- Ownership (`chown`, `chgrp`)
- Recursive Ownership
- Linux Permissions
- `chmod`
- Symbolic Permissions
- Numeric Permissions (644, 755, 700)
- Advanced `chmod`

---

# 4.4 — Ownership

Every file and directory in Linux belongs to:

- One **Owner (User)**
- One **Group**

Ownership determines who can access and manage files.

Example:

```text
-rw-r--r-- 1 afroza developers notes.txt
```

| Field | Value |
|--------|-------|
| Owner | `afroza` |
| Group | `developers` |

---

## View Ownership

```bash
ls -l
```

Example:

```text
-rw-rw-r-- 1 afroza afroza demo.txt
```

---

## Change Owner

### Syntax (Do NOT execute)

```text
sudo chown <user> <file>
```

### Actual Example

```bash
sudo chown root ownership.txt
```

Changes only the file owner.

---

## Change Group

### Syntax (Do NOT execute)

```text
sudo chgrp <group> <file>
```

### Actual Example

```bash
sudo chgrp users ownership.txt
```

Changes only the group ownership.

---

## Change Owner and Group Together

### Syntax (Do NOT execute)

```text
sudo chown <user>:<group> <file>
```

### Actual Example

```bash
sudo chown afroza:afroza ownership.txt
```

Changes both owner and group in one command.

---

## Recursive Ownership

### Syntax (Do NOT execute)

```text
sudo chown -R <user>:<group> <directory>
```

### Actual Example

```bash
sudo chown -R root:root ownership-lab
```

`-R` applies the ownership change recursively to all files and subdirectories.

---

## ⚠️ Recursive Warning

Be extremely careful when using recursive ownership.

Never run recursive commands on unknown paths.

Incorrect example:

```text
sudo chown -R root:root /
```

This may break an entire Linux installation.

---

# Ownership Commands Summary

| Command | Purpose |
|----------|---------|
| `chown user file` | Change owner |
| `chgrp group file` | Change group |
| `chown user:group file` | Change owner and group |
| `chown -R user:group dir` | Recursive ownership |

---

# 4.5 — Linux Permissions

Linux permissions determine **who can access files and directories**.

Linux evaluates permissions in this order:

```text
Owner
   ↓
Group
   ↓
Others
```

---

## Permission Structure

Example:

```text
-rw-rw-r--
```

Breakdown:

```text
- rw- rw- r--
│ │   │   │
│ │   │   └── Others
│ │   └────── Group
│ └────────── Owner
└──────────── File Type
```

---

# File Types

| Symbol | Meaning |
|---------|----------|
| `-` | Regular File |
| `d` | Directory |
| `l` | Symbolic Link |
| `c` | Character Device |
| `b` | Block Device |

---

# Permission Meanings

## Read (r)

### File

- Read file contents

### Directory

- List directory contents

---

## Write (w)

### File

- Modify file contents

### Directory

- Create
- Delete
- Rename files

---

## Execute (x)

### File

- Execute program or script

### Directory

- Enter (`cd`) the directory
- Traverse directory

---

# Permission Comparison

| Permission | File | Directory |
|------------|------|-----------|
| Read (`r`) | Read contents | List files |
| Write (`w`) | Modify file | Create/Delete/Rename |
| Execute (`x`) | Execute program | Enter directory |

---

# 4.6 — chmod

`chmod` changes Linux permissions.

---

## Symbolic Mode

Examples:

```bash
chmod u+w file

chmod g-w file

chmod o-r file

chmod +x file

chmod a-x file
```

---

## Symbol Meanings

| Symbol | Meaning |
|---------|---------|
| `u` | User (Owner) |
| `g` | Group |
| `o` | Others |
| `a` | All |
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exactly |
| `r` | Read |
| `w` | Write |
| `x` | Execute |

---

## Using '=' Operator

Example:

```bash
chmod u=rwx,g=rx,o= demo.txt
```

Meaning:

| Category | Permission |
|----------|------------|
| User | rwx |
| Group | r-x |
| Others | --- |

---

# Numeric Permissions

Linux assigns numeric values:

| Permission | Value |
|------------|------:|
| Read | 4 |
| Write | 2 |
| Execute | 1 |

---

## Numeric Permission Table

| Number | Permission |
|---------|------------|
| 7 | rwx |
| 6 | rw- |
| 5 | r-x |
| 4 | r-- |
| 3 | -wx |
| 2 | -w- |
| 1 | --x |
| 0 | --- |

---

## Common Permissions

| Numeric | Symbolic | Typical Use |
|----------|----------|-------------|
| 755 | `rwxr-xr-x` | Executable scripts & directories |
| 644 | `rw-r--r--` | Regular files |
| 700 | `rwx------` | Private files/directories |

---

## Permission Calculation

### 755

```text
7 = 4+2+1 = rwx

5 = 4+1 = r-x

5 = 4+1 = r-x
```

↓

```text
rwxr-xr-x
```

---

### 644

```text
6 = 4+2 = rw-

4 = r--

4 = r--
```

↓

```text
rw-r--r--
```

---

### 700

```text
7 = rwx

0 = ---

0 = ---
```

↓

```text
rwx------
```

---

# Common chmod Examples

```bash
chmod 755 deploy.sh

chmod 644 config.yml

chmod 700 ~/.ssh

chmod +x deploy.sh

chmod a-x demo.txt
```

---

# Common Mistakes

## Forgetting `-a`

Incorrect:

```bash
usermod -G developers username
```

May overwrite supplementary groups.

Correct:

```bash
usermod -aG developers username
```

---

## Executing Syntax Examples

Incorrect:

```text
sudo chown <user>:<group> <directory>
```

This is documentation syntax only.

Always replace placeholders with actual values.

---

# Syntax vs Actual Commands

| Syntax | Actual Example |
|---------|----------------|
| `chown <user> file` | `sudo chown root demo.txt` |
| `chgrp <group> file` | `sudo chgrp users demo.txt` |
| `chmod u+x file` | `chmod u+x deploy.sh` |

---

# Flags Reference (Part 1)

| Command | Flag | Meaning |
|----------|------|----------|
| `chown` | `-R` | Recursive |
| `chmod` | `u` | User |
| `chmod` | `g` | Group |
| `chmod` | `o` | Others |
| `chmod` | `a` | All |
| `chmod` | `+` | Add |
| `chmod` | `-` | Remove |
| `chmod` | `=` | Set exactly |

---

# Key Takeaways (Part 2)

- Every file has an owner and a group.
- `chown` changes ownership.
- `chgrp` changes group ownership.
- `chmod` manages permissions.
- Linux permissions consist of Owner, Group and Others.
- Symbolic and Numeric permission modes are equally important.
- Recursive ownership commands should always be used carefully.

---

➡️ **Part 3 covers:**

- `sudo`
- `root`
- `umask`
- Security Best Practices
- Linux Flags Dictionary (Complete)
- DevOps Use Cases
- Interview Questions & Answers
- Quick Revision
- Command Cheat Sheet
- Completion Status
- Git Commands



---

# 4.8 — sudo, root & Privilege Management

Linux follows the **Principle of Least Privilege**, meaning users should operate with only the permissions required to perform their tasks.

Instead of logging in as the root user, Linux administrators normally use `sudo` to execute administrative commands temporarily.

---

## Root User

The root account is the **superuser** of Linux.

Characteristics:

- UID = 0
- Full system access
- Can modify any file
- Can manage users and groups
- Can install/remove software
- Can change system configuration

Example:

```bash
id root
```

Output:

```text
uid=0(root) gid=0(root)
```

---

## sudo

**sudo = SuperUser DO**

Runs a command with temporary administrative privileges.

Example:

```bash
sudo apt update
```

Only this command executes as root.

After execution, control returns to the normal user.

---

## Verify sudo Access

```bash
sudo -l
```

Example Output

```text
(ALL : ALL) ALL
```

Meaning:

The current user is allowed to execute administrative commands using sudo.

---

## Verify Current Identity

Normal user

```bash
whoami
```

Output

```text
afroza
```

Temporary root

```bash
sudo whoami
```

Output

```text
root
```

---

## sudo vs root

| sudo | root |
|------|------|
| Temporary privilege | Superuser account |
| More secure | Full unrestricted access |
| Recommended | Rarely used for daily work |

---

# 4.9 — umask

`umask` controls the **default permissions** assigned to newly created files and directories.

It **does not modify existing permissions**.

---

## Check Current umask

```bash
umask

umask -S
```

Example:

```text
0002
```

or

```text
0022
```

---

## Default Linux Permissions

| Object | Default Permission |
|----------|-------------------|
| File | 666 |
| Directory | 777 |

After applying umask:

Example

```
File

666
-002
----
664
```

```
Directory

777
-002
----
775
```

---

## Common umask Values

| umask | File | Directory | Typical Usage |
|--------|------|-----------|---------------|
| 022 | 644 | 755 | Servers |
| 002 | 664 | 775 | Development Workstations |
| 077 | 600 | 700 | Highly Secure Systems |

---

## chmod vs umask

| chmod | umask |
|---------|---------|
| Changes existing permissions | Controls default permissions |
| Used after creation | Used during creation |

---

# Security Best Practices

- Never work as root unless necessary.
- Always use sudo for administrative tasks.
- Apply the Principle of Least Privilege.
- Use the minimum permissions required.
- Avoid recursive commands on unknown directories.
- Protect SSH private keys.
- Verify commands before pressing Enter.
- Keep backups before performing administrative operations.

---

# Dangerous Commands

The following commands can damage a Linux system if used incorrectly.

These are shown for awareness only.

| Command | Risk |
|----------|------|
| `rm -rf` | Permanent deletion |
| `chown -R` | Recursive ownership changes |
| `chmod -R` | Recursive permission changes |
| `dd` | Disk overwrite |
| `mkfs` | Formats a filesystem |

---

# Linux Flags & Short Forms Reference

## ls

| Flag | Meaning |
|------|----------|
| `-l` | Long listing |
| `-a` | All files (including hidden) |
| `-h` | Human-readable sizes |
| `-R` | Recursive |
| `-d` | Show directory itself |

---

## usermod

| Flag | Meaning |
|------|----------|
| `-a` | Append |
| `-G` | Supplementary Groups |
| `-g` | Primary Group |
| `-l` | Login name |
| `-s` | Login shell |

---

## chmod

| Symbol | Meaning |
|---------|----------|
| `u` | User |
| `g` | Group |
| `o` | Others |
| `a` | All |
| `+` | Add |
| `-` | Remove |
| `=` | Assign exactly |
| `r` | Read |
| `w` | Write |
| `x` | Execute |

---

## chown

| Flag | Meaning |
|------|----------|
| `-R` | Recursive |

---

## gpasswd

| Flag | Meaning |
|------|----------|
| `-a` | Add user to group |
| `-d` | Remove user from group |

---

## umask

| Flag | Meaning |
|------|----------|
| `-S` | Symbolic output |

---

# Syntax vs Actual Commands

## Syntax (Documentation Only)

```text
sudo chown <user>:<group> <directory>
```

## Actual Example

```bash
sudo chown -R afroza:afroza ownership-lab
```

---

# Common Mistakes

- Forgetting `-a` when using `usermod -aG`
- Confusing primary and supplementary groups
- Executing placeholder syntax examples
- Using recursive commands on the wrong directory
- Granting unnecessary execute permissions
- Running administrative commands without understanding them

---

# Real DevOps Use Cases

| Task | Linux Command |
|------|---------------|
| Create developer account | `adduser` |
| Create DevOps team | `groupadd` |
| Add developer to team | `usermod -aG` |
| Remove user from group | `gpasswd -d` |
| Change deployment ownership | `chown` |
| Configure deployment permissions | `chmod` |
| Secure SSH directory | `chmod 700 ~/.ssh` |
| Run administrative commands | `sudo` |

---

# Interview Questions

## Q1

What is the difference between a Primary Group and a Supplementary Group?

---

## Q2

What is the difference between `chmod` and `chown`?

---

## Q3

What is the difference between `sudo` and `root`?

---

## Q4

Explain Linux permission `755`.

---

## Q5

Explain Linux permission `644`.

---

## Q6

Explain Linux permission `700`.

---

## Q7

What is umask?

---

## Q8

What is the purpose of `/etc/passwd`?

---

## Q9

What does `chmod +x` do?

---

## Q10

Why should recursive commands be used carefully?

---

# Quick Revision

## User Management

```bash
adduser
usermod
deluser
```

---

## Groups

```bash
groupadd
groupdel
gpasswd
```

---

## Ownership

```bash
chown
chgrp
```

---

## Permissions

```bash
chmod
```

---

## Privileges

```bash
sudo
```

---

## Default Permissions

```bash
umask
```

---

# Command Cheat Sheet

```bash
whoami

id

groups

getent passwd

getent group

adduser

usermod -aG

groupadd

groupdel

gpasswd -a

gpasswd -d

chown

chgrp

chmod

chmod 755

chmod 644

chmod 700

sudo

sudo -l

id root

umask

umask -S
```

---

# Key Takeaways

- Linux is a multi-user operating system.
- Every user has one primary group.
- A user may belong to multiple supplementary groups.
- Ownership controls who owns files.
- Permissions control access.
- chmod modifies permissions.
- chown modifies ownership.
- sudo provides temporary administrative privileges.
- umask controls default permissions.
- Always follow the Principle of Least Privilege.

---

# Completion Status

| Topic | Status |
|---------|--------|
| User Identity | ✅ |
| User Management | ✅ |
| Groups | ✅ |
| Ownership | ✅ |
| Permissions | ✅ |
| chmod | ✅ |
| sudo | ✅ |
| root | ✅ |
| umask | ✅ |
| Practical Labs | ✅ |

---

# Next Step

➡️ **Step 05 — Linux Processes, Jobs & Process Management**

Topics include:

- Process lifecycle
- ps
- top
- htop
- pgrep
- pidof
- kill
- killall
- nice
- renice
- jobs
- bg
- fg
- nohup
- Signals
- Process troubleshooting
