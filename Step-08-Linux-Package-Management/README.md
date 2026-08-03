# Step 08 — Linux Package Management

---

# Overview

Package Management is one of the most important skills for every Linux Administrator, Backend Developer, and DevOps Engineer.

Unlike Windows, where software is usually installed using executable installers (.exe), Linux distributions use package managers to install, update, remove, and maintain software.

Ubuntu uses the **APT (Advanced Package Tool)** package manager to manage software packages from official repositories.

This chapter focuses on package fundamentals, repositories, package search, software information, package updates, upgrades, installation, removal, dependencies, package database inspection, and package management best practices.

---

# Learning Objectives

After completing this step, you will be able to:

- Understand Linux Package Management
- Understand Packages
- Understand Repositories
- Understand Dependencies
- Search Packages
- View Package Information
- Update Repository Index
- Upgrade Installed Packages
- Install Software
- Remove Packages
- Clean Package Cache
- Inspect Package Dependencies
- Understand dpkg
- Follow Professional Package Management Workflow

---

# What is a Package?

A package is a compressed software bundle that contains everything required to install and run an application.

A package usually contains:

- Executable files
- Libraries
- Configuration files
- Documentation
- Metadata
- Dependency information

---

# What is APT?

APT (Advanced Package Tool) is Ubuntu's package manager.

APT is responsible for:

- Searching packages
- Installing software
- Updating repositories
- Upgrading packages
- Removing packages
- Managing dependencies

---

# Package Management Workflow

```text
Repository
      │
      ▼
apt
      │
      ▼
Download Package
      │
      ▼
Check Dependencies
      │
      ▼
Install Dependencies
      │
      ▼
Install Package
      │
      ▼
Ready to Use
```

---

# Core Components

| Component | Purpose |
|-----------|----------|
| Package | Software bundle |
| Repository | Storage location of packages |
| Dependency | Required package for another package |
| APT | Package manager |
| dpkg | Low-level package database |

---

# Package vs Repository vs Dependency

| Component | Description |
|-----------|-------------|
| Package | Software to install |
| Repository | Online package storage |
| Dependency | Required software package |
| Package Manager | Tool used to manage packages |

---

# Package Lifecycle

```text
Repository
      │
      ▼
Search
      │
      ▼
Install
      │
      ▼
Use
      │
      ▼
Update
      │
      ▼
Upgrade
      │
      ▼
Remove
```

---

# Commands Learned

```bash
apt

apt --version

which apt

apt search nginx

apt show nginx

apt list --installed | head -20

sudo apt update

apt list --upgradable

sudo apt upgrade

sudo apt full-upgrade
```

---

# Command Summary

| Command | Purpose |
|----------|----------|
| apt | Show available APT commands |
| apt --version | Display installed APT version |
| which apt | Locate APT executable |
| apt search | Search repository packages |
| apt show | Show package information |
| apt list --installed | List installed packages |
| apt update | Refresh package index |
| apt list --upgradable | Show available upgrades |
| apt upgrade | Safe package upgrade |
| apt full-upgrade | Complete system upgrade |

---

# Command Differences

| Command | Purpose | Use Case |
|----------|----------|----------|
| apt search | Search packages | Find software |
| apt show | Show details | View package information |
| apt list --installed | Installed packages | Verify installation |
| apt update | Refresh repository index | Before upgrade/install |
| apt upgrade | Upgrade packages | Safe update |
| apt full-upgrade | Complete upgrade | Major system upgrade |

---

# Flags Used

| Command | Option | Meaning |
|----------|---------|----------|
| apt | --version | Show version |
| apt | search | Search packages |
| apt | show | Display package information |
| apt | list --installed | List installed packages |
| head | -20 | Display first 20 lines |

---

# Repository Update Workflow

```text
Ubuntu Repository
        │
        ▼
sudo apt update
        │
        ▼
Latest Package Index
        │
        ▼
apt list --upgradable
        │
        ▼
Available Package Updates
```

---

# Update vs Upgrade

| Command | Action |
|----------|---------|
| apt update | Downloads the latest package index |
| apt upgrade | Upgrades installed packages |
| apt full-upgrade | Upgrades packages even if installation/removal is required |

---

# Understanding Phased Updates

Sometimes Ubuntu displays:

```text
Not upgrading yet due to phasing
```

This is **not an error**.

Ubuntu gradually releases updates to users in phases.

Benefits:

- Stable rollout
- Early bug detection
- Reduced upgrade risk

---

# Best Practices

- Run `sudo apt update` before installing software.
- Review available upgrades using `apt list --upgradable`.
- Use `apt upgrade` for normal updates.
- Use `apt full-upgrade` only when necessary.
- Prefer official Ubuntu repositories whenever possible.

# Package Installation

Linux packages are installed using the APT package manager.

During installation, APT automatically:

- Downloads the package
- Resolves dependencies
- Installs required libraries
- Configures the software
- Registers the package in the package database

---

# Installation Workflow

```text
Repository
      │
      ▼
apt install
      │
      ▼
Download Package
      │
      ▼
Resolve Dependencies
      │
      ▼
Install Package
      │
      ▼
Verify Installation
```

---

# Commands Learned

```bash
sudo apt install tree

tree --version

which tree
```

---

# Command Summary

| Command | Purpose |
|----------|----------|
| sudo apt install tree | Install the package |
| tree --version | Verify installation |
| which tree | Show executable location |

---

# Package Removal

APT provides multiple ways to remove software.

Each command serves a different purpose.

---

# Commands Learned

```bash
sudo apt remove tree

sudo apt purge tree

sudo apt autoremove
```

---

# Package Removal Workflow

```text
Installed Package
        │
        ▼
apt remove
        │
        ▼
Package Removed
        │
        ▼
apt purge
        │
        ▼
Configuration Removed
        │
        ▼
apt autoremove
        │
        ▼
Unused Dependencies Removed
```

---

# remove vs purge vs autoremove

| Command | Removes Package | Removes Config | Removes Unused Dependencies |
|----------|-----------------|----------------|-----------------------------|
| apt remove | ✅ | ❌ | ❌ |
| apt purge | ✅ | ✅ | ❌ |
| apt autoremove | ❌ | ❌ | ✅ |

---

# Command Summary

| Command | Purpose |
|----------|----------|
| apt remove | Remove installed package |
| apt purge | Remove package and configuration files |
| apt autoremove | Remove unused dependency packages |

---

# Package Cache

APT stores downloaded package files in a local cache.

This cache helps avoid downloading the same package repeatedly.

Over time, the cache may consume unnecessary disk space.

---

# Commands Learned

```bash
sudo apt autoclean

sudo apt clean

ls /var/cache/apt/archives
```

---

# Cache Cleanup Workflow

```text
Package Download
        │
        ▼
APT Cache
        │
   ┌────┴────┐
   ▼         ▼
autoclean   clean
   │         │
   ▼         ▼
Old Cache   All Cache
Removed     Removed
```

---

# clean vs autoclean

| Command | Purpose |
|----------|----------|
| apt autoclean | Remove obsolete cached packages only |
| apt clean | Remove all downloaded package cache |

---

# Why are `lock` and `partial` Still Present?

After running:

```bash
sudo apt clean
```

you may still see:

```text
lock
partial/
```

This is expected behavior.

These are **not package cache files**.

---

# APT Cache Directory

```text
/var/cache/apt/archives/
├── lock
└── partial/
```

---

# Meaning

| Item | Purpose |
|------|----------|
| lock | Prevents multiple APT operations from running simultaneously |
| partial/ | Temporary download directory used while packages are downloading |

---

# What Does `apt clean` Remove?

Before cleaning:

```text
archives/
├── curl.deb
├── nginx.deb
├── tree.deb
├── lock
└── partial/
```

After cleaning:

```text
archives/
├── lock
└── partial/
```

Only downloaded package cache (`*.deb`) files are removed.

---

# Package Dependencies

Most Linux software depends on other packages.

APT automatically installs these required packages.

---

# Commands Learned

```bash
apt depends curl

apt depends git

apt depends nginx
```

---

# Dependency Workflow

```text
Package
    │
    ▼
Check Dependencies
    │
    ▼
Download Missing Packages
    │
    ▼
Install Dependencies
    │
    ▼
Install Main Package
```

---

# Dependency Keywords

| Keyword | Meaning |
|----------|----------|
| Depends | Required package |
| PreDepends | Must be installed first |
| Recommends | Recommended package |
| Suggests | Optional package |
| Breaks | Incompatible package/version |
| Replaces | Replaces another package |

---

# Dependency Example

```text
nginx
 ├── Depends: libc6
 ├── Depends: libssl3
 ├── Depends: zlib1g
 └── Recommends: ssl-cert
```

---

# Installation vs Dependencies

| Component | Responsibility |
|-----------|----------------|
| Main Package | Software to install |
| Dependency | Required supporting packages |
| Repository | Stores packages |
| APT | Resolves and installs everything automatically |

---

# Best Practices

- Verify software after installation.
- Remove unused software regularly.
- Use `purge` when you want to remove configuration files.
- Run `autoremove` periodically to clean unused dependencies.
- Use `autoclean` before `clean` for routine maintenance.
- Understand package dependencies before installing production software.


# dpkg (Low-Level Package Manager)

APT is the high-level package manager.

Behind APT, Linux actually uses **dpkg** to install and manage `.deb` packages.

Think of it like this:

```text
Repository
     │
     ▼
APT
     │
     ▼
dpkg
     │
     ▼
.deb Package
```

---

# APT vs dpkg

| Feature | APT | dpkg |
|----------|-----|------|
| Dependency Resolution | ✅ Automatic | ❌ No |
| Downloads Packages | ✅ Yes | ❌ No |
| Installs Packages | ✅ Yes | ✅ Yes |
| Removes Packages | ✅ Yes | ✅ Yes |
| Works with Repositories | ✅ Yes | ❌ No |
| Works with Local `.deb` Files | Limited | ✅ Yes |

---

# Commands Learned

```bash
dpkg -L curl

dpkg -l | head -20

dpkg -S /usr/bin/curl
```

---

# Command Summary

| Command | Purpose |
|----------|----------|
| dpkg -L curl | List all files installed by the package |
| dpkg -l | List installed packages |
| dpkg -S /path/file | Find which package owns a file |

---

# dpkg Flags

| Flag | Meaning |
|------|----------|
| -L | List files of a package |
| -l | List installed packages |
| -S | Search package by file |

---

# dpkg Workflow

```text
Installed Package
        │
        ▼
dpkg -L
        │
        ▼
Installed Files

Installed File
        │
        ▼
dpkg -S
        │
        ▼
Package Name
```

---

# Example

```bash
dpkg -L curl
```

Output (simplified):

```text
/usr/bin/curl
/usr/share/man/man1/curl.1.gz
/usr/share/doc/curl
...
```

Meaning:

The **curl** package installed these files into the system.

---

# Example

```bash
dpkg -S /usr/bin/curl
```

Output:

```text
curl: /usr/bin/curl
```

Meaning:

The executable `/usr/bin/curl` belongs to the **curl** package.

---

# Package Management Hierarchy

```text
Repository
      │
      ▼
APT
      │
      ▼
dpkg
      │
      ▼
Installed Files
```

---

# Package Management Commands Learned

## Information

```bash
apt
apt --version
which apt
```

---

## Search

```bash
apt search nginx

apt show nginx

apt list --installed
```

---

## Update

```bash
sudo apt update

apt list --upgradable
```

---

## Upgrade

```bash
sudo apt upgrade

sudo apt full-upgrade
```

---

## Install

```bash
sudo apt install tree

tree --version

which tree
```

---

## Remove

```bash
sudo apt remove tree

sudo apt purge tree

sudo apt autoremove
```

---

## Cache

```bash
sudo apt autoclean

sudo apt clean
```

---

## Dependency

```bash
apt depends curl

apt depends git

apt depends nginx
```

---

## dpkg

```bash
dpkg -L curl

dpkg -l

dpkg -S /usr/bin/curl
```

---

# Package Management Lifecycle

```text
Search Package
      │
      ▼
Show Details
      │
      ▼
Update Repository
      │
      ▼
Upgrade Packages
      │
      ▼
Install Package
      │
      ▼
Verify Installation
      │
      ▼
Check Dependencies
      │
      ▼
Inspect Installed Files
      │
      ▼
Remove Package
      │
      ▼
Remove Configuration
      │
      ▼
Remove Unused Dependencies
      │
      ▼
Clean Package Cache
```

---

# Command Categories

| Category | Commands |
|----------|----------|
| Package Info | `apt`, `apt --version`, `which apt` |
| Search | `apt search`, `apt show`, `apt list --installed` |
| Update | `apt update`, `apt list --upgradable` |
| Upgrade | `apt upgrade`, `apt full-upgrade` |
| Install | `apt install` |
| Verify | `which`, `--version` |
| Remove | `apt remove`, `apt purge`, `apt autoremove` |
| Cache | `apt autoclean`, `apt clean` |
| Dependencies | `apt depends` |
| Package Files | `dpkg -L` |
| Installed Packages | `dpkg -l` |
| File Owner | `dpkg -S` |

---

# Best Practices

- Always run `apt update` before installing new software.
- Verify installed software using `which` or `--version`.
- Prefer `apt` over `dpkg` for normal package installation because it resolves dependencies automatically.
- Use `dpkg` when inspecting installed packages or working with local `.deb` files.
- Use `apt autoremove` regularly to remove unused dependency packages.
- Use `apt autoclean` or `apt clean` periodically to free disk space.
- Install practice packages only inside your dedicated **Linux-Practice** workflow unless a project requires otherwise.

---

# Learning Outcome

After completing **Step 08 – Package Management**, you can now:

- Understand how Linux package management works.
- Use the APT package manager confidently.
- Search and inspect packages.
- Update and upgrade the system safely.
- Install and verify software.
- Remove packages correctly.
- Clean package cache.
- Understand package dependencies.
- Inspect installed package files with `dpkg`.
- Identify which package owns a specific file.
- Follow the complete Linux package management workflow used in professional Ubuntu systems.
```
