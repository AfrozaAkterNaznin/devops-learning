# Step 13 — Bash Scripting

---

# Objective

Learn how to create, execute, and automate tasks using Bash scripts. This step introduces scripting fundamentals and practical automation techniques commonly used in Linux administration, backend development, and DevOps.

---

# Learning Outcomes

After completing this step, I can:

- Create Bash scripts.
- Execute scripts using different methods.
- Use variables and user input.
- Pass command-line arguments.
- Check command exit status.
- Write conditional statements.
- Use loops and functions.
- Build simple Linux automation scripts.

---

# Bash Scripting Workflow

```text
Write Script
      │
      ▼
Save (.sh)
      │
      ▼
Add Shebang
      │
      ▼
Grant Execute Permission (Optional)
      │
      ▼
Execute Script
      │
      ▼
Perform Automation
```

---

# Bash Script Execution Flow

```text
User
 │
 ▼
Bash Script (.sh)
 │
 ▼
Bash Interpreter
 │
 ▼
Linux Commands
 │
 ▼
Linux Kernel
 │
 ▼
System Resources
 │
 ▼
Output
```

---

# What is a Bash Script?

A Bash script is a text file containing one or more Linux commands executed sequentially by the Bash interpreter. Scripts are primarily used to automate repetitive tasks and simplify system administration.

---

# Shebang

The first line of a Bash script is called the **Shebang**.

```bash
#!/bin/bash
```

It specifies which interpreter should execute the script.

---

# Script Execution Methods

| Method | Execute Permission Required | Uses Shebang |
|---------|----------------------------|--------------|
| `bash script.sh` | No | No |
| `./script.sh` | Yes | Yes |

---

# Bash Script vs Interactive Shell

| Bash Script | Interactive Shell |
|-------------|-------------------|
| Commands stored in a file | Commands typed manually |
| Executes sequentially | Executes immediately |
| Used for automation | Used for daily administration |
| Reusable | Temporary |

---

# Script Components

| Component | Purpose |
|-----------|---------|
| Shebang | Defines the interpreter |
| Variables | Store values |
| Input | Receive user input |
| Arguments | Accept command-line parameters |
| Conditions | Make decisions |
| Loops | Repeat tasks |
| Functions | Reuse code |
| Exit Status | Check command success or failure |

---

# Automation Workflow

```text
User
 │
 ▼
Execute Script
 │
 ▼
Receive Input
 │
 ▼
Process Logic
 │
 ├── Variables
 ├── Conditions
 ├── Loops
 └── Functions
 │
 ▼
Execute Linux Commands
 │
 ▼
Display Result
```

---

# Important Files

| File | Purpose |
|------|---------|
| `*.sh` | Bash script files |
| `~/.bashrc` | Bash configuration |
| `~/.profile` | Login shell configuration |

---

# Important Directories

| Directory | Purpose |
|-----------|---------|
| `~/Linux-Practice/bash-scripting` | Script practice directory |
| `~/Projects/devops-learning/Step-13-Bash-Scripting` | Documentation |

---


# Commands Used

## Script Creation & Execution

| Command | Description |
|----------|-------------|
| `nano script.sh` | Create or edit a Bash script. |
| `bash script.sh` | Execute a script using the Bash interpreter. |
| `chmod +x script.sh` | Grant execute permission to a script. |
| `./script.sh` | Execute a script directly. |
| `cat script.sh` | Display the script contents. |
| `ls -l` | View file permissions. |

---

# Variables

## Syntax

```bash
variable_name="value"
```

## Variable Expansion

```bash
echo "$variable_name"
```

## Variables Used

| Variable | Description |
|----------|-------------|
| `name` | Store user name. |
| `course` | Store course name. |
| `count` | Loop counter. |
| `username` | Store user input. |
| `dir` | Store directory name. |

---

# User Input

## Command Used

```bash
read variable_name
```

## Example

```bash
echo -n "Enter your name: "
read name
```

| Command | Purpose |
|----------|---------|
| `read` | Read input from the keyboard. |
| `echo -n` | Display text without moving to a new line. |

---

# Positional Parameters

| Parameter | Description |
|-----------|-------------|
| `$1` | First argument |
| `$2` | Second argument |
| `$@` | All arguments |
| `$#` | Total number of arguments |

## Example

```bash
bash args.sh Linux DevOps Bash
```

---

# Exit Status

| Value | Meaning |
|--------|---------|
| `0` | Command executed successfully. |
| Non-zero | Command execution failed. |

## Commands Used

```bash
echo $?
exit 0
exit 1
```

---

# Conditional Statements

## if Statement

```bash
if [ condition ]; then
    command
else
    command
fi
```

---

## if-else Workflow

```text
Condition
    │
    ├── True
    │      │
    │      ▼
    │   Execute Block
    │
    └── False
           │
           ▼
      Execute Else Block
```

---

## case Statement

```bash
case "$variable" in
    pattern)
        command
        ;;
    *)
        default_command
        ;;
esac
```

---

# Loops

## for Loop

```bash
for item in list
do
    command
done
```

---

## while Loop

```bash
while [ condition ]
do
    command
done
```

---

# Functions

## Syntax

```bash
function_name() {
    commands
}
```

## Function Call

```bash
function_name
```

---

# Automation Scripts Created

| Script | Purpose |
|--------|---------|
| `hello.sh` | First Bash script |
| `variables.sh` | Variable demonstration |
| `input.sh` | User input |
| `args.sh` | Positional parameters |
| `exit-status.sh` | Exit status checking |
| `check-directory.sh` | Directory existence check |
| `user-check.sh` | Verify Linux user |
| `menu.sh` | Menu using case statement |
| `for-loop.sh` | for loop demonstration |
| `while-loop.sh` | while loop demonstration |
| `function.sh` | Function demonstration |
| `system-info.sh` | Display system information |
| `project-init.sh` | Create project structure |
| `disk-check.sh` | Display disk usage |
| `backup.sh` | Backup Bash scripts |

---

# Common Mistakes

| Mistake | Explanation |
|----------|-------------|
| Missing Shebang | The interpreter cannot be determined when executing with `./script.sh`. |
| Missing Execute Permission | Direct execution fails without execute permission. |
| Incorrect Variable Syntax | Do not use spaces around `=`. |
| Forgetting Quotes | Can cause unexpected behavior with spaces in values. |
| Incorrect File Path | Relative paths depend on the current working directory. |
| Ignoring Exit Status | Makes debugging and automation more difficult. |

---

# Real-World Usage

| Script Type | Practical Usage |
|-------------|-----------------|
| System Information | Display host and system details |
| Backup Script | Backup important files |
| User Verification | Validate Linux users |
| Project Initialization | Create project structures |
| Disk Monitoring | Check available disk space |
| Interactive Menu | Build administration utilities |

---

# Interview Questions

| Question | Short Answer |
|----------|--------------|
| What is a Bash script? | A text file containing Bash commands for automation. |
| What is a Shebang? | The first line that specifies the script interpreter. |
| Difference between `bash script.sh` and `./script.sh`? | Direct execution requires execute permission and uses the Shebang. |
| What is `$1`? | First command-line argument. |
| What does `$?` represent? | Exit status of the previously executed command. |
| What is the purpose of `read`? | Accept user input from the terminal. |
| Why are functions used? | To organize and reuse code. |


# Skills Acquired

After completing this step, the following Bash scripting skills were practiced and verified:

- Created executable Bash scripts.
- Executed scripts using different methods.
- Used variables and variable expansion.
- Accepted user input using `read`.
- Passed command-line arguments.
- Retrieved positional parameters.
- Checked command exit status.
- Implemented conditional statements.
- Used `case` statements for menu-driven scripts.
- Implemented `for` and `while` loops.
- Created reusable functions.
- Developed practical Linux automation scripts.

---

# Real Lab Summary

| Item | Observation |
|------|-------------|
| Operating System | Ubuntu 26.04 LTS (Virtual Machine) |
| Shell | Bash |
| Practice Directory | `~/Linux-Practice/bash-scripting` |
| Documentation Directory | `~/Projects/devops-learning/Step-13-Bash-Scripting` |
| Script Extension | `.sh` |
| Interpreter | `/bin/bash` |
| Script Execution | `bash script.sh` and `./script.sh` |
| Execute Permission | Added using `chmod +x` |

---

# Scripts Created

| Script | Purpose |
|--------|---------|
| `hello.sh` | First Bash script |
| `variables.sh` | Variables |
| `input.sh` | User input |
| `args.sh` | Positional parameters |
| `exit-status.sh` | Exit status |
| `check-directory.sh` | Directory validation |
| `user-check.sh` | User verification |
| `menu.sh` | Menu-driven script |
| `for-loop.sh` | for loop |
| `while-loop.sh` | while loop |
| `function.sh` | Functions |
| `system-info.sh` | System information |
| `project-init.sh` | Project initialization |
| `disk-check.sh` | Disk usage monitoring |
| `backup.sh` | Backup automation |

---

# Commands Practiced

| Category | Commands |
|----------|----------|
| Script Creation | `nano`, `cat`, `ls -l` |
| Script Execution | `bash`, `chmod +x`, `./script.sh` |
| Variables | `echo`, `read` |
| User Information | `whoami`, `hostname` |
| System Information | `uname`, `date`, `uptime` |
| User Verification | `id` |
| File Operations | `mkdir`, `cp`, `tree` |
| Storage | `df -h` |

---

# Bash Variables Used

| Variable | Purpose |
|----------|---------|
| `$1` | First positional argument |
| `$2` | Second positional argument |
| `$@` | All positional arguments |
| `$#` | Total number of positional arguments |
| `$?` | Exit status of the previous command |
| `$SHELL` | Current login shell |

---

# Files Used

| File | Purpose |
|------|---------|
| `*.sh` | Bash scripts |
| `~/.bashrc` | Bash configuration reference |
| `~/.bash_history` | Command history reference |

---

# Directories Used

| Directory | Purpose |
|-----------|---------|
| `~/Linux-Practice/bash-scripting` | Script development and testing |
| `~/Linux-Practice/bash-scripting/backup` | Backup directory |
| `~/Projects/devops-learning/Step-13-Bash-Scripting` | Documentation |

---

# Folder Structure

```text
Step-13-Bash-Scripting/
└── README.md
```

---

# Step Summary

This step introduced Bash scripting fundamentals through practical implementation. Core scripting concepts—including variables, user input, positional parameters, exit status, conditional statements, loops, functions, and automation—were applied by developing multiple reusable Linux administration scripts. These scripting skills establish the foundation for task scheduling, infrastructure automation, and CI/CD workflows.

---




