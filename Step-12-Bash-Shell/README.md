# Step 12 — Bash Shell

---

# Objective

Learn the Bash shell fundamentals required for professional Linux administration and DevOps workflows. This step focuses on improving command-line productivity through shell features such as command history, auto-completion, aliases, and command editing.

---

# Learning Outcomes

After completing this step, I can:

- Understand the role of Bash in Linux.
- Identify the current shell and login shell.
- Check the installed Bash version.
- Work with Bash command history.
- Use Tab auto-completion.
- Create and remove Bash aliases.
- Improve terminal productivity using common Bash shortcuts.
- Navigate the command line more efficiently.

---

# Workflow

```text
Open Terminal
        │
        ▼
Bash Shell
        │
        ▼
Enter Command
        │
        ▼
Bash Parses Command
        │
        ▼
Execute Program
        │
        ▼
Display Output
```

---

# Bash Architecture

```text
User
  │
  ▼
Keyboard Input
  │
  ▼
Bash Shell
  │
  ├── Built-in Commands
  ├── Aliases
  ├── Environment Variables
  └── External Programs
           │
           ▼
Linux Kernel
           │
           ▼
Hardware
```

---

# What is Bash?

**Bash (Bourne Again SHell)** is the default command-line shell on most Linux distributions.

It acts as a command interpreter between the user and the Linux operating system.

---

# Important Concepts

| Concept | Description |
|----------|-------------|
| Shell | Command interpreter |
| Bash | Most widely used Linux shell |
| Terminal | Program used to access the shell |
| Command | Instruction executed by Bash |
| Prompt | Waiting area for user input |
| History | Previously executed commands |
| Alias | User-defined command shortcut |
| Auto Completion | Complete commands using Tab |

---

# Bash vs Terminal

| Bash | Terminal |
|------|----------|
| Command interpreter | Application |
| Executes commands | Displays shell interface |
| Understands Linux commands | Provides input/output window |
| Can run without GUI | Runs Bash inside a window |

---

# Login Shell vs Current Shell

| Login Shell | Current Shell |
|-------------|---------------|
| Configured for the user account | Currently running shell process |
| Stored in `/etc/passwd` | Running process |
| Usually `/bin/bash` | `/bin/bash` or `/usr/bin/bash` |

---

# Interactive Shell vs Bash Script

| Interactive Shell | Bash Script |
|-------------------|-------------|
| Commands entered manually | Commands stored in a file |
| Executes immediately | Executes sequentially |
| Used every day | Used for automation |

---

# Important Files

| File | Purpose |
|------|---------|
| `/etc/passwd` | User login shell information |
| `~/.bash_history` | Command history |
| `~/.bashrc` | User Bash configuration |
| `~/.profile` | Login shell configuration |

---

# Important Directories

| Directory | Purpose |
|-----------|---------|
| `/bin` | Essential executables |
| `/usr/bin` | User commands |
| `/home` | User home directory |

---

# Summary

In this step, I learned the fundamental concepts of the Bash shell, including its architecture, workflow, command execution process, login shell, current shell, and the essential files involved in Bash configuration.


# Commands Used

## Shell Information

| Command | Description |
|----------|-------------|
| `echo $SHELL` | Display the configured login shell. |
| `echo $0` | Display the currently running shell. |
| `ps -p $$` | Show the current shell process. |
| `bash --version` | Display the installed Bash version. |
| `getent passwd "$USER"` | Display user account information including the login shell. |

---

## Command History

| Command | Description |
|----------|-------------|
| `history` | Display the complete command history. |
| `history 10` | Display the last 10 commands. |
| `!!` | Execute the previous command. |
| `!number` | Execute a command using its history number. |

---

## History Configuration

| Variable | Description |
|----------|-------------|
| `HISTFILE` | Location of the Bash history file. |
| `HISTSIZE` | Maximum number of commands stored in the current session. |
| `HISTFILESIZE` | Maximum number of commands stored in the history file. |

---

# Bash Productivity Shortcuts

| Shortcut | Purpose | Priority |
|----------|---------|----------|
| `Tab` | Auto-complete commands, files, and directories. | High |
| `Tab` (Double Press) | Display available completion options. | High |
| `↑ / ↓` | Navigate through command history. | High |
| `Ctrl + R` | Search command history. | High |
| `Ctrl + C` | Interrupt the running process. | High |
| `Ctrl + D` | Exit the current shell. | High |
| `Ctrl + L` | Clear the terminal screen. | Medium |
| `Ctrl + A` | Move the cursor to the beginning of the line. | Medium |
| `Ctrl + E` | Move the cursor to the end of the line. | Medium |
| `Ctrl + W` | Delete the previous word. | Low |
| `Ctrl + U` | Delete from the cursor to the beginning of the line. | Low |
| `Ctrl + K` | Delete from the cursor to the end of the line. | Low |
| `Alt + B` | Move backward by one word. | Low |
| `Alt + F` | Move forward by one word. | Low |

---

# Bash Aliases

## Commands Used

| Command | Description |
|----------|-------------|
| `alias` | Display all configured aliases. |
| `alias name='command'` | Create a new alias. |
| `unalias name` | Remove an alias. |
| `type alias_name` | Display the actual command behind an alias. |

---

## Alias Examples

| Alias | Original Command |
|--------|------------------|
| `ll` | `ls -lah` |
| `la` | `ls -A` |
| `l` | `ls -CF` |
| `gs` | `git status` |
| `c` | `clear` |
| `update` | `sudo apt update && sudo apt upgrade -y` |

---

# Bash Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `&&` | Execute the next command only if the previous command succeeds. | `mkdir demo && cd demo` |
| `||` | Execute the next command only if the previous command fails. | `cd demo || pwd` |
| `;` | Execute all commands regardless of success or failure. | `pwd ; date` |
| `|` | Pass the output of one command as input to another. | `ls -l \| less` |
| `>` | Redirect output to a file (overwrite). | `ls > files.txt` |
| `>>` | Append output to a file. | `date >> log.txt` |
| `<` | Read input from a file. | `sort < names.txt` |

---

# Common Mistakes

| Mistake | Explanation |
|----------|-------------|
| Typing `<Tab>` literally | Press the **Tab** key instead of typing `<Tab>`. |
| Assuming aliases are permanent | Aliases created using `alias` exist only in the current shell session unless added to `~/.bashrc`. |
| Running `!!` without verification | It immediately executes the previous command. |
| Using `gs` outside a Git repository | Produces `fatal: not a git repository`, which is expected. |

---

# Real-World Usage

| Feature | Practical Usage |
|----------|-----------------|
| Command History | Reuse previously executed commands. |
| Auto Completion | Reduce typing and avoid typing errors. |
| Aliases | Create shortcuts for frequently used commands. |
| Command Operators | Build efficient command workflows. |
| Pipes | Connect multiple commands into a processing pipeline. |
| Redirection | Store command output in log or configuration files. |

---

# Configuration Files

| File | Purpose |
|------|---------|
| `~/.bashrc` | User-specific Bash configuration. |
| `~/.bash_history` | Stores command history. |
| `/etc/passwd` | Stores the configured login shell for each user. |

# Skills Acquired

After completing this step, the following Bash skills were practiced and verified:

- Identified the configured login shell.
- Verified the current running shell.
- Checked the installed Bash version.
- Examined the current shell process.
- Explored Bash command history.
- Verified Bash history configuration.
- Used Tab auto-completion.
- Created and tested temporary aliases.
- Inspected existing system aliases.
- Removed temporary aliases.
- Understood commonly used Bash productivity shortcuts.
- Learned the purpose of Bash command operators and redirection.

---

# Real Lab Summary

| Item | Observation |
|------|-------------|
| Operating System | Ubuntu 26.04 LTS (Virtual Machine) |
| Shell | Bash |
| Login Shell | `/bin/bash` |
| Current Shell | `/usr/bin/bash` |
| Shell Process | `bash` |
| Bash Version | GNU Bash 5.3.9 |
| History File | `/home/afroza/.bash_history` |
| HISTSIZE | 1000 |
| HISTFILESIZE | 2000 |
| Practice Directory | `~/Linux-Practice/tab-demo` |

---

# Files Used During Practice

| File | Purpose |
|------|---------|
| `docker-compose.yml` | Tab completion practice |
| `dockerfile` | Tab completion practice |
| `deployment.yaml` | Tab completion practice |
| `devops.txt` | Tab completion practice |
| `~/.bash_history` | Bash command history |
| `~/.bashrc` | Bash configuration reference |

---

# Directories Used

| Directory | Purpose |
|-----------|---------|
| `~/Linux-Practice` | Practice workspace |
| `~/Linux-Practice/tab-demo` | Bash practice directory |
| `~/Projects/devops-learning/Step-12-Bash-Shell` | Documentation |

---

# Environment Variables Observed

| Variable | Value / Purpose |
|----------|-----------------|
| `SHELL` | Configured login shell |
| `USER` | Current logged-in user |
| `HISTFILE` | History file location |
| `HISTSIZE` | Session history limit |
| `HISTFILESIZE` | History file limit |

---

# Important Commands

| Category | Commands |
|----------|----------|
| Shell Information | `echo`, `ps`, `bash --version`, `getent` |
| History | `history`, `!!`, `!number` |
| Alias | `alias`, `unalias`, `type` |
| Navigation | `pwd`, `cd`, `ls` |
| Productivity | `Tab`, `Ctrl + R`, `Ctrl + C`, `Ctrl + D`, `Ctrl + L` |

---

# Important Bash Operators

| Operator | Purpose |
|----------|---------|
| `&&` | Execute the next command only if the previous command succeeds. |
| `||` | Execute the next command only if the previous command fails. |
| `;` | Execute commands sequentially regardless of success or failure. |
| `|` | Pass output from one command to another. |
| `>` | Redirect output to a file (overwrite). |
| `>>` | Append output to a file. |
| `<` | Read input from a file. |

---

# Folder Structure

```text
Step-12-Bash-Shell/
└── README.md
```

---
# Step Summary

This step focused on using the Bash shell efficiently in day-to-day Linux administration. Practical exercises covered shell identification, command history, history configuration, auto-completion, aliases, and commonly used productivity features required for backend development and DevOps workflows.


