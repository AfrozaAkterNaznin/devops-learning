# Step 10 — Linux SSH Administration

---

# Objective

Learn how Linux systems provide secure remote administration using SSH.

This lab covers:

- SSH client and server
- SSH service
- Hostnames
- SSH keys
- SSH configuration
- Local SSH login
- SCP
- SFTP
- Configuration verification

---

# Learning Outcomes

After completing this lab, I can:

- Understand SSH architecture.
- Distinguish SSH Client vs SSH Server.
- Generate and identify SSH keys.
- Connect securely using SSH.
- Transfer files using SCP.
- Browse remote files using SFTP.
- Read and verify SSH configuration.
- Validate SSH configuration safely before applying changes.

---

# Real Lab Workflow

```text
Install/OpenSSH
        │
        ▼
Check SSH Client
        │
        ▼
Check Host Information
        │
        ▼
Check SSH Service
        │
        ▼
Generate / Inspect SSH Keys
        │
        ▼
SSH localhost
        │
        ▼
Read SSH Configuration
        │
        ▼
Backup Configuration
        │
        ▼
Syntax Check
        │
        ▼
Reload SSH
        │
        ▼
SCP File Transfer
        │
        ▼
SFTP Session
```

---

# Complete Lab Overview

| Step | Topic | Purpose |
|------|---------|---------|
| 10.1 | SSH Basics | Understand SSH |
| 10.2 | Host Information | Identify local machine |
| 10.3 | SSH Service | Verify SSH server |
| 10.4 | SSH Keys | Authentication |
| 10.5 | Localhost SSH | Test secure login |
| 10.6 | SSH Configuration | Read configuration |
| 10.7 | Backup Config | Safe administration |
| 10.8 | Verify Config | Validate configuration |
| 10.9 | SCP & SFTP | Secure file transfer |

---

# What to Observe

| Step | Observe |
|-------|----------|
| 10.1 | SSH version, binary location |
| 10.2 | Hostname, IP address, Port 22 |
| 10.3 | ssh.service status, ssh.socket, listening port |
| 10.4 | Public key, Private key, ~/.ssh contents |
| 10.5 | SSH login, prompt changes, logout |
| 10.6 | sshd_config location, important settings |
| 10.7 | Backup file created, grep results |
| 10.8 | Syntax check exit code, reload status |
| 10.9 | SCP upload, SCP download, SFTP navigation |

---

# SSH Workflow

| Operation | Tool |
|-----------|------|
| Remote Login | ssh |
| File Upload | scp |
| File Download | scp |
| Interactive File Transfer | sftp |
| Server Process | sshd |
| Configuration | sshd_config |
| Client Configuration | ssh_config |

---

# SSH Components

| Component | Purpose |
|------------|----------|
| ssh | Client program |
| sshd | SSH server daemon |
| ssh-keygen | Generate key pairs |
| scp | Secure copy |
| sftp | Secure file transfer |
| ssh-agent | Store decrypted keys |
| ssh-add | Add keys to agent |

---

# Important Files

| File | Purpose |
|------|----------|
| ~/.ssh/id_ed25519 | Private key |
| ~/.ssh/id_ed25519.pub | Public key |
| ~/.ssh/authorized_keys | Allowed public keys |
| ~/.ssh/known_hosts | Trusted servers |
| /etc/ssh/ssh_config | Client configuration |
| /etc/ssh/sshd_config | Server configuration |

---

# Important Directories

| Directory | Purpose |
|-----------|----------|
| ~/.ssh | User SSH files |
| /etc/ssh | System SSH configuration |

---

# SSH Authentication Flow

```text
Client
   │
   │ SSH
   ▼
Server

↓

Verify Host Key

↓

Authentication

↓

Open Secure Session

↓

Execute Commands / Transfer Files
```

---

# Local Lab Architecture

```text
Ubuntu VM

Client (ssh)

↓

SSH

↓

Server (localhost)

↓

Same Machine
```

---

# Real Production Architecture

```text
Laptop

↓

Internet

↓

Cloud Server

↓

SSH
```

---

# Local Practice vs Production

| Local Lab | Real Server |
|-----------|-------------|
| localhost | AWS / Azure / VPS |
| Same Machine | Different Machine |
| Safe Practice | Production Environment |
| Learning | Real Administration |

---

# Command Categories

| Category | Commands |
|-----------|----------|
| Information | ssh -V, which ssh |
| Host | hostname, hostnamectl |
| Service | systemctl |
| Keys | ssh-keygen |
| Login | ssh |
| Config | grep, less |
| Validation | sshd -t |
| Transfer | scp |
| Interactive | sftp |

---

# Professional Workflow

```text
Check Service

↓

Check Configuration

↓

Backup Config

↓

Edit Config

↓

Syntax Check

↓

Reload SSH

↓

Test Login

↓

Deploy
```

# Step 10 — Linux SSH Administration

# Documentation — Part 2

---

# SSH Commands Used in This Lab

| Command | Purpose |
|----------|----------|
| ssh | Secure remote login |
| sshd | SSH server daemon |
| ssh-keygen | Generate SSH key pair |
| scp | Secure file copy |
| sftp | Secure file transfer |
| hostname | Show hostname |
| hostnamectl | Detailed host information |
| systemctl | Manage SSH service |
| ss | Check listening ports |
| grep | Search configuration |
| less | Read configuration file |
| cp | Backup configuration |
| sshd -t | Validate SSH configuration |

---

# Command Variations

## 1. ssh

| Command | Purpose |
|----------|----------|
| ssh user@host | Connect to remote host |
| ssh localhost | Connect to local SSH server |
| ssh -p 2222 user@host | Custom port |
| ssh -i key user@host | Login using private key |
| ssh user@IP "command" | Execute remote command |

---

## 2. ssh-keygen

| Command | Purpose |
|----------|----------|
| ssh-keygen | Generate default key |
| ssh-keygen -t ed25519 | Generate ED25519 key |
| ssh-keygen -t rsa -b 4096 | Generate RSA key |
| ssh-keygen -l -f key.pub | Show fingerprint |

---

## 3. scp

| Command | Purpose |
|----------|----------|
| scp file user@host:/path | Upload file |
| scp user@host:/path/file . | Download file |
| scp -r folder user@host:/path | Upload directory |
| scp -P 2222 file user@host:/path | Custom SSH port |

---

## 4. sftp

| Command | Purpose |
|----------|----------|
| sftp user@host | Connect |
| pwd | Remote current directory |
| lpwd | Local current directory |
| ls | Remote files |
| lls | Local files |
| cd | Change remote directory |
| lcd | Change local directory |
| put file | Upload |
| get file | Download |
| bye | Exit |

---

## 5. systemctl

| Command | Purpose |
|----------|----------|
| systemctl status ssh | Service status |
| systemctl start ssh | Start |
| systemctl stop ssh | Stop |
| systemctl restart ssh | Restart |
| systemctl reload ssh | Reload config |
| systemctl enable ssh | Enable boot |
| systemctl disable ssh | Disable boot |
| systemctl is-enabled ssh | Boot status |
| systemctl is-active ssh | Running status |

---

## 6. ss

| Command | Purpose |
|----------|----------|
| ss -tln | TCP listening ports |
| ss -tulpn | TCP/UDP with process |
| ss -tln \| grep :22 | Check SSH port |

---

## 7. grep

| Command | Purpose |
|----------|----------|
| grep Port file | Search Port |
| grep PasswordAuthentication file | Search authentication |
| grep PubkeyAuthentication file | Search public key |
| grep PermitRootLogin file | Search root login |

---

## 8. less

| Command | Purpose |
|----------|----------|
| less file | Read file |
| /keyword | Search |
| n | Next result |
| N | Previous result |
| q | Quit |

---

# Frequently Used Flags

## ssh

| Flag | Meaning |
|------|----------|
| -p | Port |
| -i | Identity file |
| -v | Verbose |
| -X | X11 forwarding |

---

## ssh-keygen

| Flag | Meaning |
|------|----------|
| -t | Key type |
| -b | Key size |
| -f | Output filename |
| -C | Comment |

---

## scp

| Flag | Meaning |
|------|----------|
| -r | Recursive |
| -P | Port |
| -i | Identity file |
| -v | Verbose |

---

## systemctl

| Option | Purpose |
|----------|----------|
| start | Start service |
| stop | Stop service |
| restart | Restart |
| reload | Reload configuration |
| enable | Auto start |
| disable | Disable auto start |
| status | Current status |

---

# Important Configuration Files

| File | Description |
|------|-------------|
| /etc/ssh/sshd_config | SSH Server configuration |
| /etc/ssh/ssh_config | SSH Client configuration |
| ~/.ssh/id_ed25519 | Private key |
| ~/.ssh/id_ed25519.pub | Public key |
| ~/.ssh/authorized_keys | Authorized client keys |
| ~/.ssh/known_hosts | Trusted servers |

---

# Configuration Options Observed

| Option | Current Value | Purpose |
|---------|---------------|----------|
| Port | 22 | SSH listening port |
| PermitRootLogin | prohibit-password | Disable root password login |
| PasswordAuthentication | yes | Allow password login |
| PubkeyAuthentication | yes | Allow key login |

---

# SSH Service Workflow

```text
Install OpenSSH

↓

Enable SSH

↓

Start SSH

↓

Check Status

↓

Verify Port

↓

SSH Login

↓

Read Configuration

↓

Backup Config

↓

Validate

↓

Reload

↓

Test Again
```

---

# Real Lab Summary

| Task | Result |
|------|--------|
| SSH Client | Installed |
| SSH Server | Installed |
| SSH Port | 22 |
| SSH Service | Running |
| SSH Socket | Listening |
| SSH Login | Successful |
| Key Pair | Already Exists |
| Config Backup | Completed |
| Syntax Check | Passed |
| Reload | Successful |
| SCP Upload | Successful |
| SCP Download | Successful |
| SFTP Session | Successful |

---

# Common Interview Questions

| Question | Short Answer |
|-----------|--------------|
| What is SSH? | Secure remote login protocol |
| Default SSH Port? | 22 |
| Difference between ssh and sshd? | Client vs Server daemon |
| Difference between SCP and SFTP? | SCP copies files; SFTP provides an interactive file transfer session |
| Which file stores server configuration? | `/etc/ssh/sshd_config` |
| Which file stores client configuration? | `/etc/ssh/ssh_config` |
| How to verify SSH config? | `sudo sshd -t` |
| Reload without disconnecting users? | `sudo systemctl reload ssh` |

---

# Professional Notes

- Always backup `sshd_config` before editing.
- Always run `sudo sshd -t` before reloading.
- Prefer **ED25519** keys for new systems.
- Never share your private key (`id_ed25519`).
- Use SCP/SFTP instead of insecure FTP.
- Verify SSH access before logging out of a production server.


# Step 10 — Linux SSH Administration

# Documentation — Part 3

---

# SSH Concepts

| Concept | Short Meaning |
|----------|---------------|
| SSH | Secure Shell protocol for secure remote administration |
| SSH Client | The machine that initiates the connection |
| SSH Server | The machine that accepts the connection |
| ssh | SSH client program |
| sshd | SSH server daemon (background service) |
| SSH Key | Authentication credential |
| Public Key | Can be shared with servers |
| Private Key | Must remain secret |
| SCP | Secure file copy over SSH |
| SFTP | Interactive file transfer over SSH |

---

# Client vs Server

| SSH Client | SSH Server |
|------------|------------|
| Starts connection | Accepts connection |
| Command: `ssh` | Service: `sshd` |
| Installed on laptop | Installed on server |
| Uses `ssh_config` | Uses `sshd_config` |

---

# ssh vs sshd

| ssh | sshd |
|-----|------|
| Client program | Server daemon |
| User executes it | Runs in background |
| Login to server | Waits for incoming connection |

---

# SSH Authentication Methods

| Method | Description |
|----------|-------------|
| Password Authentication | Username + Password |
| Public Key Authentication | Public/Private Key Pair |

---

# SSH Keys

| File | Purpose |
|------|----------|
| id_ed25519 | Private Key |
| id_ed25519.pub | Public Key |
| authorized_keys | Allowed client keys |
| known_hosts | Trusted servers |

---

# SSH File Transfer Tools

| Tool | Purpose |
|------|----------|
| ssh | Remote login |
| scp | Copy files |
| sftp | Interactive file transfer |
| rsync | Efficient file synchronization |

---

# SCP vs SFTP vs RSYNC

| Feature | SCP | SFTP | RSYNC |
|----------|-----|------|--------|
| Login Shell | No | Yes | No |
| Interactive | No | Yes | No |
| Copy Files | ✅ | ✅ | ✅ |
| Synchronize | ❌ | ❌ | ✅ |
| Production Use | Small copies | Manual transfers | Backups & deployments |

---

# SSH Configuration Files

| File | Used By |
|------|----------|
| `/etc/ssh/sshd_config` | SSH Server |
| `/etc/ssh/ssh_config` | SSH Client |
| `~/.ssh/` | User SSH files |

---

# Local Lab vs Production

| Local Practice | Production Server |
|----------------|-------------------|
| localhost | AWS / Azure / GCP / VPS |
| Same VM | Different machine |
| Password login | Usually Key login |
| Safe testing | Real users & services |
| Learning | System administration |

---

# What Happened in This Lab

| Step | Result |
|------|--------|
| Checked SSH version | ✅ |
| Verified SSH installation | ✅ |
| Viewed hostname & IP | ✅ |
| Checked SSH service | ✅ |
| Generated / verified SSH keys | ✅ |
| Logged into localhost | ✅ |
| Read SSH configuration | ✅ |
| Backed up configuration | ✅ |
| Validated configuration | ✅ |
| Reloaded SSH service | ✅ |
| Uploaded file using SCP | ✅ |
| Downloaded file using SCP | ✅ |
| Opened SFTP session | ✅ |

---

# Real Lab Summary

| Property | Result |
|-----------|--------|
| Operating System | Ubuntu 26.04 LTS |
| SSH Client | OpenSSH 10.2p1 |
| SSH Server | OpenSSH Server |
| Default Port | 22 |
| SSH Service | Running |
| SSH Socket | Listening |
| Authentication | Password + Public Key Enabled |
| Key Type | ED25519 |
| Local Test | localhost |
| SCP Test | Successful |
| SFTP Test | Successful |

---

# Important Commands to Remember

| Purpose | Command |
|----------|----------|
| Login | `ssh localhost` |
| Upload | `scp file localhost:/tmp/` |
| Download | `scp localhost:/tmp/file .` |
| Open SFTP | `sftp localhost` |
| Check SSH | `systemctl status ssh` |
| Check Port | `ss -tln \| grep :22` |
| Backup Config | `cp sshd_config sshd_config.bak` |
| Validate Config | `sudo sshd -t` |
| Reload SSH | `sudo systemctl reload ssh` |

---

# Common Mistakes

| Mistake | Solution |
|----------|----------|
| Edit config without backup | Backup first |
| Reload without syntax check | Run `sudo sshd -t` |
| Share private key | Never share it |
| Forget port number | Check `sshd_config` |
| Disable authentication accidentally | Always test before logout |

---

# Best Practices

| Practice | Why |
|-----------|-----|
| Backup configuration | Easy recovery |
| Validate before reload | Prevent service failure |
| Use ED25519 keys | Modern & secure |
| Protect private key | Prevent unauthorized access |
| Use SCP/SFTP instead of FTP | Encrypted transfer |

---

# Professional Workflow

```text
Check SSH

↓

Check Service

↓

Check Configuration

↓

Backup Configuration

↓

Edit Configuration

↓

Syntax Check

↓

Reload SSH

↓

Test Login

↓

Transfer Files

↓

Done
```

---

# Key Takeaways

- SSH provides secure remote administration.
- `ssh` is the client, `sshd` is the server daemon.
- SSH supports password and key-based authentication.
- SCP securely copies files.
- SFTP provides interactive secure file transfer.
- Always validate (`sshd -t`) before reloading SSH.
- Never expose your private key.

---

# Conclusion

In this lab, a complete SSH administration workflow was performed on Ubuntu.

The following tasks were successfully completed:

- SSH installation verification
- Service management
- Host identification
- SSH key inspection
- Local SSH login
- SSH configuration review
- Configuration backup
- Configuration validation
- Service reload
- Secure file transfer using SCP
- Interactive file transfer using SFTP

This workflow closely matches the SSH administration process followed by Linux System Administrators and DevOps Engineers in production environments.
