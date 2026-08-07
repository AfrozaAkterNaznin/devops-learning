# Step 14 — Cron & Scheduling

---

# Objective

Learn how to automate Linux tasks by scheduling commands and Bash scripts using Cron. This step focuses on creating, managing, and troubleshooting scheduled jobs commonly used in Linux administration and DevOps.

---

# Learning Outcomes

After completing this step, I can:

- Understand the purpose of Cron.
- Create and manage user Cron jobs.
- Read and write Cron expressions.
- Schedule Bash scripts automatically.
- Redirect command output and errors.
- Troubleshoot Cron jobs.
- Build basic Linux task automation.

---

# Cron Workflow

```text
User
 │
 ▼
Create Bash Script
 │
 ▼
Create Cron Job
 │
 ▼
Cron Service
 │
 ▼
Scheduled Time
 │
 ▼
Execute Script
 │
 ▼
Generate Output / Log
```

---

# Cron Architecture

```text
User
 │
 ▼
crontab
 │
 ▼
Cron Daemon (cron)
 │
 ▼
Scheduled Job
 │
 ▼
Bash Script / Linux Command
 │
 ▼
System Resources
 │
 ▼
Log File
```

---

# What is Cron?

Cron is the Linux scheduling service responsible for executing commands or scripts automatically at specified dates and times.

---

# What is Crontab?

Crontab (Cron Table) is the configuration file used to define scheduled jobs for a user.

Each line in a crontab file represents one scheduled task.

---

# Cron Expression Format

```text
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of Week (0-7)
│ │ │ └──── Month (1-12)
│ │ └────── Day of Month (1-31)
│ └──────── Hour (0-23)
└────────── Minute (0-59)
```

---

# Common Cron Expressions

| Expression | Meaning |
|------------|---------|
| `* * * * *` | Every minute |
| `*/5 * * * *` | Every 5 minutes |
| `0 * * * *` | Every hour |
| `0 2 * * *` | Every day at 2:00 AM |
| `0 9 * * 1` | Every Monday at 9:00 AM |

---

# Weekday Reference

| Value | Day |
|------:|-----|
| 0 or 7 | Sunday |
| 1 | Monday |
| 2 | Tuesday |
| 3 | Wednesday |
| 4 | Thursday |
| 5 | Friday |
| 6 | Saturday |

---

# Cron vs Manual Execution

| Cron | Manual Execution |
|------|------------------|
| Runs automatically | Started by the user |
| Executes at scheduled times | Executes immediately |
| Background execution | Interactive execution |
| Suitable for automation | Suitable for manual tasks |

---

# User Crontab vs System Crontab

| User Crontab | System Crontab |
|--------------|----------------|
| Managed using `crontab` | Managed in `/etc/crontab` |
| User-specific | System-wide |
| No user field required | User field required |
| Suitable for personal automation | Suitable for administrative tasks |

---

# Cron Job Lifecycle

```text
Create Job
      │
      ▼
Install using crontab
      │
      ▼
Cron waits for scheduled time
      │
      ▼
Execute command
      │
      ▼
Write output or log
```

---

# Important Files

| File | Purpose |
|------|---------|
| `/etc/crontab` | System-wide Cron configuration |
| `/var/spool/cron/crontabs/` | User Cron tables |
| `~/.profile` | User login environment |
| `~/.bashrc` | Bash configuration reference |

---

# Important Directories

| Directory | Purpose |
|-----------|---------|
| `~/Linux-Practice/bash-scripting` | Bash scripts used for scheduling |
| `~/Projects/devops-learning/Step-14-Cron-Scheduling` | Documentation |

# Commands Used

## Cron Service Management

| Command | Description |
|----------|-------------|
| `systemctl status cron` | Display the current status of the Cron service. |
| `systemctl is-enabled cron` | Check whether the Cron service starts automatically during boot. |

---

## User Crontab Management

| Command | Description |
|----------|-------------|
| `crontab -l` | Display the current user's scheduled jobs. |
| `crontab -e` | Create or edit the current user's Cron jobs. |
| `crontab -r` | Remove all Cron jobs for the current user. |

---

# Cron Expressions

| Expression | Description |
|------------|-------------|
| `* * * * *` | Every minute |
| `*/2 * * * *` | Every 2 minutes |
| `*/5 * * * *` | Every 5 minutes |
| `0 * * * *` | Every hour |
| `0 2 * * *` | Every day at 2:00 AM |
| `0 9 * * 1` | Every Monday at 9:00 AM |

---

# Scheduled Jobs Used

## Cron Test

```cron
* * * * * echo "Cron is working: $(date)" >> /home/afroza/Linux-Practice/bash-scripting/cron-test.log
```

---

## System Information

```cron
* * * * * /home/afroza/Linux-Practice/bash-scripting/system-info.sh >> /home/afroza/Linux-Practice/bash-scripting/system-info.log 2>&1
```

---

## Backup Script

```cron
*/2 * * * * /home/afroza/Linux-Practice/bash-scripting/backup.sh >> /home/afroza/Linux-Practice/bash-scripting/backup.log 2>&1
```

---

## Disk Usage Monitoring

```cron
*/2 * * * * /home/afroza/Linux-Practice/bash-scripting/disk-check.sh >> /home/afroza/Linux-Practice/bash-scripting/disk.log 2>&1
```

---

# Output Redirection

| Operator | Description |
|----------|-------------|
| `>` | Write output to a file (overwrite existing content). |
| `>>` | Append output to a file. |
| `2>` | Redirect standard error to a file. |
| `2>&1` | Redirect both standard output and standard error to the same destination. |

---

# Standard Streams

| Stream | Number | Purpose |
|---------|-------:|---------|
| Standard Input | `0` | Keyboard or input source |
| Standard Output | `1` | Normal program output |
| Standard Error | `2` | Error messages |

---

# Troubleshooting Commands

| Command | Purpose |
|----------|---------|
| `journalctl -u cron` | View Cron service logs. |
| `systemctl status cron` | Verify that the Cron service is running. |
| `crontab -l` | Verify installed Cron jobs. |
| `ls -l script.sh` | Check execute permission. |
| `chmod +x script.sh` | Grant execute permission. |
| `cat log_file` | Verify script execution results. |

---

# Common Mistakes

| Mistake | Explanation |
|----------|-------------|
| Missing execute permission | Cron cannot execute the script directly. |
| Using relative paths | Cron may execute from a different working directory. |
| Forgetting log redirection | Errors become difficult to identify. |
| Assuming Cron uses Bash | Cron commonly executes commands using `/bin/sh` unless a different shell is specified. |
| Checking logs immediately | The scheduled execution time may not have been reached yet. |

---

# Best Practices

| Practice | Reason |
|----------|--------|
| Use absolute paths | Avoid path resolution issues. |
| Redirect output to log files | Simplifies debugging. |
| Test scripts manually before scheduling | Verify expected behavior. |
| Verify execute permission | Required for direct script execution. |
| Check Cron logs when troubleshooting | Identify scheduling or execution problems quickly. |

---

# Real-World Usage

| Task | Practical Example |
|------|-------------------|
| Backup Automation | Schedule daily backups |
| System Monitoring | Collect system information periodically |
| Disk Monitoring | Monitor available storage space |
| Log Generation | Store scheduled task output |
| Maintenance Tasks | Execute recurring administrative scripts |

---

# Interview Questions

| Question | Short Answer |
|----------|--------------|
| What is Cron? | A Linux service used to schedule commands and scripts. |
| What is Crontab? | A table containing scheduled jobs for a user. |
| What does `*/5 * * * *` mean? | Execute every 5 minutes. |
| Difference between `>` and `>>`? | `>` overwrites; `>>` appends. |
| What is `2>&1`? | Redirects standard error to standard output. |
| Why use absolute paths in Cron jobs? | Cron may use a different working directory. |
| Why did "Permission denied" occur during the lab? | The script did not have execute permission. |

# Skills Acquired

After completing this step, the following Cron and scheduling skills were practiced and verified:

- Verified the Cron service status.
- Checked whether the Cron service starts automatically at boot.
- Created user Cron jobs.
- Edited and removed Cron jobs.
- Read and understood Cron expressions.
- Scheduled Bash scripts for automatic execution.
- Redirected command output and errors to log files.
- Used Cron logs for troubleshooting.
- Resolved execution permission issues.
- Automated Linux administration tasks using Cron.

---

# Real Lab Summary

| Item | Observation |
|------|-------------|
| Operating System | Ubuntu 26.04 LTS (Virtual Machine) |
| Scheduler | Cron |
| Cron Service | Active (running) |
| Startup Status | Enabled |
| User Crontab | Created, modified, listed, and removed |
| Practice Directory | `~/Linux-Practice/bash-scripting` |
| Documentation Directory | `~/Projects/devops-learning/Step-14-Cron-Scheduling` |
| Log Files | `cron-test.log`, `system-info.log`, `backup.log`, `disk.log` |

---

# Scheduled Jobs Practiced

| Scheduled Task | Purpose |
|----------------|---------|
| Cron Test | Verify Cron execution |
| System Information | Execute `system-info.sh` automatically |
| Backup Automation | Execute `backup.sh` automatically |
| Disk Monitoring | Execute `disk-check.sh` automatically |

---

# Commands Practiced

| Category | Commands |
|----------|----------|
| Service Management | `systemctl status`, `systemctl is-enabled` |
| Cron Management | `crontab -l`, `crontab -e`, `crontab -r` |
| Log Verification | `cat`, `journalctl` |
| File Permissions | `chmod +x`, `ls -l` |
| File Operations | `echo`, `cp` |

---

# Log Files Used

| File | Purpose |
|------|---------|
| `cron-test.log` | Verify Cron execution |
| `system-info.log` | Store system information output |
| `backup.log` | Store backup script output |
| `disk.log` | Store disk monitoring output |

---

# Services Used

| Service | Purpose |
|----------|---------|
| `cron.service` | Execute scheduled jobs automatically |

---

# Important Files

| File | Purpose |
|------|---------|
| `/etc/crontab` | System-wide Cron configuration |
| User Crontab | User-specific scheduled jobs |
| `system-info.sh` | Scheduled system information script |
| `backup.sh` | Scheduled backup script |
| `disk-check.sh` | Scheduled disk monitoring script |

---

# Folder Structure

```text
Step-14-Cron-Scheduling/
└── README.md
```

---

# Step Summary

This step focused on Linux task automation using Cron. Practical exercises included creating and managing user Cron jobs, writing Cron expressions, scheduling Bash scripts, redirecting output to log files, troubleshooting execution problems, and resolving permission-related issues. These skills provide the automation foundation required for system administration, CI/CD pipelines, scheduled maintenance, and production DevOps workflows.

---

