# Step 05 — Linux Processes, Jobs & Process Management

---

# Overview

This step covers the fundamentals of Linux process management.

A Linux system continuously runs hundreds of processes. Understanding how to inspect, monitor, search, control and manage these processes is an essential skill for Linux administrators, Backend Developers and DevOps Engineers.

This chapter focuses on process fundamentals, process monitoring and process inspection.

---

# Learning Objectives

After completing this step, you will be able to:

- Understand Program vs Process
- Understand Linux Process Lifecycle
- Understand PID and PPID
- Monitor running processes
- Inspect process information
- Use process monitoring tools
- Search running processes
- Control processes
- Manage foreground and background jobs
- Understand process priority
- Keep processes running after terminal logout

---

# Commands Covered

## Process Information

```bash
ps
ps -f
ps -ef
ps aux
```

---

## Process Monitoring

```bash
top
htop
```

---

## Process Searching

```bash
pgrep
pidof
```

---

## Process Control

```bash
kill
killall
pkill
```

---

## Jobs

```bash
jobs
bg
fg
```

---

## Persistent Process

```bash
nohup
```

---

## Priority

```bash
nice
renice
```

---

# Process Basics

## What is a Program?

A program is an executable file stored on disk.

Example:

- bash
- python
- nginx
- docker

A program becomes a process when executed.

---

## What is a Process?

A process is a running instance of a program.

Unlike a program, a process consumes:

- CPU
- Memory (RAM)
- File Descriptors
- System Resources

---

# Program vs Process

| Program | Process |
|----------|----------|
| Stored on Disk | Running in Memory |
| Passive | Active |
| No CPU Usage | Uses CPU |
| No RAM Usage | Uses RAM |

---

# Linux Process Lifecycle

```text
Program
    │
    ▼
Execution
    │
    ▼
Process Created
    │
    ▼
Running
    │
    ▼
Waiting (Optional)
    │
    ▼
Running
    │
    ▼
Exit
```

---

# Parent & Child Process

Every process (except PID 1) is created by another process.

Example

```text
systemd (PID 1)
        │
        ▼
bash
        │
        ▼
python
```

---

# PID

PID = Process ID

Every running process has a unique Process ID.

Example

```bash
echo $$
```

Returns the PID of the current Bash shell.

---

# PPID

PPID = Parent Process ID

PPID identifies the process that created the current process.

---

# Process Monitoring Commands

## ps

Displays a snapshot of running processes.

---

## top

Displays processes in real time.

---

## htop

Interactive real-time process monitor.

Requires installation.

---

# ps Command Variations

| Command | Description | Scope |
|----------|-------------|--------|
| `ps` | Default output | Current Terminal |
| `ps -f` | Full format | Current Terminal |
| `ps -ef` | Full format + Every process | Entire System |
| `ps aux` | BSD-style full process listing | Entire System |

---

# ps Options Reference

| Option | Full Form | Meaning |
|---------|-----------|----------|
| *(none)* | Default | Current terminal processes |
| `-e` | Every Process | Show all processes |
| `-f` | Full Format | Detailed information |
| `a` | All Users | Show processes of all users |
| `u` | User Format | Display user-oriented output |
| `x` | No TTY | Include processes without a terminal |

---

# Process Monitoring Tools

| Tool | Snapshot | Real-Time | Interactive | Best Use |
|------|----------|-----------|-------------|-----------|
| `ps` | ✅ | ❌ | ❌ | Quick process inspection |
| `top` | ❌ | ✅ | Basic | Live CPU/RAM monitoring |
| `htop` | ❌ | ✅ | ✅ | Interactive process monitoring |

---

# Important Columns

| Column | Meaning |
|----------|----------|
| USER | Process owner |
| PID | Process ID |
| PPID | Parent Process ID |
| %CPU | CPU usage |
| %MEM | Memory usage |
| STAT | Process state |
| COMMAND | Running program |

---

# Commands Practiced

```bash
echo $$

ps

ps -f

ps -ef

ps aux

top

htop
```

---

# Key Differences

## ps vs top vs htop

| Feature | ps | top | htop |
|----------|----|-----|------|
| Snapshot | ✅ | ❌ | ❌ |
| Live Monitoring | ❌ | ✅ | ✅ |
| Interactive | ❌ | Basic | Full |
| Search | ❌ | Limited | Easy |
| Mouse Support | ❌ | ❌ | ✅ |

---

# DevOps Use Cases

- Monitor running applications
- Check CPU usage
- Check memory usage
- Identify long-running processes
- Inspect server processes
- Troubleshoot backend services

---

---

# Process Searching

Linux provides multiple ways to locate running processes.

Depending on the situation, you may need:

- Full process information
- Only Process ID (PID)
- Full command line
- Search by process name

---

# Process Searching Commands

```bash
ps aux | grep

pgrep

pidof
```

---

# Command Variations

## ps + grep

```bash
ps aux | grep bash

ps aux | grep nginx

ps aux | grep python
```

---

## pgrep

```bash
pgrep bash

pgrep -l bash

pgrep -a bash
```

---

## pidof

```bash
pidof bash

pidof nginx

pidof sshd
```

---

# pgrep Options Reference

| Option | Full Meaning | Purpose |
|---------|--------------|---------|
| *(none)* | Default | Display PID |
| `-l` | List Name | PID + Process Name |
| `-a` | All / Full Command | PID + Complete Command Line |

---

# Process Search Comparison

| Command | Returns | Best Use |
|----------|----------|----------|
| `ps aux \| grep` | Full process information | Detailed inspection |
| `pgrep` | PID | Quickly locate a process |
| `pidof` | PID | Find PID of a program |

---

# Commands Practiced

```bash
ps aux | grep bash

pgrep bash

pgrep -l bash

pgrep -a bash

pidof bash
```

---

# Process Control

Linux allows processes to be terminated using PID or Process Name.

---

# Commands

```bash
kill

pkill

killall
```

---

# Command Variations

## kill

```bash
kill PID

kill -9 PID
```

---

## pkill

```bash
pkill bash

pkill nginx

pkill sleep
```

---

## killall

```bash
killall bash

killall sleep

killall nginx
```

---

# kill Options

| Option | Full Meaning | Purpose |
|---------|--------------|---------|
| *(none)* | SIGTERM | Gracefully terminate |
| `-9` | SIGKILL | Force kill |

---

# Process Control Comparison

| Command | Uses | Input |
|----------|------|-------|
| `kill` | Single Process | PID |
| `pkill` | Process Name | Name / Pattern |
| `killall` | All Matching Processes | Process Name |

---

# Common Linux Signals

| Signal | Number | Description |
|----------|----------|-------------|
| SIGHUP | 1 | Hang Up / Reload |
| SIGINT | 2 | Interrupt (`Ctrl + C`) |
| SIGTERM | 15 | Graceful Termination (Default) |
| SIGKILL | 9 | Force Kill |

---

# Important Note

`kill` sends **SIGTERM (15)** by default.

`kill -9` sends **SIGKILL (9)** and immediately terminates the process.

---

# Commands Practiced

```bash
sleep 300 &

jobs

pgrep sleep

kill PID

pkill sleep

killall sleep
```

---

# Background & Foreground Jobs

Linux shells support Job Control.

A job can run in:

- Foreground
- Background
- Stopped State

---

# Job Control Commands

```bash
jobs

bg

fg
```

---

# Related Keyboard Shortcuts

| Shortcut | Action |
|-----------|--------|
| `Ctrl + Z` | Stop (Pause) Current Process |
| `Ctrl + C` | Interrupt / Terminate Current Process |

---

# Job Control Workflow

```text
Foreground

↓

Ctrl + Z

↓

Stopped

↓

bg

↓

Background

↓

fg

↓

Foreground

↓

Ctrl + C

↓

Exit
```

---

# Job Control Comparison

| Command | Purpose |
|----------|----------|
| `jobs` | List Background Jobs |
| `bg` | Continue Job in Background |
| `fg` | Bring Job to Foreground |

---

# Commands Practiced

```bash
sleep 300

Ctrl + Z

jobs

bg

jobs

fg

Ctrl + C
```

---

# DevOps Use Cases

- Find application PID
- Stop frozen processes
- Kill unnecessary background tasks
- Manage multiple running services
- Move jobs between foreground and background
- Troubleshoot production processes

---

---

# Persistent Processes

Normally, when a terminal session is closed, its child processes also terminate.

`nohup` (No Hang Up) allows a process to continue running even after the terminal or SSH session is closed.

---

# nohup

## Syntax

```bash
nohup COMMAND &
```

---

## Examples

```bash
nohup sleep 300 &

nohup python app.py &

nohup node server.js &
```

---

# nohup Workflow

```text
Normal Process

Terminal Closed

↓

Process Stops
```

```text
nohup Process

Terminal Closed

↓

Process Continues Running
```

---

# nohup Output

By default, nohup stores command output in:

```text
nohup.out
```

---

# Commands Practiced

```bash
nohup sleep 300 &

jobs

pgrep sleep

ls -l nohup.out
```

---

# Process Priority

Linux schedules CPU time using process priorities.

Priority is controlled using:

- nice
- renice

---

# nice

Starts a **new process** with a specified Nice Value.

## Syntax

```bash
nice -n VALUE COMMAND
```

Example

```bash
nice -n 10 sleep 300 &
```

---

# renice

Changes the priority of an **already running process**.

## Syntax

```bash
renice VALUE -p PID
```

Example

```bash
sudo renice 5 -p 1234
```

---

# nice vs renice

| Command | Purpose |
|----------|----------|
| `nice` | Start a new process with a custom priority |
| `renice` | Change the priority of a running process |

---

# Nice Value

| Nice Value | Priority |
|------------|----------|
| -20 | Highest Priority |
| 0 | Default |
| 19 | Lowest Priority |

> **Important:** A higher Nice value means a lower CPU priority.

---

# nice Options

| Option | Meaning |
|---------|----------|
| `-n` | Specify Nice Value |

---

# renice Options

| Option | Meaning |
|---------|----------|
| `-p` | Specify PID |

---

# Commands Practiced

```bash
nice -n 10 sleep 300 &

ps -o pid,ni,comm -p PID

renice VALUE -p PID
```

---

# ps Options Used in This Step

| Option | Meaning |
|---------|----------|
| `-e` | Every Process |
| `-f` | Full Format |
| `-o` | Custom Output Columns |
| `-p` | Select Process by PID |
| `a` | All Users |
| `u` | User Format |
| `x` | Include Processes Without TTY |

---

# pgrep Options

| Option | Meaning |
|---------|----------|
| `-l` | PID + Process Name |
| `-a` | PID + Full Command |

---

# kill Options

| Option | Meaning |
|---------|----------|
| *(default)* | SIGTERM (15) |
| `-9` | SIGKILL (Force Kill) |

---

# Complete Commands Practiced

## Process Inspection

```bash
echo $$

ps

ps -f

ps -ef

ps aux

top

htop
```

---

## Process Searching

```bash
ps aux | grep

pgrep

pidof
```

---

## Process Control

```bash
kill

kill -9

pkill

killall
```

---

## Job Control

```bash
jobs

bg

fg
```

Keyboard Shortcuts

```text
Ctrl + Z

Ctrl + C
```

---

## Persistent Process

```bash
nohup
```

---

## Process Priority

```bash
nice

renice
```

---

# Common Mistakes

- Killing the wrong PID
- Using `kill -9` unnecessarily
- Forgetting `&` for background execution
- Confusing `kill`, `pkill`, and `killall`
- Forgetting `Ctrl + Z` pauses a process instead of terminating it
- Trying to increase priority without root privileges
- Assuming `nohup` automatically runs a process in the background without `&`

---

# Best Practices

- Prefer `SIGTERM` before using `SIGKILL`
- Verify the PID before terminating a process
- Use `htop` for interactive monitoring
- Use `nohup` for long-running server tasks
- Use lower priorities for non-critical background tasks
- Avoid killing system processes unless necessary

---

# Practical DevOps Workflow

```text
Start Process
      │
      ▼
Monitor (ps / top / htop)
      │
      ▼
Find PID (pgrep / pidof)
      │
      ▼
Check Priority
      │
      ▼
Terminate if Required
      │
      ▼
Verify
```

---

# Real DevOps Use Cases

- Monitor backend services
- Identify high CPU usage
- Locate application processes
- Gracefully terminate applications
- Run long-running scripts after SSH disconnect
- Adjust CPU priority for background jobs
- Troubleshoot production servers

---

# Interview Questions

### Q1. What is a Process?

### Q2. What is PID?

### Q3. What is PPID?

### Q4. What is the difference between a Program and a Process?

### Q5. What is the difference between `ps`, `top`, and `htop`?

### Q6. What is the difference between `kill`, `pkill`, and `killall`?

### Q7. What is `nohup` used for?

### Q8. What is a Nice Value?

### Q9. What is the difference between `nice` and `renice`?

### Q10. Why should `kill -9` be used carefully?

---

# Quick Revision

```text
View Process
------------
ps
top
htop

Find Process
------------
pgrep
pidof

Terminate
----------
kill
pkill
killall

Jobs
----
jobs
bg
fg

Persistent
----------
nohup

Priority
--------
nice
renice
```

---

# Key Takeaways

- Every running application is a process.
- Every process has a unique PID.
- `ps` provides a snapshot, while `top` and `htop` provide live monitoring.
- `pgrep` and `pidof` quickly locate running processes.
- `kill`, `pkill`, and `killall` terminate processes using different inputs.
- `nohup` keeps a process running after terminal logout.
- `nice` starts a process with a custom priority.
- `renice` changes the priority of a running process.
- Always verify before terminating production processes.

---

# Completion Status

| Module | Status |
|----------|--------|
| Process Basics | ✅ |
| Process Monitoring | ✅ |
| Process Searching | ✅ |
| Process Control | ✅ |
| Jobs | ✅ |
| nohup | ✅ |
| nice / renice | ✅ |
| Practical Lab | ✅ |
| Interview Revision | ✅ |

---

# Next Step

➡️ **Step 06 — Linux Services & systemd**

Topics include:

- Services
- Daemons
- systemd
- systemctl
- service
- enable
- disable
- start
- stop
- restart
- status
- journalctl
- Boot targets

