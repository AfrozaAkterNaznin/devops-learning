# Step 11 — Shell Environment & Bash Configuration

---

# Objective

In this step, we learned how Bash stores and uses environment variables, shell variables, aliases, functions, history, startup configuration files, and special variables.

The goal is to understand how the Linux shell works internally and how to customize it safely.

---

# Learning Outcomes

After completing this step I can:

- Understand Shell Variables
- Understand Environment Variables
- Export Variables
- Remove Variables
- Understand Child Shell
- Understand .bashrc
- Understand .profile
- Create Aliases
- Remove Aliases
- Create Functions
- Remove Functions
- Understand Bash History
- Understand Special Variables
- Pass Arguments into Scripts
- Reload Bash Configuration

---

# Workflow

```
Shell
      │
      ▼
Shell Variable
      │
      ▼
Environment Variable
      │
      ▼
export
      │
      ▼
Child Shell
      │
      ▼
.bashrc
      │
      ▼
.profile
      │
      ▼
Aliases
      │
      ▼
Functions
      │
      ▼
History
      │
      ▼
Special Variables
      │
      ▼
Script Arguments
      │
      ▼
Cleanup
```

---

# Linux Shell Environment

A shell keeps information about the current session.

Examples

- Username
- Home Directory
- Current Directory
- PATH
- Shell
- Language
- History
- Variables

These values are stored as variables.

---

# Shell Variable vs Environment Variable

| Shell Variable | Environment Variable |
|---------------|----------------------|
| Current shell only | Available to child shells |
| Created normally | Created using export |
| Temporary | Can be exported |
| Example: CITY=Dhaka | export CITY=Dhaka |

---

# Shell Startup Order

```
Login

↓

/etc/profile

↓

~/.profile

↓

~/.bashrc

↓

Interactive Shell
```

---

# Important Configuration Files

| File | Purpose |
|------|----------|
| ~/.bashrc | Interactive shell configuration |
| ~/.profile | Login shell configuration |
| /etc/profile | Global login configuration |
| /etc/bash.bashrc | Global bash configuration |

---

# Important Environment Variables

| Variable | Meaning |
|----------|----------|
| USER | Username |
| HOME | Home directory |
| SHELL | Default shell |
| PATH | Command search path |
| PWD | Current directory |
| LANG | System language |
| HISTFILE | History file |
| HISTSIZE | Memory history size |
| HISTFILESIZE | File history size |

---

# Export

Export makes a variable available to child processes.

Example

```
CITY=Dhaka

export CITY
```

Child shell

```
bash

echo $CITY
```

---

# Child Shell

A child shell inherits exported variables.

Non-exported variables stay only in the current shell.

```
Parent Shell

↓

Child Shell
```

---

# .bashrc

Used for

- Aliases
- Functions
- Environment Variables
- Prompt customization

Reload

```
source ~/.bashrc
```

---

# .profile

Runs during login.

Usually loads .bashrc automatically.

```
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
```
---

# Aliases

An alias is a shortcut for a command.

Instead of writing a long command every time, we can create a short name.

Example

```bash
alias ll='ls -alF'
```

Remove an alias

```bash
unalias ll
```

List aliases

```bash
alias
```

---

# Functions

A function stores multiple commands together.

Example

```bash
myinfo() {
    echo "User : $USER"
    echo "Home : $HOME"
    echo "Shell: $SHELL"
}
```

Run

```bash
myinfo
```

Remove

```bash
unset -f myinfo
```

List functions

```bash
declare -F
```

---

# Bash History

Bash automatically stores executed commands.

History file

```text
~/.bash_history
```

Useful commands

```bash
history

history | tail

history | grep ssh
```

History settings

| Variable | Meaning |
|----------|----------|
| HISTSIZE | Commands kept in memory |
| HISTFILESIZE | Commands stored in history file |
| HISTFILE | History file location |
| HISTCONTROL | History behavior |

---

# Special Variables

| Variable | Meaning |
|----------|----------|
| $? | Exit status of previous command |
| $$ | Current shell PID |
| $PPID | Parent process ID |
| $0 | Current shell or script |
| $# | Number of arguments |
| $* | All arguments |
| $@ | All arguments separately |
| $PWD | Current directory |
| $OLDPWD | Previous directory |

Example

```bash
echo $$
echo $?
echo $PWD
```

---

# Script Arguments

Example

```bash
./demo-args.sh Linux DevOps Bash
```

Output

| Variable | Value |
|----------|-------|
| $0 | Script name |
| $1 | Linux |
| $2 | DevOps |
| $3 | Bash |
| $# | 3 |

---

# Commands Used

| Command | Purpose |
|----------|----------|
| env | Show environment variables |
| printenv | Print environment variables |
| set | Show shell variables |
| export | Export variable |
| unset | Remove variable |
| source | Reload configuration |
| alias | Create alias |
| unalias | Remove alias |
| declare -F | List functions |
| unset -f | Remove function |
| history | Show history |
| grep | Search |
| echo | Print value |
| chmod | Change permission |
| bash | Start child shell |

---

# Command Variations

| Command | Description |
|----------|-------------|
| env | Show environment |
| printenv | Environment only |
| printenv PATH | Show one variable |
| echo $PATH | Print PATH |
| export NAME=value | Create exported variable |
| export NAME | Export existing variable |
| unset NAME | Delete variable |
| alias | List aliases |
| alias ll='ls -alF' | Create alias |
| unalias ll | Remove alias |
| history | Full history |
| history \| tail | Recent history |
| history \| grep ssh | Search history |
| source ~/.bashrc | Reload bashrc |
| . ~/.bashrc | Same as source |
| declare -F | List functions |
| unset -f hello | Delete function |

---

# What to Observe

| Lab | Observe |
|-----|----------|
| User | USER, HOME, SHELL |
| PATH | Multiple directories separated by ':' |
| export | Variable visible in child shell |
| Child shell | Exported variables available |
| unset | Variable disappears |
| .bashrc | Custom variables loaded |
| .profile | Loads .bashrc |
| alias | Shortcut command |
| function | Multiple commands inside one command |
| history | Previously executed commands |
| HISTSIZE | Maximum memory history |
| HISTFILE | ~/.bash_history |
| $? | Success = 0, Failure ≠ 0 |
| $$ | Current shell PID |
| $PPID | Parent process |
| $PWD | Current working directory |

---

# Common Mistakes

| Mistake | Solution |
|----------|----------|
| Forgot export | Use `export` |
| Forgot source | Run `source ~/.bashrc` |
| Alias not working | Check using `alias` |
| Function not found | Define it again |
| Variable empty | Check spelling and export |
| PATH edited incorrectly | Restore from backup |
| Forgot unset | Remove unwanted variable |
| Wrong bashrc syntax | Verify before reloading |

---

# Real World Uses

| Feature | Practical Use |
|----------|---------------|
| PATH | Add custom software |
| export | Application configuration |
| Alias | Faster daily work |
| Function | Automate repetitive tasks |
| .bashrc | Developer environment |
| History | Troubleshooting |
| Special Variables | Bash scripting |
| Script Arguments | Automation scripts |
| Environment Variables | Docker, Kubernetes, CI/CD, Jenkins, GitHub Actions |

---

# Interview Questions

| Question | Short Answer |
|----------|--------------|
| Difference between shell variable and environment variable? | Exported variables are inherited by child shells. |
| What is PATH? | Command search path. |
| What is .bashrc? | Interactive shell configuration. |
| What is .profile? | Login shell configuration. |
| What is export? | Makes variables available to child processes. |
| Difference between `$*` and `$@`? | Both contain all arguments; `$@` preserves argument boundaries when quoted. |
| What does `$?` return? | Exit status of previous command. |
| Where is history stored? | `~/.bash_history` |

---

# Lab Summary

During this lab, the Bash shell environment was explored from a practical Linux administration perspective.

The following topics were completed successfully:

- Shell Variables
- Environment Variables
- Export Variables
- Child Shell
- .bashrc Configuration
- .profile Configuration
- Aliases
- Functions
- Bash History
- Special Variables
- Script Arguments
- Environment Cleanup

---

# Skills Gained

After completing this lab I can:

✅ Identify shell variables

✅ Identify environment variables

✅ Export variables to child shells

✅ Remove variables safely

✅ Understand Bash startup sequence

✅ Modify `.bashrc`

✅ Understand `.profile`

✅ Reload Bash configuration

✅ Create custom aliases

✅ Remove aliases

✅ Create shell functions

✅ Remove shell functions

✅ Read Bash history

✅ Understand special shell variables

✅ Pass arguments into Bash scripts

✅ Check command exit status

---

# Real Lab Results

| Item | Result |
|------|--------|
| Current Shell | `/usr/bin/bash` |
| Home Directory | `/home/afroza` |
| Working Directory | `/home/afroza/Linux-Practice` |
| History File | `~/.bash_history` |
| History Size | `1000` |
| History File Size | `2000` |
| History Control | `ignoreboth` |
| Startup File | `~/.bashrc` |
| Login File | `~/.profile` |
| Default Shell | Bash |
| PATH | Successfully verified |
| Export Variables | Successfully tested |
| Child Shell | Successfully verified |
| Aliases | Successfully created and removed |
| Functions | Successfully created and removed |
| Script Arguments | Successfully tested |
| Exit Status | Successfully tested |

---

# Files Used

| File | Purpose |
|------|----------|
| `~/.bashrc` | Bash configuration |
| `~/.profile` | Login shell configuration |
| `~/.bash_history` | Command history |
| `demo-args.sh` | Script argument demonstration |

---

# Important Commands Learned

```text
env
printenv
set
export
unset
source
alias
unalias
history
declare -F
unset -f
echo
grep
chmod
bash
```

---

# Important Variables Learned

```text
USER
HOME
SHELL
PATH
PWD
OLDPWD
LANG
HISTFILE
HISTSIZE
HISTFILESIZE

$?
$$
$PPID
$0
$#
$*
$@
```

---

# Key Takeaways

- Shell variables exist only in the current shell.
- Environment variables are inherited by child processes.
- `export` makes variables available outside the current shell.
- `.bashrc` configures interactive Bash sessions.
- `.profile` is executed during login.
- `source ~/.bashrc` reloads configuration without restarting the terminal.
- Aliases simplify long commands.
- Functions group multiple commands into one reusable command.
- Bash automatically stores command history.
- Special variables provide information about the current shell and scripts.
- Script arguments make Bash scripts reusable.

---

# Folder Structure

```text
Step-11-Shell-Environment/
│
├── README.md
```

---

# Commands Practiced

| Category | Status |
|----------|--------|
| Shell Variables | ✅ |
| Environment Variables | ✅ |
| Export | ✅ |
| Child Shell | ✅ |
| .bashrc | ✅ |
| .profile | ✅ |
| Alias | ✅ |
| Functions | ✅ |
| History | ✅ |
| Special Variables | ✅ |
| Script Arguments | ✅ |
| Cleanup | ✅ |

---

# Step Completion

**Step 11 — Shell Environment & Bash Configuration**

Status:

```text
COMPLETED ✅
```


