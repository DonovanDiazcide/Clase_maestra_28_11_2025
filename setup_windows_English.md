# Complete Installation Guide for Windows 2025: Git, Python, VS Code, oTree, and SSH

This guide verified with official 2025 sources provides precise instructions for setting up a complete development environment on **Windows 10 and 11**. Each step includes direct quotes from official documentation, exact commands, and solutions to common problems organized by frequency.

---

## 1. Git on Windows

Git is the version control system that allows you to manage code and collaborate with GitHub. The current version is **Git 2.52.0** (November 2025).

### COMMON Case: Standard Installation

**Step 1: Download the official installer**

Visit https://git-scm.com/download/win and download the 64-bit installer. According to the official documentation:

> "Click here to download the latest (2.52.0) x64 version of Git for Windows. This is the most recent maintained build."
> — *Source: git-scm.com/download/win*

**Step 2: Run the installer with recommended options**

During installation, you'll see several configuration screens. The **critical** options are:

| Screen | Recommended option | Why |
|--------|-------------------|-----|
| **Select Components** | ✅ Keep default values | Includes Git Bash and Explorer integration |
| **Default Editor** | Visual Studio Code or Notepad++ | Vim can be difficult for beginners |
| **PATH environment** | ✅ "Git from the command line and also from 3rd-party software" | **CRITICAL**: Allows using `git` from any terminal |
| **SSH executable** | ✅ "Use bundled OpenSSH" | Avoids conflicts with other versions |
| **Line ending conversions** | ✅ "Checkout Windows-style, commit Unix-style" | Automatically handles CRLF/LF differences |
| **Terminal emulator** | ✅ "Use MinTTY" | Modern terminal with better support |

About the PATH option, the documentation states:

> "The PATH is the default set of directories included when you run a command from the command line. Keep the middle (recommended) selection."
> — *Source: git-scm.com/book/en/v2/Getting-Started-Installing-Git*

About line endings (CRLF):

> "If you're on a Windows machine, set it to true — this converts LF endings into CRLF when you check out code."
> — *Source: git-scm.com/book/en/v2/Customizing-Git-Git-Configuration*

**Step 3: Verify the installation**

Open a **new** terminal (Command Prompt, PowerShell, or Git Bash) and run:

```cmd
git --version
```

Expected output: `git version 2.52.0.windows.1`

**Step 4: Required initial configuration**

According to the official Pro Git book:

> "The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information."
> — *Source: git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup*

Run these commands (replace with your information):

```bash
git config --global user.name "Your Full Name"
git config --global user.email "your@email.com"
git config --global init.defaultBranch main
```

To verify the configuration:

```bash
git config --list
```

### Git Bash vs Command Prompt vs PowerShell

Git for Windows documentation explains:

> "Git for Windows provides a BASH emulation used to run Git from the command line. *NIX users should feel right at home, as the BASH emulation behaves just like the 'git' command in LINUX and UNIX environments."
> — *Source: gitforwindows.org*

| Terminal | When to use it |
|----------|---------------|
| **Git Bash** | ✅ Recommended for SSH and scripts. Compatible with Linux/Mac tutorials |
| **PowerShell** | Good for users who prefer native Windows environment |
| **CMD** | Functional but with fewer features |

**Important difference in paths:**
- Git Bash: `cd /c/Users/name/projects`
- CMD/PowerShell: `cd C:\Users\name\projects`

### MODERATELY COMMON Case: Common Problems

**Problem: "git is not recognized as a command"**

Cause: The terminal was opened before installing Git or PATH wasn't configured.

Solutions:
1. **Close and reopen** the terminal (required after installation)
2. If it persists, manually add to PATH:
   - Win + R → `sysdm.cpl` → Advanced → Environment Variables
   - Edit `Path` in System Variables
   - Add: `C:\Program Files\Git\cmd`

**Problem: Git extremely slow**

Cause: Windows Defender scans every file.

Solution with PowerShell (run as administrator):
```powershell
Add-MpPreference -ExclusionPath "C:\Program Files\Git"
Add-MpPreference -ExclusionPath "C:\Users\YourUser\projects"
Add-MpPreference -ExclusionProcess "git.exe"
```

**Problem: Warning "LF will be replaced by CRLF"**

This warning is **normal** and indicates that Git is correctly converting line endings. To silence it, confirm you have:

```bash
git config --global core.autocrlf true
```

---

## 2. Python on Windows

For oTree, you need **Python 3.11.x** specifically. The most recent version with binary installers is **Python 3.11.9**.

### ⚠️ Critical warning about versions

The oTree source code explicitly specifies:

> "Error: This version of oTree is only compatible with these Python versions: 3.7, 3.8, 3.9, 3.10, 3.11"
> — *Source: github.com/oTree-org/otree-core/blob/master/setup.py*

**DO NOT install Python 3.12 or higher** if you're going to use oTree.

### COMMON Case: Python 3.11.9 Installation

**Step 1: Download the correct installer**

Download from: https://www.python.org/ftp/python/3.11.9/python-3.11.9-amd64.exe

According to python.org:

> "Python 3.11.9 was the last full bugfix release of Python 3.11 with binary installers."
> — *Source: python.org/downloads/release/python-3119/*

**Step 2: Install with "Add Python to PATH" enabled**

When running the installer, on the **first screen** you'll see two crucial options at the bottom:

**✅ MANDATORY CHECK**: "Add python.exe to PATH"

According to the official documentation:

> "When you first install a runtime, you will likely be prompted to add a directory to your PATH. This is optional, if you prefer to use the py command, but is offered for those who prefer the full range of aliases."
> — *Source: docs.python.org/3/using/windows.html*

**What happens if you DON'T check this option?**
- The `python` command won't work from any terminal
- You'll see the error: `'python' is not recognized as an internal or external command`
- Or Windows will open the Microsoft Store offering to install another version

Installation options:
| Option | Meaning |
|--------|---------|
| "Install Now" | Installs for current user with standard configuration |
| "Customize installation" | Allows choosing location and additional options |

For most users, **"Install Now"** with PATH enabled is sufficient.

**Step 3: Verify the installation**

**IMPORTANT**: Close and reopen the terminal before verifying.

```cmd
python --version
```
Expected output: `Python 3.11.9`

```cmd
pip --version
```
Expected output: `pip 24.x.x from ... (python 3.11)`

### MODERATELY COMMON Case: Python Problems

**Problem: "python was not found" or opens Microsoft Store**

Cause: Microsoft Store aliases active competing with your installation.

Solution according to the documentation:

> "Click Start, open 'Manage app execution aliases', and check that the aliases for 'Python (default)' are enabled. If they already are, try disabling and re-enabling."
> — *Source: docs.python.org/3/using/windows.html*

Steps:
1. Settings → Apps → App execution aliases
2. **Disable** the "python.exe" and "python3.exe" aliases from "App Installer"

**Problem: Add Python to PATH manually (if forgotten)**

1. Win + R → `sysdm.cpl` → "Advanced" tab
2. Click "Environment Variables"
3. In "User variables", select "Path" → "Edit"
4. Add these two paths (adjust according to your user):
   ```
   C:\Users\YourUser\AppData\Local\Programs\Python\Python311\
   C:\Users\YourUser\AppData\Local\Programs\Python\Python311\Scripts\
   ```
5. Accept and **close all terminals** before testing

**Problem: Multiple Python versions installed**

Use the Python Launcher to specify version:
```cmd
py -3.11 -m venv my_environment
py -3.11 script.py
```

To see all installed versions:
```cmd
py --list
```

---

## 3. Visual Studio Code on Windows

VS Code is the recommended code editor for development with Python and oTree.

### COMMON Case: Standard Installation

**Step 1: Download the correct installer**

Download from: https://code.visualstudio.com/download

According to Microsoft's official documentation:

> "**User setup:** Does not require administrator privileges to run, as the location is under your user Local AppData folder. **This is the preferred way to install VS Code on Windows.**"
> — *Source: code.visualstudio.com/docs/setup/windows*

Choose **"User Installer x64"** (recommended) instead of the System Installer.

**Step 2: Important installation options**

During installation, **check these options**:

- ✅ **"Add to PATH"** — Allows using the `code` command from terminal
- ✅ "Register Code as an editor for supported file types"
- ✅ "Add 'Open with Code' action to Windows Explorer context menu"

About the PATH option, the documentation states:

> "Setup adds Visual Studio Code to your %PATH% environment variable, to let you type 'code .' in the console to open VS Code on that folder. You need to restart your console after the installation for the change to the %PATH% environmental variable to take effect."
> — *Source: code.visualstudio.com/docs/setup/windows*

**Step 3: Verify the installation**

Close and open a new terminal:
```cmd
code --version
```

To open VS Code in the current folder:
```cmd
code .
```

**Step 4: Install the Python extension**

Microsoft's official Python extension is essential. According to the documentation:

> "The Python extension will automatically install the following extensions by default: Pylance (ms-python.vscode-pylance), Python Debugger (ms-python.debugpy), Python Environments (ms-python.vscode-python-envs)"
> — *Source: marketplace.visualstudio.com/items?itemName=ms-python.python*

**Installation from command line:**
```cmd
code --install-extension ms-python.python
```

**Installation from VS Code:**
1. Press `Ctrl+Shift+X` to open extensions
2. Search "Python"
3. Install Microsoft's extension (ID: `ms-python.python`)

**Additional recommended extensions:**

| Extension | ID | Purpose |
|-----------|---|---------|
| Pylance | `ms-python.vscode-pylance` | Intelligent autocompletion |
| Python Debugger | `ms-python.debugpy` | Debugging |
| Jupyter | `ms-toolsai.jupyter` | Notebooks (optional) |

### Selecting the correct Python interpreter

According to the documentation:

> "The Python extension automatically detects Python interpreters that are installed in standard locations. It also detects virtual environments in the workspace folder."
> — *Source: code.visualstudio.com/docs/languages/python*

To select manually:
1. Press `Ctrl+Shift+P`
2. Type "Python: Select Interpreter"
3. Choose your Python 3.11 installation or virtual environment

### MODERATELY COMMON Case: VS Code Problems

**Problem: Windows Defender SmartScreen blocks installation**

Solution:
1. Click "More info"
2. Click "Run anyway"

The official VS Code installer is digitally signed by Microsoft and is safe.

**Problem: The `code` command doesn't work**

The documentation states:

> "'code' is not recognized as an internal or external command: Your OS cannot find the VS Code binary on its path. Try uninstalling and reinstalling VS Code."
> — *Source: code.visualstudio.com/docs/configure/command-line*

Manual solution: Add to PATH the path:
```
C:\Users\YourUser\AppData\Local\Programs\Microsoft VS Code\bin
```

---

## 4. oTree on Windows

oTree is the framework for creating economic experiments. It's installed via pip within a virtual environment.

### COMMON Case: oTree Installation

**Prerequisite**: Python 3.11.x installed and working.

**Step 1: Create folder for the project**
```cmd
mkdir my_otree_project
cd my_otree_project
```

**Step 2: Create virtual environment**
```cmd
python -m venv venv
```

**Step 3: Activate virtual environment**

In CMD:
```cmd
venv\Scripts\activate.bat
```

In PowerShell:
```powershell
.\venv\Scripts\Activate.ps1
```

You'll know it's active because `(venv)` appears at the beginning of the prompt.

**Step 4: Install oTree**
```cmd
pip install otree
```

According to the official documentation:

> "pip3 install -U otree"
> — *Source: otree.readthedocs.io/en/latest/install-nostudio.html*

**Windows Note**: On Windows use `pip` instead of `pip3`:

> "Hi awallen, just use pip instead. pip3 is typically used for Mac OS."
> — *Source: otreehub.com/forum/1030/*

**Step 5: Verify installation**
```cmd
otree --version
```

**Step 6: Create oTree project**
```cmd
otree startproject project_name
```

The command will ask if you want to include sample games (answer "y" or "n").

**Step 7: Run development server**
```cmd
cd project_name
otree devserver
```

Open your browser at: **http://localhost:8000**

You should see the oTree admin interface.

### Main oTree commands

| Command | Description |
|---------|-------------|
| `otree devserver` | Development server (local access only) |
| `otree prodserver` | Production server (network access) |
| `otree startproject name` | Create new project |
| `otree startapp name` | Create new app within project |
| `otree resetdb` | Reset database |
| `otree --version` | View installed version |

### Difference devserver vs prodserver

> "With otree devserver, I can only access oTree via http://localhost:8000. If I want to access oTree from other computers, I have to use otree prodserver."
> — *Source: BonnEconLab, otreehub.com/forum/1337/*

### MODERATELY COMMON Case: oTree Problems

**Problem: Python version error**

If you see this error:
```
Error: This version of oTree is only compatible with these Python versions: 3.7, 3.8, 3.9, 3.10, 3.11
```

Solution: Install Python 3.11.9 and create a new virtual environment:
```cmd
py -3.11 -m venv venv_otree
venv_otree\Scripts\activate
pip install otree
```

**Problem: Port 8000 in use**

> "This means that there is another server process already running on that port."
> — *Source: groups.google.com/g/otree*

Solutions:
1. Close all terminals and retry
2. Find the process:
   ```cmd
   netstat -ano | findstr :8000
   ```
3. Kill the process (replace PID with the number):
   ```cmd
   taskkill /PID [pid_number] /F
   ```

**Problem: Port blocked by firewall**

Open port in Windows Firewall (run as administrator):
```powershell
netsh advfirewall firewall add rule name="oTree Port 8000" dir=in action=allow protocol=TCP localport=8000
```

**Problem: Python freezes with PowerShell**

> "If Python crashes when you run this command, and you're using PowerShell, try using CMD instead."
> — *Source: ibsen-otree-docs.ibsen-h2020.eu/install.html*

**Problem: Special characters in paths**

Avoid folders with spaces, accents, or special characters:
- ✅ Correct: `C:\Users\John\projects\otree`
- ❌ Avoid: `C:\Users\José María\my projects\oTree experiment`

---

## 5. SSH with GitHub on Windows

SSH allows you to connect securely to GitHub without typing your password each time.

### COMMON Case: Complete SSH Configuration

GitHub recommends using **Git Bash** for SSH on Windows:

> "Open Terminal Terminal **Git Bash**."
> — *Source: docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent*

**Step 1: Open Git Bash**

Search "Git Bash" in the start menu or right-click in any folder → "Git Bash Here"

**Step 2: Generate the SSH key**

According to GitHub's official documentation:

> "Paste the text below, replacing the email used in the example with your GitHub email address.
> `ssh-keygen -t ed25519 -C "your_email@example.com"`
> Note: If you are using a legacy system that doesn't support the Ed25519 algorithm, use:
> `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`"
> — *Source: docs.github.com/en/authentication/connecting-to-github-with-ssh*

Run in Git Bash:
```bash
ssh-keygen -t ed25519 -C "your@email.com"
```

When asked for location, press Enter to accept the default (`/c/Users/YourUser/.ssh/id_ed25519`).

When asked for passphrase, you can:
- **Leave empty** (press Enter twice) — More convenient
- **Enter a password** — More secure

About the passphrase, GitHub states:

> "If your key has a passphrase and you don't want to enter the passphrase every time you use the key, you can add your key to the SSH agent."
> — *Source: docs.github.com*

**Step 3: Start ssh-agent and add the key**

In Git Bash:
```bash
eval "$(ssh-agent -s)"
```
Expected output: `Agent pid XXXXX`

Add the key:
```bash
ssh-add ~/.ssh/id_ed25519
```

**Step 4: Copy the public key**

In Git Bash:
```bash
clip < ~/.ssh/id_ed25519.pub
```

According to the documentation:

> "$ clip < ~/.ssh/id_ed25519.pub
> Copies the contents of the id_ed25519.pub file to your clipboard"
> — *Source: docs.github.com*

**Note for PowerShell/Windows Terminal**: The `<` operator doesn't work the same:

> "On newer versions of Windows that use the Windows Terminal, or anywhere else that uses the PowerShell command line, you may receive a ParseError. In this case, the following alternative command should be used:
> `$ cat ~/.ssh/id_ed25519.pub | clip`"
> — *Source: docs.github.com*

**Step 5: Add the key to GitHub**

1. Open https://github.com/settings/keys
2. Click "New SSH key"
3. In "Title", enter a descriptive name (e.g., "Personal Windows Laptop")
4. In "Key", paste the copied content (Ctrl+V)
5. Click "Add SSH key"

According to the documentation:

> "In the upper-right corner of any page on GitHub, click your profile picture, then click Settings. In the 'Access' section of the sidebar, click SSH and GPG keys. Click New SSH key."
> — *Source: docs.github.com*

**Step 6: Test the connection**

```bash
ssh -T git@github.com
```

The first time you'll see a warning about the host's authenticity:

> "The authenticity of host 'github.com (IP ADDRESS)' can't be established.
> ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
> Are you sure you want to continue connecting (yes/no)?"
> — *Source: docs.github.com*

Type `yes` and press Enter.

**Expected success message:**
```
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

### SSH configuration file (optional but recommended)

Create or edit the file `~/.ssh/config` (on Windows: `C:\Users\YourUser\.ssh\config`):

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes
```

This ensures the correct key is always used and automatically added to the agent.

### MODERATELY COMMON Case: SSH Problems

**Problem: "Permission denied (publickey)"**

> "A 'Permission denied' error means that the server rejected your connection."
> — *Source: docs.github.com/en/authentication/troubleshooting-ssh/error-permission-denied-publickey*

Verify the key is loaded:
```bash
ssh-add -l -E sha256
```

Solutions:
1. Verify the public key is correctly added on GitHub
2. Make sure to connect as user `git` (not your GitHub username)
3. Restart ssh-agent and re-add the key

**Problem: ssh-agent is not running**

In Git Bash:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

In PowerShell (as administrator):
```powershell
Get-Service ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

**Problem: Conflict between Git SSH and Windows OpenSSH**

Git for Windows includes its own SSH client. To use Windows OpenSSH instead:
```bash
git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"
```

### Git Bash vs PowerShell for SSH

| Feature | Git Bash | PowerShell |
|---------|----------|------------|
| Start ssh-agent | `eval "$(ssh-agent -s)"` | `Start-Service ssh-agent` |
| Copy key | `clip < ~/.ssh/id_ed25519.pub` | `cat ~/.ssh/id_ed25519.pub \| clip` |
| Paths | `~/.ssh/` | `$env:USERPROFILE\.ssh\` |
| GitHub documentation | ✅ Recommended | ✅ Supported |

**Recommendation**: Git Bash is the simplest option and best documented by GitHub.

---

## Complete Installation Summary

### Recommended installation order

1. **Git** → Includes Git Bash needed for SSH
2. **Python 3.11.9** → With "Add to PATH" enabled
3. **Visual Studio Code** → With Python extension
4. **Virtual environment** → To isolate oTree
5. **oTree** → Within the virtual environment
6. **SSH with GitHub** → Using Git Bash

### Quick verification commands

```bash
# Git
git --version

# Python  
python --version
pip --version

# VS Code
code --version

# oTree (with virtual environment activated)
otree --version

# SSH
ssh -T git@github.com
```

### Important locations on Windows

| Element | Typical path |
|---------|-------------|
| Git | `C:\Program Files\Git\` |
| Python 3.11 | `C:\Users\YourUser\AppData\Local\Programs\Python\Python311\` |
| VS Code | `C:\Users\YourUser\AppData\Local\Programs\Microsoft VS Code\` |
| SSH keys | `C:\Users\YourUser\.ssh\` |
| Git config | `C:\Users\YourUser\.gitconfig` |

---

## Appendices: Rare Cases

### Windows in S mode

Windows in S mode only allows Microsoft Store apps. To install these tools you need to exit S mode (free but irreversible) or use the Microsoft Store version of Python (not recommended for oTree due to compatibility issues).

### WSL as an alternative

> "If you are on Windows, WSL is a great way to do Python development."
> — *Source: code.visualstudio.com/docs/languages/python*

WSL (Windows Subsystem for Linux) allows running a complete Linux environment within Windows. It can be useful for advanced users, but this guide focuses on native Windows installation which is simpler for beginners.

### Git installation with Winget

Alternative for users who prefer command line:
```powershell
winget install --id Git.Git -e --source winget
```

> "Install winget tool if you don't already have it, then type this command in command prompt or Powershell."
> — *Source: git-scm.com*

### VS Code installation with Winget

```powershell
winget install Microsoft.VisualStudioCode
```

### oTree 6.0 Beta (October 2025)

> "October 2025 update: oTree 6.0 beta is available with AI integration, better wait pages, custom welcome pages and consent forms, back button, better admin interface, and many other features."
> — *Source: otree.readthedocs.io*

To install the beta version:
```cmd
pip install otree==6.0.0b1
```

Only use for testing new features; for production use the stable version.

### SSL certificate problems with pip

If pip shows SSL certificate errors:
```cmd
pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org otree
```

### Automatic ssh-agent configuration in Git Bash

Add to the `~/.bashrc` file (create if it doesn't exist):

```bash
SSH_ENV="$HOME/.ssh/environment"

function run_ssh_env {
  . "${SSH_ENV}" > /dev/null
}

function start_ssh_agent {
  echo "Initializing SSH agent..."
  ssh-agent | sed 's/^echo/#echo/' > "${SSH_ENV}"
  chmod 600 "${SSH_ENV}"
  run_ssh_env;
  ssh-add ~/.ssh/id_ed25519;
}

if [ -f "${SSH_ENV}" ]; then
  run_ssh_env;
  ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || {
    start_ssh_agent;
  }
else
  start_ssh_agent;
fi
```

This automatically starts ssh-agent every time you open Git Bash.

---

## Conclusion

This guide provides a verified and documented path to set up a complete development environment on Windows 10/11. The most critical points to remember are:

The **"Add Python to PATH"** option is absolutely essential during Python installation; without it, no Python commands will work properly. For oTree, use exclusively **Python 3.11.x** to avoid compatibility errors that can be difficult to diagnose. **Virtual environments** are not optional for serious projects; they protect your system from dependency conflicts. And finally, **Git Bash** is the preferred terminal for SSH operations because it follows exactly the official GitHub documentation.


Always close and reopen terminals after modifying the PATH or installing new tools. This simple step avoids most "command not recognized" errors that frustrate beginners.
