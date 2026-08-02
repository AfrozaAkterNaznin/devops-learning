# Step 06 — Linux Services & Systemctl

---

# Overview

Linux systems rely on background services to provide essential functionality such as networking, scheduling, logging, remote access, databases, and web servers.

In modern Linux distributions, these services are managed by **systemd**, which is the default initialization system and service manager.

This chapter focuses on understanding Linux services, inspecting their status, managing their lifecycle, analyzing logs, and controlling their startup behavior. These are among the most frequently used skills in Linux Administration, DevOps, Cloud Engineering, and Site Reliability Engineering (SRE).

---

# Learning Objectives

After completing this step, you will be able to:

- Understand Linux Services
- Understand Daemons
- Understand systemd Architecture
- Understand systemctl
- View Running Services
- Inspect Service Status
- Start and Stop Services
- Restart and Reload Services
- Enable and Disable Services
- Check Boot-Time Service Status
- View Service Logs
- Reload systemd Configuration
- Troubleshoot Linux Services

---

# What is a Linux Service?

A **Linux Service** is a background program that continuously performs a specific task or provides functionality to the operating system or applications.

Unlike normal user programs, services usually start automatically during system boot and continue running until the system shuts down.

Examples include:

- SSH Server
- Cron Scheduler
- Docker
- Nginx
- Apache
- MySQL
- PostgreSQL
- NetworkManager

---

# What is a Daemon?

A **Daemon** is a background process that runs without direct user interaction.

Most Linux services are implemented as daemons.

Examples:

| Daemon | Purpose |
|---------|---------|
| sshd | Secure Shell Server |
| crond / cron | Task Scheduler |
| systemd | System & Service Manager |
| rsyslogd | System Logging |
| dockerd | Docker Engine |

> Traditionally, many daemon names end with the letter **d**, meaning **daemon**.

---

# Service vs Process vs Daemon

| Component | Description |
|-----------|-------------|
| Program | Executable file stored on disk |
| Process | A running instance of a program |
| Service | A long-running background process providing functionality |
| Daemon | Unix/Linux background service process |

---

# What is Cron?

**Cron** is Linux's built-in **Task Scheduler**.

It automatically executes commands or scripts at scheduled times.

Common Uses:

- Automatic Backups
- Log Cleanup
- Scheduled Reports
- Automation Scripts
- Maintenance Tasks

---

# What is systemd?

**systemd** is the default **Init System** and **Service Manager** in modern Linux distributions.

Its responsibilities include:

- Booting the system
- Starting services
- Stopping services
- Monitoring services
- Managing dependencies
- Collecting logs
- Handling system targets

---

# Why systemd is Important

Without systemd, administrators would have to manually start and manage every service.

systemd automates:

- Service startup
- Dependency management
- Failure recovery
- Boot process
- Logging integration

---

# systemd Architecture

```text
                Linux Kernel
                      │
                      ▼
                systemd (PID 1)
                      │
      ┌───────────────┼────────────────┐
      │               │                │
      ▼               ▼                ▼
   Services        Sockets         Timers
      │
      ▼
 systemctl Commands
```

---

# PID 1

After the Linux kernel finishes booting, the very first userspace process started is:

```text
systemd
```

Its Process ID is always:

```text
PID 1
```

Almost every service in the system is managed directly or indirectly by PID 1.

---

# What is systemctl?

`systemctl` is the command-line interface used to communicate with **systemd**.

It allows administrators to:

- Inspect services
- Start services
- Stop services
- Restart services
- Reload configurations
- Enable services at boot
- Disable services
- Check service status
- View service information

---

# systemctl Workflow

```text
User
 │
 ▼
systemctl
 │
 ▼
systemd (PID 1)
 │
 ▼
Linux Services
```

---

# Service Management Workflow

```text
System Boot
      │
      ▼
systemd (PID 1)
      │
      ▼
Starts Required Services
      │
      ▼
Administrator
      │
      ▼
systemctl Commands
      │
 ┌────┼─────┬──────┬───────┬─────────┐
 ▼    ▼     ▼      ▼       ▼
status start stop restart enable disable
```

---

# Common systemctl Commands

| Command | Purpose |
|----------|---------|
| `systemctl` | Display loaded units |
| `systemctl --version` | Show installed systemd version |
| `systemctl status` | Show service status |
| `systemctl list-units` | List loaded units |
| `systemctl list-unit-files` | List installed unit files |
| `systemctl start` | Start a service |
| `systemctl stop` | Stop a service |
| `systemctl restart` | Restart a service |
| `systemctl reload` | Reload configuration (supported services only) |
| `systemctl enable` | Enable service at boot |
| `systemctl disable` | Disable service at boot |
| `systemctl is-enabled` | Check boot-time status |
| `journalctl` | View system logs |
| `systemctl daemon-reload` | Reload unit files |
| `systemctl daemon-reexec` | Re-execute systemd |

---

# systemctl Subcommands

| Subcommand | Description |
|------------|-------------|
| status | Display current service status |
| start | Start service immediately |
| stop | Stop service immediately |
| restart | Restart service |
| reload | Reload configuration without stopping (if supported) |
| enable | Enable automatic startup during boot |
| disable | Disable automatic startup during boot |
| is-enabled | Check boot startup state |
| daemon-reload | Reload modified unit files |
| daemon-reexec | Re-execute systemd process |

---

# Difference — Service vs systemctl

| Command | Purpose | Recommended |
|----------|---------|-------------|
| `service` | Legacy compatibility command | No |
| `systemctl` | Modern systemd management tool | Yes |

> Modern Linux distributions should use **systemctl** for service management.

---

# Difference — Start vs Stop vs Enable vs Disable

| Command | Immediate Effect | Boot-Time Effect |
|----------|------------------|------------------|
| start | Starts the service immediately | No |
| stop | Stops the service immediately | No |
| enable | No immediate change | Starts automatically after reboot |
| disable | No immediate change | Does not start automatically after reboot |

> **Important:** `start` and `enable` are different operations.

---

# Difference — Restart vs Reload

| Command | Stops Service | Reads Updated Configuration | Typical Usage |
|----------|---------------|----------------------------|---------------|
| restart | Yes | Yes | Configuration changes or service recovery |
| reload | No | Yes | Reload configuration without downtime |

> Some services (such as `cron`) do **not** support `reload`.

---

# Difference — Active vs Enabled

| State | Meaning |
|--------|----------|
| Active | Current runtime status |
| Enabled | Boot-time startup configuration |

A service can be:

- Active + Enabled
- Active + Disabled
- Inactive + Enabled
- Inactive + Disabled

These are independent states.

---

# Service Troubleshooting Workflow

```text
Application Problem
        │
        ▼
Check Service Status
        │
        ▼
systemctl status SERVICE
        │
        ▼
Check Logs
        │
        ▼
journalctl -u SERVICE
        │
        ▼
Identify Error
        │
        ▼
Fix Configuration
        │
        ▼
Restart Service
```

---

# Understanding systemctl Output

During this step, several `systemctl` commands were executed to inspect Linux services.

Understanding the output is essential for troubleshooting Linux systems.

---

# systemctl status Output

Example:

```bash
systemctl status cron
```

Typical Output:

```text
Loaded: loaded (/usr/lib/systemd/system/cron.service; enabled)
Active: active (running)
Main PID: 1310
Tasks: 1
Memory: 528K
CPU: 272ms
Docs: man:cron(8)
```

---

# systemctl status Fields

| Field | Meaning |
|--------|---------|
| Loaded | Whether the service configuration file has been loaded |
| Active | Current runtime state |
| Main PID | Main process ID of the service |
| Tasks | Number of running tasks |
| Memory | Current memory usage |
| CPU | CPU time consumed |
| Docs | Related manual page |

---

# Active States

| State | Meaning |
|--------|---------|
| active (running) | Service is running normally |
| inactive (dead) | Service is stopped |
| failed | Service crashed or failed |
| activating | Service is starting |
| deactivating | Service is stopping |

---

# systemctl list-units

Command:

```bash
systemctl list-units --type=service
```

Purpose:

Display all currently loaded services.

---

# list-units Columns

| Column | Meaning |
|---------|---------|
| UNIT | Service name |
| LOAD | Unit file successfully loaded |
| ACTIVE | High-level runtime state |
| SUB | Detailed runtime state |
| DESCRIPTION | Short description of the service |

---

# systemctl list-unit-files

Command

```bash
systemctl list-unit-files --type=service
```

Purpose

Displays every installed service and its boot-time configuration.

Unlike `list-units`, this command also shows services that are currently not running.

---

# Unit File States

| State | Meaning |
|--------|---------|
| enabled | Starts automatically during boot |
| disabled | Does not start automatically |
| static | Started only as a dependency |
| masked | Cannot be started manually |
| alias | Alternative service name |

---

# journalctl

`journalctl` is the log viewer for **systemd**.

It is one of the most important troubleshooting tools for Linux administrators and DevOps engineers.

---

# Common journalctl Commands

| Command | Purpose |
|----------|---------|
| `journalctl` | Show complete system log |
| `journalctl -u cron` | Show logs for a specific service |
| `journalctl -u cron -n 10` | Show last 10 log entries |
| `journalctl -u cron --since "10 minutes ago"` | Show recent logs |
| `journalctl -u cron --no-pager` | Disable pager output |
| `journalctl -xe` | Show recent important system logs |

---

# journalctl Workflow

```text
Service Problem
      │
      ▼
systemctl status SERVICE
      │
      ▼
journalctl -u SERVICE
      │
      ▼
Read Error Messages
      │
      ▼
Fix Configuration
      │
      ▼
Restart Service
```

---

# Pager

Large outputs from commands like:

- journalctl
- systemctl status
- systemctl list-units
- man

are usually displayed using a **pager** (`less`).

A pager allows scrolling through long output page by page.

---

# Pager Navigation Keys

| Key | Action |
|-----|--------|
| ↑ ↓ | Scroll line by line |
| Space | Next page |
| b | Previous page |
| /word | Search |
| n | Next search match |
| Shift + G | Go to last line |
| g | Go to first line |
| q | Quit pager |

---

# Exiting Different Situations

| Situation | Key |
|-----------|-----|
| Viewing pager (`less`, `journalctl`, `systemctl`, `man`) | `q` |
| Stop a running command | `Ctrl + C` |
| Suspend a running command | `Ctrl + Z` |

> **Note:** Never use **Ctrl + Z** to exit a pager. Use **q** instead.

---

# daemon-reload

Command

```bash
sudo systemctl daemon-reload
```

Purpose

Reload modified or newly added systemd unit files without restarting systemd.

Typical Usage:

- New `.service` file added
- Existing service file modified

---

# daemon-reexec

Command

```bash
sudo systemctl daemon-reexec
```

Purpose

Re-executes the running `systemd` process while preserving its current state.

This command is rarely needed in daily administration.

---

# daemon-reload vs daemon-reexec

| Command | Purpose | Common Usage |
|----------|---------|--------------|
| daemon-reload | Reload unit configuration files | Frequently |
| daemon-reexec | Restart the systemd manager process internally | Rarely |

---

# Flags Reference

| Flag | Full Form | Purpose |
|------|-----------|---------|
| `-u` | Unit | Specify a service/unit |
| `-n` | Number | Show last N log entries |
| `-x` | Explain | Show additional explanations |
| `-e` | End | Jump near the end of logs |
| `--since` | Since | Show logs after specified time |
| `--no-pager` | No Pager | Disable pager output |
| `--type=service` | Type Filter | Display only service units |
| `--state=running` | State Filter | Display only running services |

---

# Real-World DevOps Workflow

```text
Website is Down
        │
        ▼
systemctl status nginx
        │
        ▼
journalctl -u nginx -n 50
        │
        ▼
Identify Error
        │
        ▼
Fix Configuration
        │
        ▼
systemctl restart nginx
        │
        ▼
Verify Service Status
```

---

# Common Mistakes

| Mistake | Correct Approach |
|----------|------------------|
| Using `enable` instead of `start` | `enable` only affects boot-time startup |
| Expecting `reload` to work on every service | Not all services support reload |
| Using `Ctrl + Z` to leave pager | Use `q` |
| Ignoring logs during troubleshooting | Always inspect `journalctl` logs first |

---


# Best Practices

- Always check the service status before troubleshooting.
- Read service logs before restarting a service.
- Use `systemctl` instead of the legacy `service` command whenever possible.
- Use `restart` only when necessary.
- Use `reload` if the service supports configuration reload without downtime.
- Keep important production services enabled during boot.
- Verify service status after every configuration change.
- Learn to identify failed services using `systemctl status` and `journalctl`.

---

# Service Management Cheat Sheet

| Task | Command |
|------|---------|
| Show systemd version | `systemctl --version` |
| Show service status | `systemctl status SERVICE` |
| List loaded services | `systemctl list-units --type=service` |
| List installed service files | `systemctl list-unit-files --type=service` |
| Start service | `sudo systemctl start SERVICE` |
| Stop service | `sudo systemctl stop SERVICE` |
| Restart service | `sudo systemctl restart SERVICE` |
| Reload service | `sudo systemctl reload SERVICE` |
| Enable at boot | `sudo systemctl enable SERVICE` |
| Disable at boot | `sudo systemctl disable SERVICE` |
| Check boot status | `systemctl is-enabled SERVICE` |
| View logs | `journalctl -u SERVICE` |
| Last 10 logs | `journalctl -u SERVICE -n 10` |
| Recent logs | `journalctl -u SERVICE --since "10 minutes ago"` |
| Reload unit files | `sudo systemctl daemon-reload` |
| Re-execute systemd | `sudo systemctl daemon-reexec` |

---

# Complete Command Reference

## General

```bash
systemctl

systemctl --version
```

---

## Service Information

```bash
systemctl status cron

systemctl status ssh

systemctl list-units --type=service

systemctl list-units --type=service --state=running

systemctl list-unit-files --type=service
```

---

## Service Control

```bash
sudo systemctl start cron

sudo systemctl stop cron

sudo systemctl restart cron

sudo systemctl reload cron
```

---

## Boot Management

```bash
systemctl is-enabled cron

sudo systemctl enable cron

sudo systemctl disable cron
```

---

## Logs

```bash
journalctl

journalctl -u cron

journalctl -u cron -n 10

journalctl -u cron --since "10 minutes ago"

journalctl -u cron --no-pager

journalctl -xe
```

---

## systemd Maintenance

```bash
sudo systemctl daemon-reload

sudo systemctl daemon-reexec
```

---

# Command Option Reference

| Command | Option | Meaning |
|----------|--------|---------|
| systemctl | `status` | Show service status |
| systemctl | `start` | Start service |
| systemctl | `stop` | Stop service |
| systemctl | `restart` | Restart service |
| systemctl | `reload` | Reload configuration |
| systemctl | `enable` | Enable auto-start |
| systemctl | `disable` | Disable auto-start |
| systemctl | `is-enabled` | Check boot status |
| systemctl | `list-units` | List loaded units |
| systemctl | `list-unit-files` | List installed unit files |
| journalctl | `-u` | Specify unit/service |
| journalctl | `-n` | Last N log entries |
| journalctl | `-x` | Show explanations |
| journalctl | `-e` | Jump to end of logs |
| journalctl | `--since` | Show logs after specified time |
| journalctl | `--no-pager` | Disable pager |
| list-units | `--type=service` | Show only services |
| list-units | `--state=running` | Show only running services |

---

# Linux Service Troubleshooting Checklist

```text
Problem Reported
       │
       ▼
Check Service Status
       │
       ▼
systemctl status SERVICE
       │
       ▼
Read Service Logs
       │
       ▼
journalctl -u SERVICE
       │
       ▼
Identify Root Cause
       │
       ▼
Modify Configuration
       │
       ▼
daemon-reload (if unit file changed)
       │
       ▼
Restart Service
       │
       ▼
Verify Service Status
       │
       ▼
Confirm Service is Running
```

---

# Real-World Examples

| Service | Typical Purpose |
|----------|-----------------|
| ssh | Remote server access |
| cron | Scheduled task execution |
| nginx | Web server / Reverse proxy |
| apache2 | Web server |
| docker | Container engine |
| mysql | Relational database |
| postgresql | Relational database |
| NetworkManager | Network management |

---

# Interview Questions

### What is systemd?

systemd is the default init system and service manager used by modern Linux distributions.

---

### What is systemctl?

systemctl is the command-line utility used to manage services and interact with systemd.

---

### Difference between start and enable?

- **start** starts the service immediately.
- **enable** configures the service to start automatically during boot.

---

### Difference between restart and reload?

- **restart** stops and starts the service again.
- **reload** reloads configuration without stopping the service (if supported).

---

### Difference between Active and Enabled?

- **Active** indicates the current runtime state.
- **Enabled** indicates whether the service starts automatically during boot.

---

### What is journalctl?

journalctl is the log viewer for systemd-managed services and the system journal.

---

### What is daemon-reload?

Reloads modified or newly added systemd unit files.

---

### What is daemon-reexec?

Re-executes the running systemd process while preserving its state.

---

# Key Takeaways

- Linux services are managed by **systemd**.
- `systemctl` is the primary tool for service management.
- Always check service status before making changes.
- Use `journalctl` for troubleshooting.
- Understand the difference between runtime state and boot-time state.
- Not every service supports `reload`.
- Use `daemon-reload` after modifying service unit files.
- Verify services after every configuration change.

---

# Summary

In this step, the following concepts were covered:

- Linux Services
- Daemons
- systemd
- systemctl
- Service Status
- Running Services
- Installed Services
- Start / Stop
- Restart / Reload
- Enable / Disable
- Active vs Enabled
- journalctl
- Pager Navigation
- daemon-reload
- daemon-reexec
- Service Troubleshooting Workflow

This chapter provides the foundational knowledge required to manage Linux services efficiently in System Administration, DevOps, Cloud Engineering, Docker, Kubernetes, CI/CD, and Production Server environments.

---
