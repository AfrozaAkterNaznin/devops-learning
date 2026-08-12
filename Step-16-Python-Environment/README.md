# Step 16 — Python Environment

## Part 1 — Foundation

## 1. Objective

The objective of Step 16 is to build a professional Python environment for DevOps-oriented development and automation.

This step focuses on using Python as a DevOps tool rather than learning Python programming from the beginning.

The lab covers:

* Python runtime verification
* `pip` package management
* Python virtual environments
* Dependency management
* `requirements.txt`
* Reproducible Python environments
* Environment variables
* `.env` configuration
* Basic Python project structure
* Linux system-information automation
* Final environment verification

---

## 2. Learning Outcomes

After completing Step 16, the learner can:

| Skill                 | Outcome                                            |
| --------------------- | -------------------------------------------------- |
| Python Runtime        | Verify Python installation and runtime information |
| pip                   | Install, inspect and remove Python packages        |
| Virtual Environment   | Create and activate an isolated `.venv`            |
| Package Isolation     | Understand project-level package installation      |
| Dependencies          | Identify direct and transitive dependencies        |
| `requirements.txt`    | Capture exact package versions                     |
| Reproducibility       | Recreate an environment from `requirements.txt`    |
| Environment Variables | Read configuration from environment variables      |
| `.env`                | Manage local application configuration             |
| Project Structure     | Organize a small Python DevOps project             |
| Automation            | Use Python to collect Linux system information     |
| Verification          | Validate the final environment and dependencies    |

---

# 3. Why Python Matters for DevOps

Python is useful in DevOps because it can automate repetitive system and infrastructure-related tasks.

| DevOps Area           | Python Usage                                                 |
| --------------------- | ------------------------------------------------------------ |
| System Administration | Collect system information and automate administrative tasks |
| Automation            | Replace repetitive manual operations with scripts            |
| API Integration       | Communicate with REST APIs                                   |
| CI/CD                 | Build helper and deployment scripts                          |
| Monitoring            | Collect system and application information                   |
| Infrastructure        | Automate interactions with infrastructure tools              |
| Configuration         | Read environment-based configuration                         |
| Tooling               | Build internal DevOps utilities                              |

The important DevOps workflow is:

```text
Linux System
    ↓
Python
    ↓
Automation Script
    ↓
System/API/Infrastructure Interaction
    ↓
Repeatable Operation
```

---

# 4. Step 16 Scope

Step 16 is intentionally limited to Python concepts that are useful for DevOps work.

### Included

* Python 3 runtime
* `python3`
* `pip`
* `venv`
* Virtual environment activation
* Package installation
* Package inspection
* Package removal
* Dependencies
* `pip freeze`
* `requirements.txt`
* Environment reproduction
* Environment variables
* `python-dotenv`
* `.env`
* `.gitignore`
* Basic Python project structure
* `psutil`
* Linux system-information automation

### Not Included

Step 16 does not cover:

* Python programming from zero
* Basic programming theory
* Advanced Python language features
* Competitive programming
* Data science
* Machine learning
* Django/Flask application development
* Advanced Python internals
* Large Python application development

The learner already has programming fundamentals, so the focus remains on practical DevOps usage.

---

# 5. Python Environment Architecture

The environment developed during the lab follows this structure:

```text
Ubuntu Linux
    │
    ├── System Python
    │      └── /usr/bin/python3
    │
    └── Project Virtual Environment
           └── .venv/
                ├── bin/
                ├── include/
                ├── lib/
                └── pyvenv.cfg
```

The project-specific environment provides an isolated package environment for the Python project.

---

# 6. Virtual Environment Concept

A Python virtual environment provides an isolated environment for project dependencies.

Without a virtual environment:

```text
System Python
    │
    ├── Project A packages
    ├── Project B packages
    └── Project C packages
```

With virtual environments:

```text
System Python
    │
    ├── Project A
    │     └── .venv/
    │
    ├── Project B
    │     └── .venv/
    │
    └── Project C
          └── .venv/
```

This allows different projects to maintain their own dependency environments.

---

# 7. Virtual Environment Lifecycle

The practical workflow used in this lab was:

```text
Verify Python
     ↓
Create .venv
     ↓
Activate .venv
     ↓
Use isolated Python/pip
     ↓
Install packages
     ↓
Inspect packages
     ↓
Remove packages when required
     ↓
Deactivate/clean temporary environments
```

The important distinction is:

| Component          | Purpose                                      |
| ------------------ | -------------------------------------------- |
| System Python      | Ubuntu/system-level Python runtime           |
| `.venv`            | Project-specific environment                 |
| `pip`              | Python package manager                       |
| `site-packages`    | Location where Python packages are installed |
| `requirements.txt` | Dependency specification                     |

---

# 8. Package Management Architecture

Python packages are installed into the active virtual environment.

During the lab:

```text
pip install requests
```

resulted in:

```text
requests
├── certifi
├── charset-normalizer
├── idna
└── urllib3
```

This demonstrates the concept of dependencies.

A package can depend on other packages, so installing one package may install several additional packages.

---

# 9. Direct Dependencies vs Transitive Dependencies

| Type                  | Meaning                                          | Example from Lab                                   |
| --------------------- | ------------------------------------------------ | -------------------------------------------------- |
| Direct dependency     | Package explicitly requested by the user/project | `requests`                                         |
| Transitive dependency | Package required by another package              | `urllib3`, `idna`, `certifi`, `charset-normalizer` |

Example:

```text
Project
   │
   └── requests
        ├── certifi
        ├── charset-normalizer
        ├── idna
        └── urllib3
```

Understanding this relationship is important when troubleshooting dependency conflicts and maintaining reproducible environments.

---

# 10. `pip list` and `pip freeze`

Two package-inspection approaches were used during the lab.

| Command                | Purpose                               | Typical Output     |
| ---------------------- | ------------------------------------- | ------------------ |
| `python -m pip list`   | Human-readable installed package list | Package + Version  |
| `python -m pip freeze` | Requirements-style dependency listing | `package==version` |

Example:

```text
pip list
    ↓
requests  2.34.2
```

Whereas:

```text
pip freeze
    ↓
requests==2.34.2
```

`pip freeze` is particularly useful when creating a dependency specification.

---

# 11. Dependency Reproducibility

A major goal of the lab was to reproduce an existing Python environment.

The workflow was:

```text
Existing Environment
        │
        │ pip freeze
        ↓
requirements.txt
        │
        │ pip install -r requirements.txt
        ↓
New Virtual Environment
        │
        ↓
Same Packages + Versions
```

This allows a Python environment to be recreated on another machine or server.

---

# 12. `requirements.txt`

`requirements.txt` is a text-based dependency specification.

The actual lab generated a dependency file containing the installed package versions.

Conceptually:

```text
package==version
```

Example:

```text
requests==2.34.2
psutil==7.2.2
python-dotenv==1.2.2
```

The actual file also contains the dependencies required by `requests`.

### Purpose

| Purpose           | Benefit                                           |
| ----------------- | ------------------------------------------------- |
| Dependency record | Documents required packages                       |
| Reproducibility   | Recreates an environment                          |
| Deployment        | Helps install dependencies on servers             |
| CI/CD             | Provides predictable package installation         |
| Collaboration     | Gives other developers a dependency specification |

---

# 13. Environment Variables

Environment variables allow application configuration to be separated from source code.

The lab used:

```text
APP_ENV=development
APP_NAME=devops-python-lab
APP_PORT=5000
```

The conceptual flow was:

```text
Environment Configuration
        ↓
       .env
        ↓
 load_dotenv()
        ↓
   os.getenv()
        ↓
 Python Application
```

---

# 14. Why Configuration Should Be Separated from Code

Hardcoded configuration:

```text
Python Source
     │
     └── APP_PORT = 5000
```

Environment-based configuration:

```text
Python Source
     │
     └── os.getenv("APP_PORT")
                  ↑
                  │
             Environment
```

This makes the same application easier to use across different environments.

Example:

| Environment | Possible Configuration |
| ----------- | ---------------------- |
| Development | `APP_ENV=development`  |
| Testing     | `APP_ENV=test`         |
| Production  | `APP_ENV=production`   |

The application code can remain unchanged while configuration changes according to the environment.

---

# 15. `.env`

The lab used a `.env` file for local configuration.

Example structure:

```text
.env
├── APP_ENV
├── APP_NAME
└── APP_PORT
```

The `.env` file was loaded using `python-dotenv`.

### Important Security Principle

`.env` files may contain:

* Database credentials
* API keys
* Tokens
* Passwords
* Environment-specific secrets

Therefore `.env` should normally be excluded from Git repositories.

The lab configured:

```text
.env
```

inside `.gitignore`.

The actual `.env` used during the lab did not contain sensitive credentials; it contained application configuration values.

---

# 16. Python Project Structure

The final practical project structure is:

```text
Step-16-Python-Environment/
├── .env
├── .gitignore
├── .venv/
├── requirements.txt
└── src/
    ├── app.py
    └── system_info.py
```

### Directory/File Responsibilities

| Item               | Purpose                       | Git |
| ------------------ | ----------------------------- | --- |
| `.venv/`           | Local virtual environment     | No  |
| `.env`             | Local configuration           | No  |
| `.gitignore`       | Git exclusion rules           | Yes |
| `requirements.txt` | Dependency specification      | Yes |
| `src/`             | Python source directory       | Yes |
| `app.py`           | Configuration-reading example | Yes |
| `system_info.py`   | DevOps automation script      | Yes |

---

# 17. `.gitignore` Concept

The project uses ignore rules for local/generated files:

```text
.venv/
.env
__pycache__/
*.py[cod]
```

Conceptually:

```text
Source Code              → Git
requirements.txt         → Git
.gitignore               → Git

Virtual Environment      → Local only
.env                     → Local only
Python Cache             → Local/generated
```

This keeps the repository clean and prevents local environments or sensitive configuration from being committed.

---

# 18. DevOps Automation Architecture

The practical automation script uses Python to collect information directly from the Linux system.

```text
Linux Host
    │
    ├── Hostname
    ├── CPU
    ├── Memory
    ├── Disk
    ├── OS
    └── Python Runtime
          │
          ↓
   Python Automation
          │
          ↓
   System Information Report
```

The script uses standard Python modules and `psutil` to collect system information programmatically.

---

# 19. Automation Components

| Python Module | Purpose in Lab                          |
| ------------- | --------------------------------------- |
| `os`          | CPU count and working directory         |
| `platform`    | Operating system and Python information |
| `socket`      | Hostname                                |
| `shutil`      | Disk usage                              |
| `psutil`      | Memory/system information               |

This demonstrates how Python can interact with a Linux environment programmatically.

---

# 20. Practical DevOps Workflow

The complete Step 16 workflow was:

```text
Environment Discovery
        ↓
Python Runtime Verification
        ↓
Virtual Environment
        ↓
pip Package Management
        ↓
Dependency Inspection
        ↓
requirements.txt
        ↓
Environment Reproduction
        ↓
Environment Variables
        ↓
Python Project Structure
        ↓
Linux System Automation
        ↓
Final Verification
```

---

# 21. Key Concepts

| Concept              | Core Idea                                       |
| -------------------- | ----------------------------------------------- |
| Python Runtime       | Executes Python applications                    |
| `pip`                | Installs and manages Python packages            |
| `venv`               | Creates isolated Python environments            |
| `site-packages`      | Package installation location                   |
| Dependency           | Package required by another package/application |
| `pip freeze`         | Captures installed package versions             |
| `requirements.txt`   | Stores dependency specifications                |
| `.env`               | Stores local application configuration          |
| Environment Variable | Configuration supplied outside source code      |
| `python-dotenv`      | Loads `.env` values into the environment        |
| `.gitignore`         | Prevents selected files from Git tracking       |
| `psutil`             | Provides system/process information             |
| Automation Script    | Performs repeatable tasks programmatically      |

---

# 22. Step 16 Part 1 Summary

Step 16 established a Python environment suitable for DevOps-oriented work.

The final conceptual architecture is:

```text
Ubuntu Linux
     │
     ├── System Python
     │
     └── Python Project
            │
            ├── .venv
            │      └── Isolated Packages
            │
            ├── requirements.txt
            │      └── Dependency Specification
            │
            ├── .env
            │      └── Local Configuration
            │
            ├── .gitignore
            │
            └── src/
                   ├── app.py
                   └── system_info.py
```

The practical outcome is a small, reproducible Python environment that can be used for Linux/DevOps automation without turning the step into a full Python programming course.
# Step 16 — Python Environment

## Part 2 — Commands, Operations & Practical Reference

## 1. Environment Discovery Commands

### 1.1 Operating System

```bash
cat /etc/os-release
```

Used to display Linux distribution information.

Targeted information used during the lab:

```bash
cat /etc/os-release | grep -E '^(NAME|VERSION)='
```

| Command                       | Purpose                      |
| ----------------------------- | ---------------------------- |
| `cat /etc/os-release`         | Display OS identification    |
| `grep -E '^(NAME\|VERSION)='` | Extract only OS name/version |

---

## 2. Python Runtime Verification

### 2.1 Python Version

```bash
python3 --version
```

Shows the installed Python 3 version.

Lab result:

```text
Python 3.14.4
```

### 2.2 Python Executable Location

```bash
command -v python3
```

Lab result:

```text
/usr/bin/python3
```

### 2.3 Python Runtime Information

```bash
python3 -c 'import sys; print("Executable :", sys.executable); print("Version    :", sys.version); print("Prefix     :", sys.prefix); print("Base Prefix:", sys.base_prefix)'
```

Important attributes:

| Attribute         | Meaning                             |
| ----------------- | ----------------------------------- |
| `sys.executable`  | Python executable currently running |
| `sys.version`     | Detailed Python version information |
| `sys.prefix`      | Current Python environment prefix   |
| `sys.base_prefix` | Base Python installation prefix     |

---

## 3. pip Verification

### 3.1 pip Version

```bash
pip3 --version
```

Lab result:

```text
pip 25.1.1
```

### 3.2 pip Location

```bash
command -v pip3
```

System-level result before activating `.venv`:

```text
/usr/bin/pip3
```

Inside `.venv`, the equivalent verification was:

```bash
python -m pip --version
```

This showed that pip was being used from:

```text
.venv/lib/python3.14/site-packages/pip
```

### Recommended form

```bash
python -m pip <command>
```

This explicitly uses the pip associated with the selected Python interpreter.

---

# 4. Virtual Environment Commands

## 4.1 Create a Virtual Environment

```bash
python3 -m venv .venv
```

Creates:

```text
.venv/
```

### Components

| Component | Purpose                           |
| --------- | --------------------------------- |
| `python3` | Python runtime                    |
| `-m`      | Execute a Python module           |
| `venv`    | Python virtual-environment module |
| `.venv`   | Destination directory             |

---

## 4.2 Activate the Virtual Environment

```bash
source .venv/bin/activate
```

After activation:

```text
(.venv)
```

appears in the shell prompt.

### Effect

Before activation:

```text
python3 → /usr/bin/python3
```

After activation:

```text
python → .venv/bin/python
pip → .venv/bin/pip
```

---

## 4.3 Verify the Active Environment

```bash
printf "VIRTUAL_ENV=%s\n" "$VIRTUAL_ENV"
```

```bash
command -v python
```

```bash
python --version
```

```bash
python -m pip --version
```

```bash
python -c 'import sys; print("Executable :", sys.executable); print("Prefix     :", sys.prefix); print("Base Prefix:", sys.base_prefix)'
```

### Important comparison

| System               | Virtual Environment |
| -------------------- | ------------------- |
| `/usr/bin/python3`   | `.venv/bin/python`  |
| System prefix `/usr` | `.venv` prefix      |
| System pip           | `.venv` pip         |
| System packages      | Project packages    |

---

# 5. Virtual Environment Filesystem Inspection

The environment was inspected manually with:

```bash
ls -la .venv
```

```bash
ls -la .venv/bin
```

```bash
ls -la .venv/lib
```

```bash
ls -la .venv/lib/python3.14
```

```bash
ls -la .venv/lib/python3.14/site-packages
```

Important directories:

```text
.venv/
├── bin/
├── include/
├── lib/
├── lib64
└── pyvenv.cfg
```

Package installation location:

```text
.venv/lib/python3.14/site-packages/
```

---

# 6. pip Package Inspection

## 6.1 List Installed Packages

```bash
python -m pip list
```

Provides a human-readable package list.

---

## 6.2 Freeze Installed Dependencies

```bash
python -m pip freeze
```

Produces:

```text
package==version
```

format.

Example from the lab:

```text
requests==2.34.2
psutil==7.2.2
python-dotenv==1.2.2
```

---

## 6.3 Inspect a Specific Package

```bash
python -m pip show requests
```

Useful information includes:

* Name
* Version
* Location
* Dependencies
* Metadata

---

## 6.4 Show Package Files

```bash
python -m pip show -f requests
```

The `-f` option displays files installed by the package.

---

# 7. Installing Packages

The lab initially tested:

```bash
python -m pip install requests
```

Later the actual DevOps environment used:

```bash
python -m pip install requests python-dotenv psutil
```

### Important behavior

Installing `requests` automatically installed its required dependencies:

```text
requests
├── certifi
├── charset-normalizer
├── idna
└── urllib3
```

---

# 8. Uninstalling Packages

```bash
python -m pip uninstall requests
```

For non-interactive removal:

```bash
python -m pip uninstall -y certifi charset-normalizer idna urllib3
```

### Important observation

Uninstalling:

```bash
pip uninstall requests
```

removed `requests`, but its dependencies remained.

This demonstrates that package removal does not automatically mean removing every dependency associated with that package.

---

# 9. Python Package Import Verification

A package can be verified directly from Python.

```bash
python -c 'import requests; print("requests version:", requests.__version__); print("requests location:", requests.__file__)'
```

Dependency verification used:

```bash
python -c 'import certifi, charset_normalizer, idna, urllib3; print("certifi           :", certifi.__version__); print("charset-normalizer:", charset_normalizer.__version__); print("idna              :", idna.__version__); print("urllib3            :", urllib3.__version__)'
```

After uninstalling `requests`, the import test produced:

```text
ModuleNotFoundError: No module named 'requests'
```

This was expected because the package had been removed.

---

# 10. Creating `requirements.txt`

The lab generated the dependency file using:

```bash
python -m pip freeze > requirements.txt
```

The `>` operator redirects command output into a file.

### Verify the file

```bash
ls -lh requirements.txt
```

```bash
cat requirements.txt
```

### Purpose

```text
pip freeze
     ↓
Current dependency state
     ↓
requirements.txt
```

---

# 11. Installing From `requirements.txt`

A new environment was created:

```bash
python3 -m venv .venv-recreated
```

Packages were then installed without manually naming individual packages:

```bash
.venv-recreated/bin/python -m pip install -r requirements.txt
```

### Important option

| Option | Meaning                               |
| ------ | ------------------------------------- |
| `-r`   | Read package requirements from a file |

This recreated the same package versions in the new environment.

---

# 12. Environment Reproducibility Verification

The recreated environment was inspected using:

```bash
.venv-recreated/bin/python -m pip list
```

```bash
.venv-recreated/bin/python -m pip freeze
```

The output matched the original dependency set.

This verified:

```text
Environment A
     ↓
requirements.txt
     ↓
Environment B
     ↓
Same dependency versions
```

---

# 13. Environment Variables

The lab created:

```text
APP_ENV=development
APP_NAME=devops-python-lab
APP_PORT=5000
```

The shell variable can be inspected using:

```bash
printf "%s\n" "$VIRTUAL_ENV"
```

The Python application reads configuration using:

```python
os.getenv("APP_NAME", "default-app")
os.getenv("APP_ENV", "production")
os.getenv("APP_PORT", "8000")
```

### `os.getenv()` pattern

```text
os.getenv(NAME, DEFAULT)
```

| Component | Meaning                               |
| --------- | ------------------------------------- |
| `NAME`    | Environment variable name             |
| `DEFAULT` | Value used if variable is unavailable |

---

# 14. Loading `.env`

The project uses:

```python
from dotenv import load_dotenv

load_dotenv()
```

This loads values from `.env` into the environment available to the Python application.

The configuration was then read with:

```python
os.getenv(...)
```

---

# 15. Python Project Creation Commands

The source directory was created using:

```bash
mkdir -p src
```

### `-p`

Creates the directory if necessary and avoids an error when the directory already exists.

---

# 16. Project File Inspection

Project contents were inspected using:

```bash
ls -la
```

```bash
find . -maxdepth 2 -print | sort
```

Python source files:

```bash
find src -maxdepth 1 -type f -name "*.py" -print
```

The final source files were:

```text
src/app.py
src/system_info.py
```

---

# 17. `.gitignore`

The project-level `.gitignore` contains:

```text
.venv/
.env
__pycache__/
*.py[cod]
```

### Rules

| Pattern        | Purpose                                |
| -------------- | -------------------------------------- |
| `.venv/`       | Ignore local virtual environment       |
| `.env`         | Ignore local configuration/secrets     |
| `__pycache__/` | Ignore Python bytecode cache           |
| `*.py[cod]`    | Ignore generated Python bytecode files |

---

# 18. Git Ignore Verification

The following commands were attempted:

```bash
git status --short --ignored
```

```bash
git check-ignore -v .venv/bin/python
```

```bash
git check-ignore -v .env
```

However, the project was not yet a Git repository.

Result:

```text
fatal: not a git repository
```

This was not treated as a Python environment failure.

Git repository initialization and final Git verification belong to **Step 16.8**.

---

# 19. Running Python Scripts

Application configuration test:

```bash
python src/app.py
```

DevOps automation:

```bash
python src/system_info.py
```

The automation script collected:

* Hostname
* Operating system
* Python version
* CPU cores
* Memory information
* Disk information
* Working directory

---

# 20. Python System Information Modules

The automation script used:

```python
import os
import platform
import shutil
import socket
import sys

import psutil
```

### Module Reference

| Module     | Usage                                                    |
| ---------- | -------------------------------------------------------- |
| `os`       | Operating-system interface, CPU count, working directory |
| `platform` | OS and runtime information                               |
| `shutil`   | Disk usage information                                   |
| `socket`   | Hostname                                                 |
| `sys`      | Python runtime information                               |
| `psutil`   | Memory/system information                                |

---

# 21. Final Verification Commands

The final environment was verified with:

```bash
printf "VIRTUAL_ENV=%s\n" "$VIRTUAL_ENV"
```

```bash
python --version
```

```bash
command -v python
```

```bash
python -m pip --version
```

```bash
python -c 'import sys; print("Executable :", sys.executable); print("Prefix     :", sys.prefix); print("Base Prefix:", sys.base_prefix)'
```

Installed packages:

```bash
python -m pip list
```

Dependency specification:

```bash
cat requirements.txt
```

Dependency consistency:

```bash
python -m pip freeze > /tmp/step16-freeze.txt
diff -u requirements.txt /tmp/step16-freeze.txt
rm -f /tmp/step16-freeze.txt
```

Successful comparison produced:

```text
requirements.txt matches installed packages.
```

Application verification:

```bash
python src/app.py
```

Automation verification:

```bash
python src/system_info.py
```

---

# 22. Useful Command Variations

| Task                        | Command                                     |
| --------------------------- | ------------------------------------------- |
| Python version              | `python --version`                          |
| Python location             | `command -v python`                         |
| pip version                 | `python -m pip --version`                   |
| List packages               | `python -m pip list`                        |
| Freeze packages             | `python -m pip freeze`                      |
| Install package             | `python -m pip install PACKAGE`             |
| Install multiple packages   | `python -m pip install PACKAGE1 PACKAGE2`   |
| Install from file           | `python -m pip install -r requirements.txt` |
| Show package information    | `python -m pip show PACKAGE`                |
| Show package files          | `python -m pip show -f PACKAGE`             |
| Remove package              | `python -m pip uninstall PACKAGE`           |
| Remove without confirmation | `python -m pip uninstall -y PACKAGE`        |
| Create venv                 | `python3 -m venv .venv`                     |
| Activate venv               | `source .venv/bin/activate`                 |
| Create directory            | `mkdir -p DIRECTORY`                        |
| List hidden files           | `ls -la`                                    |
| Display file                | `cat FILE`                                  |
| Search/filter output        | `grep -E 'PATTERN'`                         |
| Compare files               | `diff -u FILE1 FILE2`                       |

---

# 23. Important Options and Flags

| Flag/Option | Command            | Meaning                             |
| ----------- | ------------------ | ----------------------------------- |
| `-m`        | `python -m pip`    | Run a Python module                 |
| `--version` | `python --version` | Display version                     |
| `-r`        | `pip install -r`   | Read requirements from file         |
| `-f`        | `pip show -f`      | Show installed package files        |
| `-y`        | `pip uninstall -y` | Automatically confirm uninstall     |
| `-p`        | `mkdir -p`         | Create parent directories if needed |
| `-a`        | `ls -la`           | Include hidden files                |
| `-l`        | `ls -la`           | Long listing format                 |
| `-E`        | `grep -E`          | Use extended regular expressions    |
| `-u`        | `diff -u`          | Unified diff format                 |
| `-maxdepth` | `find`             | Limit directory traversal depth     |
| `-type f`   | `find`             | Select regular files                |
| `-name`     | `find`             | Match filename pattern              |

---

# 24. Configuration Files and Important Paths

| Path/File                             | Purpose                                          |
| ------------------------------------- | ------------------------------------------------ |
| `/usr/bin/python3`                    | System Python executable observed during the lab |
| `/usr/bin/pip3`                       | System pip executable observed during discovery  |
| `.venv/`                              | Project virtual environment                      |
| `.venv/bin/python`                    | Virtual-environment Python                       |
| `.venv/bin/pip`                       | Virtual-environment pip                          |
| `.venv/lib/python3.14/site-packages/` | Installed Python packages                        |
| `.venv/pyvenv.cfg`                    | Virtual-environment configuration                |
| `.env`                                | Local application configuration                  |
| `.gitignore`                          | Git exclusion rules                              |
| `requirements.txt`                    | Python dependency specification                  |
| `src/app.py`                          | Environment-configuration example                |
| `src/system_info.py`                  | DevOps automation script                         |

---

# 25. Common Mistakes

| Mistake                                         | Problem                                            | Better Practice                             |
| ----------------------------------------------- | -------------------------------------------------- | ------------------------------------------- |
| Installing packages directly into system Python | Can affect system/project dependencies             | Use `.venv`                                 |
| Forgetting to activate `.venv`                  | Package may install into another environment       | Check `VIRTUAL_ENV` and `command -v python` |
| Using the wrong pip                             | Package may be installed for another Python        | Prefer `python -m pip`                      |
| Committing `.venv/`                             | Large local environment enters repository          | Add `.venv/` to `.gitignore`                |
| Committing `.env`                               | Secrets/configuration may be exposed               | Add `.env` to `.gitignore`                  |
| Forgetting dependencies                         | Recreated environment may fail                     | Maintain `requirements.txt`                 |
| Manually reinstalling every package             | Error-prone and inefficient                        | Use `pip install -r requirements.txt`       |
| Assuming uninstall removes every dependency     | Other packages may still require them              | Inspect dependency relationships            |
| Hardcoding environment-specific values          | Same code becomes difficult to deploy              | Use environment variables                   |
| Treating `pip list` as a requirements file      | Output is not intended as dependency specification | Use `pip freeze`                            |

---

# 26. Real-World DevOps Uses

### Virtual Environments

Used for:

* Backend services
* Automation scripts
* CI/CD utilities
* Internal DevOps tools
* API integration tools

### `requirements.txt`

Used for:

* Server deployments
* CI pipelines
* Development environment setup
* Reproducible builds
* Team collaboration

### Environment Variables

Used for:

* Database connection strings
* API endpoints
* Ports
* Environment names
* Credentials and secrets through appropriate secret-management mechanisms

### Python Automation

Used for:

* Server health checks
* Log processing
* API automation
* Deployment helpers
* Cloud automation
* Monitoring utilities
* File/system administration

---

# 27. Interview Questions

### Python Environment

1. What is a Python virtual environment?
2. Why should you avoid installing project dependencies directly into system Python?
3. What is the purpose of `venv`?
4. How do you create a virtual environment?
5. How do you activate a virtual environment?
6. How can you verify which Python executable is active?
7. What is `site-packages`?

### pip

8. What is pip?
9. What is the difference between `pip list` and `pip freeze`?
10. Why is `python -m pip` often preferable to calling `pip` directly?
11. How do you inspect a package with pip?
12. How do you uninstall a package?

### Dependencies

13. What is a dependency?
14. What is a transitive dependency?
15. Why can installing one package install several other packages?
16. Why does uninstalling a package not necessarily remove its dependencies?

### `requirements.txt`

17. What is `requirements.txt`?
18. How do you generate it?
19. How do you install dependencies from it?
20. Why is it important for reproducible environments?

### Environment Variables

21. Why should configuration be separated from application code?
22. What is an environment variable?
23. What is `.env` used for?
24. What does `os.getenv()` do?
25. Why should `.env` usually be excluded from Git?

### DevOps Automation

26. Why is Python useful for DevOps?
27. How can Python interact with Linux?
28. What is `psutil`?
29. Give examples of DevOps tasks that can be automated using Python.
30. How could the system-information script be extended into a server health-check utility?

---

# 28. Command Workflow Summary

```text
Python Discovery
    ↓
python3 --version
command -v python3
python3 -c ...
    ↓
pip Verification
    ↓
python3 -m venv .venv
    ↓
source .venv/bin/activate
    ↓
python -m pip install ...
    ↓
python -m pip list
python -m pip show ...
python -m pip freeze
    ↓
pip freeze > requirements.txt
    ↓
python3 -m venv .venv-recreated
    ↓
python -m pip install -r requirements.txt
    ↓
Environment Variables
    ↓
.env + python-dotenv + os.getenv()
    ↓
Python Automation
    ↓
python src/system_info.py
    ↓
Final Verification
```

# 29. Part 2 Summary

Part 2 records the practical command knowledge developed during Step 16.

The key operational pattern is:

```text
Inspect
  ↓
Isolate
  ↓
Install
  ↓
Inspect
  ↓
Record
  ↓
Reproduce
  ↓
Configure
  ↓
Automate
  ↓
Verify
```

This workflow is directly applicable to Python-based DevOps automation, backend environments, CI/CD utilities and server-side scripting.
```markdown
# Step 16 — Python Environment

## Part 3 — Real Lab Documentation

## 1. Skills Gained

| Skill | Practical Result |
|---|---|
| Python Environment Discovery | Verified the existing Python environment |
| Python Runtime Management | Verified Python 3.14.4 |
| pip Management | Installed, inspected and removed Python packages |
| Virtual Environment | Created and used `.venv` |
| Package Isolation | Verified packages inside the project virtual environment |
| Dependency Management | Managed project dependencies using pip |
| requirements.txt | Created and used dependency specification |
| Environment Reproduction | Recreated the Python environment from `requirements.txt` |
| Environment Variables | Used `.env` with `python-dotenv` |
| Python Project Structure | Created a small DevOps-oriented project structure |
| Git Ignore Configuration | Added `.venv`, `.env` and Python cache rules |
| Python Automation | Created a system-information automation script |
| Final Verification | Verified runtime, dependencies, configuration and scripts |

---

## 2. Real Lab Summary

Step 16 was completed as a supplementary DevOps Python environment lab.

The lab focused on preparing Python for practical DevOps work rather than learning Python programming from the beginning.

### Practical Workflow

```text
Python Environment Discovery
        ↓
Virtual Environment
        ↓
pip Package Management
        ↓
Dependency Management
        ↓
requirements.txt
        ↓
Environment Reproduction
        ↓
Environment Variables
        ↓
Python Project Structure
        ↓
DevOps Automation
        ↓
Final Verification
```

---

## 3. Real Machine Information

| Item | Observed Value |
|---|---|
| Operating System | Ubuntu 26.04 LTS |
| Python Version | 3.14.4 |
| System Python | `/usr/bin/python3` |
| pip Version | 25.1.1 |
| Hostname | `afroza-VirtualBox` |
| CPU Cores | 2 |
| Total Memory | 3.32 GB |
| Memory Used During Final Verification | 1.75 GB |
| Memory Usage During Final Verification | 52.7% |
| Total Disk | 58.84 GB |
| Disk Used During Final Verification | 22.34 GB |
| Disk Usage During Final Verification | 37.96% |
| Project Directory | `/home/afroza/Projects/devops-supplementary-labs/Step-16-Python-Environment` |
| Active Virtual Environment | `.venv` |

---

## 4. Python Environment

### Final Environment

```text
Python Version : 3.14.4

Executable :
/home/afroza/Projects/devops-supplementary-labs/Step-16-Python-Environment/.venv/bin/python

Virtual Environment Prefix :
/home/afroza/Projects/devops-supplementary-labs/Step-16-Python-Environment/.venv

Base Prefix :
/usr
```

### Environment Variable

```text
VIRTUAL_ENV=/home/afroza/Projects/devops-supplementary-labs/Step-16-Python-Environment/.venv
```

---

## 5. Installed Packages

| Package | Version | Purpose |
|---|---:|---|
| pip | 25.1.1 | Python package management |
| requests | 2.34.2 | HTTP/API communication |
| python-dotenv | 1.2.2 | `.env` configuration loading |
| psutil | 7.2.2 | System information |
| certifi | 2026.7.22 | Certificate support |
| charset-normalizer | 3.4.9 | Character encoding support |
| idna | 3.18 | Internationalized domain names |
| urllib3 | 2.7.0 | HTTP connection functionality |

---

## 6. requirements.txt

The final `requirements.txt` contains:

```text
certifi==2026.7.22
charset-normalizer==3.4.9
idna==3.18
psutil==7.2.2
python-dotenv==1.2.2
requests==2.34.2
urllib3==2.7.0
```

### Final Verification

```text
requirements.txt matches installed packages.
```

The installed dependency state and `requirements.txt` were successfully verified as matching.

---

## 7. Environment Variables Used

The actual `.env` file contains:

```text
APP_ENV=development
APP_NAME=devops-python-lab
APP_PORT=5000
```

These values are read by `src/app.py`.

### Application Output

```text
Application : devops-python-lab
Environment : development
Port        : 5000
```

---

## 8. Python Source Files

### `src/app.py`

Purpose:

- Load `.env`
- Read environment variables
- Use default values when necessary
- Display application configuration

### `src/system_info.py`

Purpose:

- Collect Linux system information
- Demonstrate Python-based DevOps automation
- Read system information programmatically

Information collected:

```text
Hostname
Operating System
Python Version
CPU Cores
Memory Information
Disk Information
Working Directory
```

---

## 9. Actual Automation Result

Final execution of `system_info.py` produced:

```text
Hostname       : afroza-VirtualBox
Operating System: Linux 7.0.0-29-generic
Python Version : 3.14.4
CPU Cores      : 2
Memory Total   : 3.32 GB
Memory Used    : 1.75 GB
Memory Usage   : 52.7%
Disk Total     : 58.84 GB
Disk Used      : 22.34 GB
Disk Usage     : 37.96%
Working Dir    : /home/afroza/Projects/devops-supplementary-labs/Step-16-Python-Environment
```

These values were collected from the actual Linux environment by Python.

---

## 10. Important Commands Used

### Environment Discovery

```bash
cat /etc/os-release | grep -E '^(NAME|VERSION)='
python3 --version
command -v python3
python3 -c 'import sys; print("Executable :", sys.executable); print("Version    :", sys.version); print("Prefix     :", sys.prefix); print("Base Prefix:", sys.base_prefix)'
pip3 --version
command -v pip3
```

### Virtual Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### Environment Verification

```bash
printf "VIRTUAL_ENV=%s\n" "$VIRTUAL_ENV"
python --version
command -v python
python -m pip --version
python -c 'import sys; print("Executable :", sys.executable); print("Prefix     :", sys.prefix); print("Base Prefix:", sys.base_prefix)'
```

### Package Management

```bash
python -m pip list
python -m pip freeze
python -m pip show requests
python -m pip show -f requests
python -m pip install requests
python -m pip install requests python-dotenv psutil
python -m pip uninstall requests
```

### Dependency Management

```bash
python -m pip freeze > requirements.txt
cat requirements.txt
python3 -m venv .venv-recreated
.venv-recreated/bin/python -m pip install -r requirements.txt
```

### Project Structure

```bash
mkdir -p src
ls -la
find . -maxdepth 2 -print | sort
find src -maxdepth 1 -type f -name "*.py" -print
```

### Python Applications

```bash
python src/app.py
python src/system_info.py
```

---

## 11. Important Files

| File/Directory | Purpose |
|---|---|
| `.venv/` | Active Python virtual environment |
| `.env` | Local application configuration |
| `.gitignore` | Git exclusion rules |
| `requirements.txt` | Dependency specification |
| `src/` | Python source directory |
| `src/app.py` | Environment configuration example |
| `src/system_info.py` | DevOps automation script |

---

## 12. Final Folder Structure

```text
Step-16-Python-Environment/
├── .env
├── .gitignore
├── .venv/
├── requirements.txt
└── src/
    ├── app.py
    └── system_info.py
```

The temporary `.venv-recreated/` environment was removed after the reproducibility test.

---

## 13. Git Ignore Configuration

The project-level `.gitignore` contains:

```text
.venv/
.env
__pycache__/
*.py[cod]
```

| Rule | Purpose |
|---|---|
| `.venv/` | Ignore local virtual environment |
| `.env` | Ignore local configuration |
| `__pycache__/` | Ignore Python cache |
| `*.py[cod]` | Ignore generated Python bytecode |

Git verification was not completed at this stage because the project directory was not yet a Git repository.

Git initialization and final Git verification will be performed in the Git workflow.

---

## 14. Services Used

Step 16 did not create or configure a dedicated systemd service.

| Component | Usage |
|---|---|
| Python Runtime | Used |
| pip | Used |
| Python `venv` | Used |
| `.env` Configuration | Used |
| Linux System Information | Read by Python |
| systemd Service | Not created |

---

## 15. Variables Used

### Environment Variables

```text
APP_ENV=development
APP_NAME=devops-python-lab
APP_PORT=5000
```

### Python Application Variables

```text
app_name
app_env
app_port
```

### Automation Information

```text
hostname
operating system
Python version
CPU count
memory usage
disk usage
working directory
```

---

## 16. Observation and Verification Summary

| Area | Verification Result |
|---|---|
| Python Runtime | Python 3.14.4 verified |
| Python Executable | `.venv/bin/python` verified |
| Virtual Environment | Active and functional |
| pip | 25.1.1 verified |
| Package Installation | Successful |
| Package Removal | Successfully verified |
| Package Isolation | Verified inside `.venv` |
| requirements.txt | Created successfully |
| Environment Reproduction | Successfully tested |
| Environment Variables | Successfully loaded |
| Python Project Structure | Created successfully |
| DevOps Automation | Successfully executed |
| Dependency Consistency | Exact match |
| Temporary Environment | Removed |
| Final Environment | Clean and functional |

---

## 17. Key Takeaways

1. Inspect the existing Python environment before making changes.
2. Use a virtual environment for project-specific dependencies.
3. Prefer `python -m pip` to associate pip with the active Python interpreter.
4. Use `pip list` for package inspection.
5. Use `pip freeze` for dependency capture.
6. Use `requirements.txt` for reproducible environments.
7. Keep application configuration outside source code.
8. Keep `.env` outside Git.
9. Use Python to automate repetitive Linux/DevOps tasks.
10. Verify the environment before documenting and committing it.

---

## 18. Step 16 Final Outcome

Step 16 established a working Python environment suitable for DevOps-oriented automation.

The final environment contains:

```text
Python Runtime
      +
Virtual Environment
      +
pip Package Management
      +
requirements.txt
      +
Environment Variables
      +
Python Project Structure
      +
Linux System Automation
      +
Final Verification
```

The Python environment is ready for future DevOps automation work.

---

## 19. Next Step Preview

```text
Step 16 — Python Environment
        ↓
Completed

Next
Step 17 — PostgreSQL
        ↓
Supplementary Database Administration Lab
```
