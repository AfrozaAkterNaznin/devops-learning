# Step 03 — Linux Fundamentals

## Overview

Linux fundamentals are essential for Backend Development, DevOps, Cloud Engineering, System Administration, and Cybersecurity.

Before working with tools such as Docker, Kubernetes, Nginx, Jenkins, and cloud servers, it is important to understand how to navigate and operate a Linux system confidently from the command line.

In this step, I practiced the core Linux commands and filesystem concepts that will be used throughout my DevOps learning journey.

---

# Learning Objectives

After completing this step, I became familiar with:

- Linux filesystem structure
- Home and root directories
- Absolute and relative paths
- Directory navigation
- Listing files and directories
- Creating files and directories
- Copying, moving, renaming, and deleting files
- Reading file contents
- Searching files and commands
- Text processing
- Pipes and redirection
- Command history
- Hard links and symbolic links
- File compression and archives
- Basic file permissions

---

# 1. Linux Filesystem Structure

Linux uses a hierarchical filesystem.

Everything starts from the root directory:

```text
/
```

A simplified Linux filesystem looks like:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── lib64
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

## Important Directories

| Directory | Purpose |
|---|---|
| `/` | Root of the entire filesystem |
| `/home` | Home directories of normal users |
| `/root` | Home directory of the root user |
| `/etc` | System and application configuration |
| `/usr` | User applications, libraries, and shared resources |
| `/var` | Logs, caches, databases, and variable application data |
| `/tmp` | Temporary files |
| `/boot` | Bootloader and kernel-related files |
| `/dev` | Device representations |
| `/proc` | Virtual filesystem containing process/kernel information |
| `/sys` | Kernel and hardware information |
| `/opt` | Optional third-party software |
| `/mnt` | Temporary filesystem mount point |
| `/media` | Removable media mount point |
| `/run` | Runtime system information |
| `/srv` | Data served by system services |

---

# 2. Home Directory and Root Directory

My Linux username:

```bash
whoami
```

Output:

```text
afroza
```

My home directory:

```text
/home/afroza
```

Commands used:

```bash
pwd
echo $HOME
echo ~
```

All returned:

```text
/home/afroza
```

The root filesystem is:

```text
/
```

The root user's home directory is:

```text
/root
```

These are different concepts.

| Path | Meaning |
|---|---|
| `/` | Root of filesystem |
| `/root` | Root user's home directory |
| `/home/afroza` | My user's home directory |
| `~` | Shortcut for my home directory |

---

# 3. Absolute vs Relative Paths

## Absolute Path

An absolute path starts from `/`.

Example:

```text
/home/afroza/Projects
```

It identifies the complete location of a file or directory.

## Relative Path

A relative path starts from the current working directory.

Example:

```text
Projects/devops-learning
```

Useful shortcuts:

```text
.    Current directory
..   Parent directory
~    Home directory
-    Previous directory
```

---

# 4. Directory Navigation

## Show Current Directory

```bash
pwd
```

`pwd` means:

```text
Print Working Directory
```

Example:

```text
/home/afroza/Linux-Practice
```

---

## Change Directory

Go to home:

```bash
cd ~
```

or simply:

```bash
cd
```

Go to filesystem root:

```bash
cd /
```

Go to parent directory:

```bash
cd ..
```

Return to previous directory:

```bash
cd -
```

Example:

```bash
cd ~/Projects
```

---

# 5. Listing Files and Directories

Basic listing:

```bash
ls
```

Detailed listing:

```bash
ls -l
```

Show hidden files:

```bash
ls -a
```

Human-readable detailed output:

```bash
ls -lh
```

Combined:

```bash
ls -lah
```

Example:

```bash
ls -lah
```

This displays:

- permissions
- owner
- group
- file size
- modification time
- hidden files
- file/directory names

---

# 6. Hidden Files

Linux hidden files normally begin with a dot:

```text
.
```

Examples from my home directory:

```text
.bashrc
.bash_history
.profile
.gitconfig
.ssh
.cache
.config
.local
```

Display them with:

```bash
ls -a
```

or:

```bash
ls -lah
```

---

# 7. Viewing Directory Trees

I installed and used `tree` to visualize directory structures.

Example:

```bash
tree
```

Example output:

```text
.
├── notes
├── projects
└── README.md
```

This is useful when inspecting project structures.

---

# 8. Creating Directories

Create one directory:

```bash
mkdir Linux-Practice
```

Create multiple directories:

```bash
mkdir notes projects backup
```

Create nested directories:

```bash
mkdir -p DevOps/Linux/Basics
```

Result:

```text
DevOps/
└── Linux/
    └── Basics/
```

The `-p` option creates parent directories when necessary.

---

# 9. Removing Empty Directories

Remove an empty directory:

```bash
rmdir backup
```

`rmdir` only removes empty directories.

For example:

```bash
rmdir DevOps
```

may return:

```text
Directory not empty
```

The child directories must be removed first, or `rm -r` can be used when appropriate.

---

# 10. Creating Files

Create an empty file:

```bash
touch notes.txt
```

Create multiple files:

```bash
touch file1.txt file2.txt file3.txt
```

`touch` can also update a file's timestamp if the file already exists.

---

# 11. Copying Files

Copy a file:

```bash
cp notes.txt notes-copy.txt
```

Copy multiple files into a directory:

```bash
cp file1.txt file2.txt notes/
```

Copy a directory recursively:

```bash
cp -r notes projects
```

Without `-r`, `cp` cannot copy a directory recursively.

---

# 12. Moving and Renaming

The `mv` command can both move and rename files.

Rename:

```bash
mv old.txt new.txt
```

Move:

```bash
mv file.txt projects/
```

Therefore:

```text
mv = Move + Rename
```

---

# 13. Removing Files and Directories

Remove a file:

```bash
rm file1.txt
```

Remove multiple files:

```bash
rm file1.txt file2.txt
```

Remove a directory recursively:

```bash
rm -r projects
```

Important:

```bash
rm -rf directory
```

is powerful and potentially destructive.

`rm -rf` should always be used carefully, especially with `sudo`.

---

# 14. Inspecting Files

Determine file type:

```bash
file notes.txt
```

Example output:

```text
notes.txt: empty
```

Display detailed metadata:

```bash
stat notes.txt
```

`stat` can display information such as:

- file size
- inode
- permissions
- owner
- group
- access time
- modification time
- metadata change time

---

# 15. Writing Text to Files

Write text and overwrite existing content:

```bash
echo "Hello Linux" > notes.txt
```

Append text:

```bash
echo "Welcome to DevOps" >> notes.txt
```

Difference:

```text
>   Overwrite
>>  Append
```

Example:

```bash
echo "Ubuntu is awesome" >> notes.txt
echo "Learning Linux" >> notes.txt
echo "Backend Development" >> notes.txt
echo "Docker and Kubernetes" >> notes.txt
```

---

# 16. Reading Files

Display entire file:

```bash
cat notes.txt
```

Display first lines:

```bash
head notes.txt
```

Example:

```bash
head -3 notes.txt
```

Display last lines:

```bash
tail notes.txt
```

Example:

```bash
tail -2 notes.txt
```

Display with line numbers:

```bash
nl notes.txt
```

---

# 17. Counting Lines, Words, and Bytes

Command:

```bash
wc notes.txt
```

Example output:

```text
6 15 105 notes.txt
```

Meaning:

```text
Lines Words Bytes
```

Specific options:

```bash
wc -l notes.txt
wc -w notes.txt
wc -c notes.txt
```

| Command | Meaning |
|---|---|
| `wc -l` | Count lines |
| `wc -w` | Count words |
| `wc -c` | Count bytes |

---

# 18. Searching Files with find

Search for `.txt` files:

```bash
find . -name "*.txt"
```

Search from home:

```bash
find ~ -name "notes.txt"
```

Find directories:

```bash
find . -type d
```

Find regular files:

```bash
find . -type f
```

Important distinction:

`find` searches the filesystem directly.

---

# 19. Finding Commands

Find executable location:

```bash
which git
```

Output:

```text
/usr/bin/git
```

Other examples:

```bash
which python3
which nano
```

`which` is useful for finding the executable that will run from the current shell environment.

---

# 20. Using whereis

Examples:

```bash
whereis git
whereis python3
whereis bash
```

Unlike `which`, `whereis` may display executable, manual page, and related standard locations.

Example:

```text
git: /usr/bin/git /usr/share/man/man1/git.1.gz
```

---

# 21. Using locate

Initially `locate` was unavailable.

I installed `plocate`:

```bash
sudo apt install plocate -y
```

Updated its database:

```bash
sudo updatedb
```

Then searched:

```bash
locate notes.txt
```

Example result:

```text
/home/afroza/Linux-Practice/notes.txt
```

## find vs locate

| Command | Method |
|---|---|
| `find` | Searches filesystem directly |
| `locate` | Searches a prebuilt database |

`locate` is usually faster, but its database may need updating.

---

# 22. Searching Text with grep

Example file:

```text
Rahim
Karim
Rahim
Sakib
Karim
Hasan
Sakib
```

Search:

```bash
grep "Rahim" students.txt
```

Output:

```text
Rahim
Rahim
```

`grep` is one of the most important Linux commands for searching text.

A DevOps-style example:

```bash
grep "404" access.log
```

This could be used to search web server logs for HTTP 404 responses.

---

# 23. Sorting Text

Command:

```bash
sort students.txt
```

Example output:

```text
Hasan
Karim
Karim
Rahim
Rahim
Sakib
Sakib
```

---

# 24. Removing Duplicate Lines

Command:

```bash
sort students.txt | uniq
```

Output:

```text
Hasan
Karim
Rahim
Sakib
```

`uniq` works best when duplicate lines are adjacent, which is why it is commonly combined with `sort`.

---

# 25. Text Transformation with tr

Convert lowercase text to uppercase:

```bash
tr '[:lower:]' '[:upper:]' < students.txt
```

Example output:

```text
RAHIM
KARIM
RAHIM
SAKIB
KARIM
HASAN
SAKIB
```

---

# 26. Extracting Fields with cut

Created:

```bash
echo "Afroza,26,Bangladesh" > info.csv
```

Extract first field:

```bash
cut -d "," -f1 info.csv
```

Output:

```text
Afroza
```

Extract second field:

```bash
cut -d "," -f2 info.csv
```

Output:

```text
26
```

Explanation:

```text
-d   delimiter
-f   field
```

---

# 27. Pipes

The pipe operator is:

```text
|
```

It sends the standard output of one command to the standard input of another.

Example:

```bash
sort students.txt | uniq
```

Conceptually:

```text
Command 1
   ↓
 output
   ↓
   |
   ↓
 input
   ↓
Command 2
```

DevOps example:

```bash
docker ps | grep nginx
```

This would filter running Docker containers for entries containing `nginx`.

Docker was not installed at this stage, so this command was used only to understand the pipeline concept.

---

# 28. Output Redirection

Overwrite:

```bash
echo "Linux" > file.txt
```

Append:

```bash
echo "DevOps" >> file.txt
```

Input redirection:

```bash
command < file.txt
```

Example:

```bash
tr '[:lower:]' '[:upper:]' < students.txt
```

---

# 29. Command History

Show command history:

```bash
history
```

Show recent commands:

```bash
history | tail
```

Show first commands:

```bash
history | head
```

Command history is especially useful for:

- finding previously executed commands
- debugging
- documentation
- repeating administrative tasks
- reviewing system changes

---

# 30. Hard Links

Created a hard link:

```bash
ln notes.txt hardlink.txt
```

Checked inode numbers:

```bash
ls -li
```

Both files shared the same inode.

Example concept:

```text
notes.txt --------\
                   → Same inode → Same underlying data
hardlink.txt -----/
```

After deleting the original filename:

```bash
rm notes.txt
```

the hard link still worked:

```bash
cat hardlink.txt
```

The data remained available because another hard link still referenced the inode.

---

# 31. Symbolic Links

Created a symbolic link:

```bash
ln -s notes.txt softlink.txt
```

Checked:

```bash
ls -l
```

Example:

```text
softlink.txt -> notes.txt
```

A symbolic link stores a path to another file.

After deleting `notes.txt`, accessing the symlink failed because its target no longer existed.

This is known as a broken or dangling symbolic link.

---

# 32. Hard Link vs Symbolic Link

| Feature | Hard Link | Symbolic Link |
|---|---|---|
| Command | `ln` | `ln -s` |
| Same inode as target | Yes | No |
| Stores target pathname | No | Yes |
| Survives deletion of original filename | Yes, while another hard link remains | Usually no |
| Can point to directories normally | Restricted | Yes |
| Can cross filesystems | No | Yes |
| Can become broken | Not in the same way | Yes |

---

# 33. ZIP Archives

Created:

```bash
echo "Linux DevOps Practice" > demo.txt
```

Compressed:

```bash
zip demo.zip demo.txt
```

Extracted:

```bash
unzip demo.zip
```

ZIP is a common archive/compression format and is widely supported across operating systems.

---

# 34. TAR Archives

Create TAR archive:

```bash
tar -cvf demo.tar demo.txt
```

Options:

```text
c = create
v = verbose
f = archive file
```

Inspect archive:

```bash
tar -tvf demo.tar
```

Here:

```text
t = list archive contents
```

Extract:

```bash
mkdir extract
tar -xvf demo.tar -C extract
```

Here:

```text
x  = extract
-C = extract into specified directory
```

---

# 35. gzip and gunzip

Compress:

```bash
gzip demo.txt
```

Result:

```text
demo.txt.gz
```

Decompress:

```bash
gunzip demo.txt.gz
```

Result:

```text
demo.txt
```

Important distinction:

`tar` primarily creates archives, while `gzip` primarily performs compression.

They are often combined:

```bash
tar -czvf archive.tar.gz directory/
```

Extraction:

```bash
tar -xzvf archive.tar.gz
```

---

# 36. Linux File Permissions

Example:

```text
-rw-rw-r--
```

Permission structure:

```text
- rw- rw- r--
│ │   │   │
│ │   │   └── Others
│ │   └────── Group
│ └────────── Owner
└──────────── File type
```

Permission symbols:

| Symbol | Meaning |
|---|---|
| `r` | Read |
| `w` | Write |
| `x` | Execute |
| `-` | Permission absent |

Numeric values:

```text
r = 4
w = 2
x = 1
```

Therefore:

```text
7 = rwx
6 = rw-
5 = r-x
4 = r--
```

Example:

```bash
chmod 755 script.sh
```

means:

```text
Owner  = rwx = 7
Group  = r-x = 5
Others = r-x = 5
```

---

# 37. Ownership

A long listing can show:

```text
-rw-rw-r-- 1 afroza afroza 105 notes.txt
```

Here:

```text
Owner = afroza
Group = afroza
```

Ownership can be changed using:

```bash
chown
```

Example syntax:

```bash
sudo chown user:group file
```

Ownership changes should only be performed when required.

---

# 38. Important Commands Practiced

| Command | Purpose |
|---|---|
| `pwd` | Show current directory |
| `cd` | Change directory |
| `ls` | List directory contents |
| `mkdir` | Create directory |
| `rmdir` | Remove empty directory |
| `touch` | Create empty file/update timestamp |
| `cp` | Copy |
| `mv` | Move/rename |
| `rm` | Remove |
| `tree` | Display directory tree |
| `cat` | Display/concatenate files |
| `head` | First lines |
| `tail` | Last lines |
| `nl` | Number lines |
| `wc` | Count lines/words/bytes |
| `file` | Determine file type |
| `stat` | File metadata |
| `find` | Search filesystem |
| `locate` | Database-based file search |
| `which` | Locate command executable |
| `whereis` | Locate binary/manual-related paths |
| `grep` | Search text |
| `sort` | Sort lines |
| `uniq` | Filter adjacent duplicate lines |
| `tr` | Translate characters |
| `cut` | Extract fields |
| `history` | Command history |
| `ln` | Hard link |
| `ln -s` | Symbolic link |
| `zip` | ZIP compression/archive |
| `unzip` | Extract ZIP |
| `tar` | Archive files |
| `gzip` | gzip compression |
| `gunzip` | gzip decompression |
| `chmod` | Change permissions |
| `chown` | Change ownership |

---

# 39. Important DevOps Command Patterns

These patterns are useful when working with servers.

Search logs:

```bash
grep "ERROR" application.log
```

Follow logs in real time:

```bash
tail -f application.log
```

Find configuration files:

```bash
find /etc -name "*.conf"
```

Find an executable:

```bash
which nginx
```

Search command history:

```bash
history | grep docker
```

Count matching log entries:

```bash
grep "404" access.log | wc -l
```

Archive a directory:

```bash
tar -czvf backup.tar.gz project/
```

Extract an archive:

```bash
tar -xzvf backup.tar.gz
```

---

# 40. Interview Quick Revision

### What is `/` in Linux?

`/` is the root of the entire Linux filesystem hierarchy.

### What is `~`?

`~` represents the current user's home directory.

For my environment:

```text
~ = /home/afroza
```

### Difference between `/` and `/root`?

`/` is the filesystem root.

`/root` is the home directory of the root user.

### What does `pwd` do?

Displays the current working directory.

### Difference between `>` and `>>`?

`>` overwrites a file.

`>>` appends to a file.

### Difference between `find` and `locate`?

`find` searches the filesystem directly.

`locate` searches a prebuilt database and is usually faster.

### What does `grep` do?

Searches text for lines matching a pattern.

### What is a pipe?

A pipe (`|`) passes the output of one command as input to another command.

Example:

```bash
cat file.txt | grep "error"
```

### Difference between hard link and symbolic link?

A hard link references the same underlying inode/data.

A symbolic link stores a pathname pointing to another file.

### What is an inode?

An inode is a filesystem data structure containing metadata about a file and references to its data blocks.

Filenames are directory entries that reference inodes.

### What does `chmod` do?

Changes file or directory permissions.

### What does `755` mean?

```text
Owner  = rwx
Group  = r-x
Others = r-x
```

### What does `chown` do?

Changes file or directory ownership.

### Difference between `tar` and `gzip`?

`tar` combines files into an archive.

`gzip` compresses data.

They are commonly combined into `.tar.gz` archives.

### What does `rm -rf` do?

`rm -r` recursively removes directories and their contents.

`-f` forces removal without normal confirmation.

It must be used carefully.

---

# 41. Command Cheat Sheet

```bash
# Navigation
pwd
cd ~
cd /
cd ..
cd -

# Listing
ls
ls -l
ls -a
ls -lah
tree

# Directories
mkdir directory
mkdir -p parent/child
rmdir directory

# Files
touch file.txt
cp source destination
cp -r source_directory destination
mv old new
rm file
rm -r directory

# Reading
cat file.txt
head file.txt
tail file.txt
tail -f file.txt
nl file.txt

# Information
file file.txt
stat file.txt
wc file.txt
wc -l file.txt
wc -w file.txt

# Searching
find . -name "*.txt"
find . -type f
find . -type d
locate filename
which command
whereis command
grep "text" file

# Text Processing
sort file.txt
sort file.txt | uniq
cut -d "," -f1 file.csv
tr '[:lower:]' '[:upper:]'

# Redirection
echo "text" > file.txt
echo "text" >> file.txt

# Pipes
command1 | command2

# History
history
history | tail
history | grep docker

# Links
ln original hardlink
ln -s target symlink

# ZIP
zip archive.zip file
unzip archive.zip

# TAR
tar -cvf archive.tar file
tar -tvf archive.tar
tar -xvf archive.tar

# TAR + gzip
tar -czvf archive.tar.gz directory
tar -xzvf archive.tar.gz

# gzip
gzip file
gunzip file.gz

# Permissions
chmod 755 file
chmod +x script.sh

# Ownership
sudo chown user:group file
```

---

# Practical Environment

The exercises in this step were performed in:

```text
Host OS: Windows
Virtualization: Oracle VirtualBox
Guest OS: Ubuntu 26.04
Linux User: afroza
Practice Directory: ~/Linux-Practice
Repository: devops-learning
```

---

# Key Takeaways

Through this step I developed practical familiarity with the Linux command line rather than only studying Linux concepts theoretically.

I practiced navigating the filesystem, managing files and directories, searching and processing text, using shell pipelines and redirection, working with links, creating archives, and understanding Linux permissions.

These fundamentals provide the base for later work involving:

- Linux servers
- Backend deployment
- Docker
- Kubernetes
- Nginx
- CI/CD
- Cloud infrastructure
- System administration
- Cybersecurity

---

## Status

**Step 03 — Linux Fundamentals: Completed**


