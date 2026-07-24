# Step 02 - Git, GitHub & SSH Setup

## 📖 Overview

Git is a distributed version control system used to track changes in source code, collaborate with other developers, and manage software projects.

GitHub is a cloud-based platform for hosting Git repositories.

In this step, Git was configured on Ubuntu, SSH authentication was established with GitHub, and a local repository was created and pushed to GitHub.

---

# 🎯 Learning Objectives

After completing this step, you should understand how to:

- Configure Git identity
- Set the default Git branch
- Understand Git credentials
- Generate an SSH key pair
- Start the SSH agent
- Add a private key to the SSH agent
- Add a public SSH key to GitHub
- Test GitHub SSH authentication
- Initialize a Git repository
- Stage and commit files
- Connect local and remote repositories
- Push code to GitHub

---

# 1. Verify Git Installation

```bash
git --version
```

Example:

```text
git version 2.53.0
```

This confirms that Git is installed and accessible from the terminal.

---

# 2. Configure Git Identity

Git attaches an author's name and email address to commits.

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Check the configuration:

```bash
git config --global --list
```

### Why `--global`?

`--global` applies the configuration to the current Linux user's Git environment.

Without it, configuration can be applied only to a specific repository.

---

# 3. Configure the Default Branch

Set `main` as the default branch for newly initialized repositories:

```bash
git config --global init.defaultBranch main
```

Verify:

```bash
git config --global --list
```

---

# 4. Git Credential Helper

A Git credential helper can store or manage authentication credentials.

The following configuration was tested during this setup:

```bash
git config --global credential.helper store
```

> **Security Note:** `credential.helper store` stores credentials unencrypted on disk. It is useful to understand how Git credential helpers work, but SSH authentication or a secure credential manager is preferred for real-world environments.

Since this environment uses GitHub SSH authentication, SSH is the primary authentication method used in this repository.

---

# 5. Understanding SSH

SSH stands for **Secure Shell**.

It is a secure protocol commonly used for:

- Authenticating with GitHub
- Connecting to remote Linux servers
- Managing cloud servers
- Secure remote administration
- Automated deployments

For GitHub, SSH allows Git operations without repeatedly entering account credentials.

---

# 6. Generate an SSH Key

An Ed25519 SSH key pair was generated using:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

This creates two important files:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

| File | Purpose | Share? |
|---|---|---|
| `id_ed25519` | Private Key | ❌ Never |
| `id_ed25519.pub` | Public Key | ✅ Yes |

## ⚠️ Critical Security Rule

Never publish, upload, commit, or share the private SSH key.

```text
id_ed25519
```

The `.pub` file is the public key and can safely be added to GitHub.

---

# 7. SSH Passphrase

During key generation, SSH may ask for a passphrase.

A passphrase provides an additional security layer for the private key.

Conceptually:

```text
SSH Private Key
        +
Passphrase
        =
Stronger Protection
```

---

# 8. Start SSH Agent

Start the SSH authentication agent:

```bash
eval "$(ssh-agent -s)"
```

The SSH agent keeps decrypted private keys available in memory during the session.

Example output:

```text
Agent pid 6762
```

---

# 9. Add the SSH Private Key

Add the private key to the SSH agent:

```bash
ssh-add ~/.ssh/id_ed25519
```

The SSH agent can now use the key for authentication.

---

# 10. Display the Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

The output begins with something similar to:

```text
ssh-ed25519 ...
```

Only the **public key** should be copied to GitHub.

---

# 11. Add SSH Key to GitHub

The public key was added through:

```text
GitHub
→ Settings
→ SSH and GPG keys
→ New SSH key
```

A descriptive name such as:

```text
Ubuntu VirtualBox
```

can be used to identify the machine.

---

# 12. Test GitHub SSH Authentication

```bash
ssh -T git@github.com
```

During the first connection, SSH may ask whether the GitHub host should be trusted.

After successful authentication, GitHub returns a message indicating that authentication succeeded.

This confirms:

```text
Ubuntu
   ↓
SSH
   ↓
GitHub
   ↓
Authenticated
```

---

# 13. Create a Project Directory

```bash
mkdir -p ~/Projects
cd ~/Projects
```

Create a project:

```bash
mkdir devops-learning
cd devops-learning
```

---

# 14. Initialize a Git Repository

```bash
git init
```

This creates the hidden:

```text
.git/
```

directory.

The `.git` directory contains Git's repository metadata and history.

---

# 15. Check Repository Status

```bash
git status
```

`git status` shows information such as:

- Current branch
- Untracked files
- Modified files
- Staged files

This is one of the most frequently used Git commands.

---

# 16. Create a README

```bash
echo "# DevOps Learning Journey" > README.md
```

Check:

```bash
git status
```

The new file initially appears as **untracked**.

---

# 17. Git Working Areas

A basic Git workflow has three important areas:

```text
Working Directory
       │
       │ git add
       ▼
Staging Area
       │
       │ git commit
       ▼
Local Repository
       │
       │ git push
       ▼
Remote Repository (GitHub)
```

Understanding this flow is fundamental to Git.

---

# 18. Stage a File

```bash
git add README.md
```

Check:

```bash
git status
```

The file is now staged and ready to be committed.

To stage all changes:

```bash
git add .
```

---

# 19. Create a Commit

```bash
git commit -m "Initial commit"
```

A commit creates a saved snapshot of the staged changes in Git history.

---

# 20. View Commit History

```bash
git log --oneline
```

This displays a compact version of the repository's commit history.

Example:

```text
2363be1 Initial commit
```

The first part is the abbreviated commit hash.

---

# 21. Local vs Remote Repository

A **local repository** exists on the developer's machine.

A **remote repository** exists on a remote Git hosting platform such as GitHub.

```text
Ubuntu / Local Repository
          │
          │ SSH
          ▼
       GitHub
 Remote Repository
```

---

# 22. Add GitHub as Remote

```bash
git remote add origin git@github.com:USERNAME/devops-learning.git
```

`origin` is the conventional name for the primary remote repository.

Verify it:

```bash
git remote -v
```

Typical output:

```text
origin  git@github.com:USERNAME/devops-learning.git (fetch)
origin  git@github.com:USERNAME/devops-learning.git (push)
```

---

# 23. Push to GitHub

```bash
git push -u origin main
```

Meaning:

| Part | Meaning |
|---|---|
| `git push` | Upload commits |
| `origin` | Remote repository |
| `main` | Branch |
| `-u` | Set upstream tracking |

After upstream tracking is configured, future pushes can normally use:

```bash
git push
```

---

# 🔄 Daily Git Workflow

For everyday development, one of the most common workflows is:

```bash
git status
git add .
git commit -m "meaningful commit message"
git push
```

A useful habit is to check:

```bash
git status
```

before committing.

---

# 🌍 Why This Matters for Backend & DevOps

Git and SSH are foundational tools in modern software engineering.

### Backend Development

Git is used to:

- Maintain application source code
- Track API changes
- Collaborate with developers
- Manage releases and branches

### DevOps

Git and SSH are commonly involved in:

- CI/CD pipelines
- Infrastructure repositories
- Remote server administration
- Deployment automation
- GitOps workflows
- Configuration management

---

# 🔐 SSH Security Rules

1. Never share a private SSH key.
2. Never commit private keys to Git.
3. Public keys can be shared.
4. Use a passphrase when stronger key protection is required.
5. Remove old or unknown SSH keys from GitHub.
6. Use separate keys when environments require stronger isolation.

---

# ❌ Common Mistakes

### Mistake 1: Sharing the private key

Never share:

```text
~/.ssh/id_ed25519
```

### Mistake 2: Forgetting to stage changes

Running:

```bash
git commit
```

does not automatically stage every modified file.

Use:

```bash
git add .
```

when appropriate.

### Mistake 3: Wrong remote URL

Check with:

```bash
git remote -v
```

### Mistake 4: Wrong branch

Check the current branch with:

```bash
git branch
```

or:

```bash
git status
```

---

# 🎤 Interview Questions

## Q1. What is Git?

Git is a distributed version control system used to track changes in files and coordinate software development.

## Q2. What is GitHub?

GitHub is a platform for hosting and collaborating on Git repositories.

## Q3. What is the difference between Git and GitHub?

Git is the version control system.

GitHub is a hosting and collaboration platform built around Git repositories.

## Q4. What is a Git repository?

A repository is a project directory whose changes and history are tracked by Git.

## Q5. What does `git add` do?

It adds selected changes to the staging area.

## Q6. What does `git commit` do?

It records staged changes as a snapshot in the local repository history.

## Q7. What does `git push` do?

It sends local commits to a remote repository.

## Q8. What is `origin`?

`origin` is the conventional default name assigned to a remote repository.

## Q9. What is SSH?

SSH is a secure protocol used for encrypted remote communication and authentication.

## Q10. Public key vs private key?

The public key can be shared with services such as GitHub.

The private key must remain secret on the user's machine.

## Q11. What is the staging area?

It is the intermediate area where changes are prepared before creating a commit.

## Q12. What is a commit?

A commit is a recorded snapshot of staged changes in Git history.

---

# ⚡ Interview Quick Revision

```text
Working Directory
      ↓ git add
Staging Area
      ↓ git commit
Local Repository
      ↓ git push
Remote Repository
```

And remember:

```text
Git      = Version Control
GitHub   = Git Hosting Platform
SSH      = Secure Authentication / Communication
origin   = Remote Name
main     = Branch
commit   = Saved Snapshot
push     = Local → Remote
```

---

# 📚 Command Cheat Sheet

| Command | Purpose |
|---|---|
| `git --version` | Check Git version |
| `git config --global --list` | View global configuration |
| `git init` | Initialize repository |
| `git status` | Check repository status |
| `git add <file>` | Stage a file |
| `git add .` | Stage changes |
| `git commit -m "message"` | Create commit |
| `git log --oneline` | View compact history |
| `git remote -v` | View remotes |
| `git push` | Push commits |
| `ssh-keygen` | Generate SSH keys |
| `ssh-add` | Add private key to agent |
| `ssh -T git@github.com` | Test GitHub SSH authentication |

---

# 🧠 Five Things to Remember Before an Interview

1. **Git and GitHub are not the same thing.**
2. **`git add` → staging, `git commit` → local history, `git push` → remote.**
3. **Never share an SSH private key.**
4. **`origin` is a remote name, not GitHub itself.**
5. **SSH provides secure authentication and communication.**

---

# ✅ Status

**Completed**

Git configuration, SSH authentication, local repository creation, commits, remote configuration, and GitHub push were successfully tested.
