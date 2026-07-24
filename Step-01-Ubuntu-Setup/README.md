# Step 01 - Ubuntu Setup

## 📖 Overview

Before learning Linux, Docker, Kubernetes, or any DevOps tools, a stable Linux environment is required.

In this step, Ubuntu was installed inside Oracle VirtualBox and configured as the primary development environment.

This environment will be used throughout the entire DevOps learning journey.

---

# 🎯 Learning Objectives

After completing this step, you will be able to:

- Install Ubuntu inside VirtualBox
- Configure a virtual machine
- Update the operating system
- Install VirtualBox Guest Additions
- Enable copy & paste between host and guest
- Enable drag & drop
- Create a clean virtual machine snapshot

---

# 🖥️ Development Environment

| Component | Value |
|-----------|-------|
| Host OS | Windows 10 |
| Virtualization Software | Oracle VirtualBox |
| Guest OS | Ubuntu 26.04 LTS |
| RAM | 4 GB |
| CPU | 2 Cores |
| Storage | 60 GB |
| Shell | Bash |

---

# 📦 Software Used

- Oracle VirtualBox
- Ubuntu 26.04 LTS ISO
- VirtualBox Guest Additions

---

# 📋 Completed Tasks

- ✅ Installed Ubuntu 26.04
- ✅ Created Virtual Machine
- ✅ Allocated RAM, CPU and Storage
- ✅ Updated Ubuntu Packages
- ✅ Upgraded Installed Packages
- ✅ Removed Unnecessary Packages
- ✅ Cleaned Package Cache
- ✅ Installed VirtualBox Guest Additions
- ✅ Enabled Shared Clipboard
- ✅ Enabled Drag & Drop
- ✅ Enabled Auto Resize Display
- ✅ Created Initial Snapshot

---

# 📚 Commands Used

## Update Package Index

```bash
sudo apt update
```

Updates the local package list.

---

## Upgrade Installed Packages

```bash
sudo apt upgrade -y
```

Installs the latest available updates.

---

## Remove Unused Packages

```bash
sudo apt autoremove -y
```

Removes packages that are no longer required.

---

## Clean Package Cache

```bash
sudo apt autoclean
```

Removes outdated package files from the cache.

---

# 💡 Why These Commands Matter

Keeping the operating system updated is one of the first responsibilities of every Linux user and DevOps engineer.

An updated system is generally more secure, stable, and compatible with modern development tools.

---

# 🌍 Real-World Use Cases

These commands are commonly executed when:

- Setting up a new development machine
- Preparing a cloud server
- Configuring an Ubuntu VPS
- Creating Docker host machines
- Provisioning virtual machines

---

# 📌 Key Takeaways

- Ubuntu is the primary operating system used throughout this repository.
- Every future project depends on this environment.
- A clean and updated Linux system reduces many future problems.

---

# 🎤 Interview Questions

### Why do we run `sudo apt update` before installing software?

### What is the difference between `apt update` and `apt upgrade`?

### Why is `autoremove` useful?

### Why should a fresh Linux installation be updated?

---

# 📚 Quick Revision

| Command | Purpose |
|----------|---------|
| `sudo apt update` | Refresh package list |
| `sudo apt upgrade -y` | Upgrade installed packages |
| `sudo apt autoremove -y` | Remove unused packages |
| `sudo apt autoclean` | Clean package cache |

---

# ✅ Status

**Completed**
