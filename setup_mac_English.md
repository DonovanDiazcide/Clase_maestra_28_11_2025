# Complete Installation Guide for Development on macOS (2025)

**Git, Python, Visual Studio Code, oTree, and SSH with GitHub** — This guide provides a clear path from zero to a fully functional development environment for economic experiments with oTree on your Mac. The most important factor to consider: **oTree only works with Python 3.7-3.11**, not with newer versions, which determines the optimal installation order.

The recommended strategy is to first install the base tools (Git, Python 3.11, VS Code), then oTree in a virtual environment, and finally configure SSH to connect with GitHub. The entire process takes approximately **45-60 minutes** for new users.

---

## Before You Begin: Know Your Mac

Before installing any tools, you need to identify two fundamental characteristics of your computer that will affect some installation steps.

### Check Your Processor Type

Open the **Terminal** application (search for it in Spotlight with `Cmd + Space` and type "Terminal") and run:

```bash
uname -m
```

- If it shows `arm64`: you have a **Mac with Apple Silicon** (M1, M2, M3, or M4)
- If it shows `x86_64`: you have a **Mac with Intel processor**

### Check Your macOS Version

In the Apple menu () → **About This Mac**, you'll see your version. This guide covers:

| Version | Name | Support |
|---------|------|---------|
| 15.x | Sequoia | ✅ Full |
| 14.x | Sonoma | ✅ Full |
| 13.x | Ventura | ✅ Full |
| 12.x | Monterey | ⚠️ Functional with limitations |

### The Default Shell: zsh

Since macOS Catalina (10.15), the default shell is **zsh**, not bash. This means that environment variable configuration goes in the `~/.zshrc` file (or `~/.zprofile` for variables loaded at login). The tilde (`~`) represents your user folder, equivalent to `/Users/your_username`.

---

## Part 1: Installing Git

Git is the version control system that allows you to track changes in your code and collaborate with others. It's a fundamental requirement for working with GitHub.

### COMMON Case: Installation via Xcode Command Line Tools

This is the simplest method and the one **70-80% of Mac users will follow**. Apple includes Git as part of its development tools.

> *"Apple ships a binary package of Git with Xcode Command Line Tools. You can install this via: `$ xcode-select --install`"*
> — Official Git documentation (git-scm.com/download/mac)

**Step 1:** Open Terminal and run:

```bash
xcode-select --install
```

**Step 2:** A popup window will appear asking if you want to install the developer tools. Click **"Install"** and accept the license terms.

**Step 3:** Wait for the download and installation (approximately 1 GB, may take 10-20 minutes depending on your connection).

**Step 4:** Verify the installation:

```bash
git --version
```

You should see something like: `git version 2.39.3 (Apple Git-146)`

**Important note:** The version of Git that Apple includes is usually a few versions behind the latest, but it's perfectly functional for most uses.

**Alternative (automatic) method:** If you've never installed the developer tools, simply typing `git` in Terminal will automatically trigger the installer:

```bash
git
```

### MODERATELY COMMON Case: Installation via Homebrew

If you already have **Homebrew** installed (the most popular package manager for Mac), or if you need the **latest version of Git**, this method is preferable.

> *"Install homebrew if you don't already have it, then: `$ brew install git`"*
> — Official Git documentation

**If you don't have Homebrew installed yet:**

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**Important for Apple Silicon (M1/M2/M3/M4):** After installing Homebrew, you must add its path to PATH. The installer will show you specific instructions, but generally it's:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
eval "$(/opt/homebrew/bin/brew shellenv)"
```

**For Intel Macs**, Homebrew installs to `/usr/local` and generally doesn't require additional PATH configuration.

**Install Git with Homebrew:**

```bash
brew install git
```

**Verify you're using the Homebrew version:**

```bash
which git
```

- Apple Silicon: should show `/opt/homebrew/bin/git`
- Intel: should show `/usr/local/bin/git`

### Initial Git Configuration (REQUIRED for everyone)

After installing Git, you must configure your identity. This information is included in every commit you make.

> *"The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information, and it's immutably baked into the commits you start creating."*
> — Pro Git Book (git-scm.com/book)

```bash
# Your name (use your real name or the one you use professionally)
git config --global user.name "Your Full Name"

# Your email (use the same email as your GitHub account)
git config --global user.email "your_email@example.com"

# Set default branch to "main" (modern standard)
git config --global init.defaultBranch main

# Verify configuration
git config --list
```

---

## Part 2: Installing Python

Python is the programming language in which oTree is written. The version choice is **critical** because oTree has specific compatibility requirements.

### Fundamental Consideration: oTree Compatibility

> *"This version of oTree is only compatible with these Python versions: 3.7, 3.8, 3.9, 3.10, 3.11"*
> — oTree error message / oTreeHub Forum

**Python 3.12 and 3.13 are NOT compatible with oTree.** For this reason, this guide recommends installing specifically **Python 3.11**, which is the latest compatible version with active support.

### Python Status on Modern macOS

> *"Recent versions of macOS include a python3 command in /usr/bin/python3 that links to a usually older and incomplete version of Python provided by and for use by the Apple development tools. You should never modify or attempt to delete this installation."*
> — Official Python documentation (docs.python.org/3/using/mac.html)

- **Python 2.7 was removed** starting with macOS 12.3 (Monterey)
- macOS includes Python 3.9.6 through Xcode Command Line Tools
- This system version **should not be used for development**; install your own version

### COMMON Case: Installation from python.org

This method is the most straightforward and recommended for users who don't have Homebrew or prefer a traditional graphical installer.

> *"Current installers provide a universal2 binary build of Python which runs natively on all Macs (Apple Silicon and Intel) that are supported by a wide range of macOS versions."*
> — Official Python documentation

**Step 1:** Go to https://www.python.org/downloads/macos/

**Step 2:** Download the **Python 3.11.x** installer (the latest specific version of the 3.11 series). Look for the file ending in `-macos11.pkg`.

**Step 3:** Open the downloaded `.pkg` file and follow the installation wizard. Accept the default values.

**Step 4:** When installation finishes, a folder will open with two important files:
- `Install Certificates.command` — **Run it by double-clicking**. This installs SSL certificates necessary for pip to work correctly.
- `Update Shell Profile.command` — Automatically updates the PATH.

**Step 5:** Close and reopen Terminal, then verify:

```bash
python3 --version
# Should show: Python 3.11.x

pip3 --version
# Should show pip version and Python 3.11 path
```

**Installation location:** `/Library/Frameworks/Python.framework/Versions/3.11/`

### MODERATELY COMMON Case: Installation via Homebrew

If you already use Homebrew, this method keeps all your tools managed in one place.

```bash
# Install Python 3.11 specifically
brew install python@3.11

# Verify installation
python3.11 --version

# Verify pip
pip3.11 --version
```

**Create aliases for easier use** (add to `~/.zshrc`):

```bash
echo 'alias python3="/opt/homebrew/opt/python@3.11/bin/python3.11"' >> ~/.zshrc
echo 'alias pip3="/opt/homebrew/opt/python@3.11/bin/pip3.11"' >> ~/.zshrc
source ~/.zshrc
```

### PATH Configuration

If after installation the `python3` or `pip3` commands don't work, you need to add Python to the PATH. Open `~/.zshrc` with any text editor:

```bash
nano ~/.zshrc
```

Add the following line (adjust the version if different):

```bash
export PATH="/Library/Frameworks/Python.framework/Versions/3.11/bin:$PATH"
```

Save (`Ctrl + O`, then `Enter`) and close (`Ctrl + X`). Reload the configuration:

```bash
source ~/.zshrc
```

### Complete Python Verification

Run these commands to confirm everything is correctly configured:

```bash
# Python version
python3 --version

# Executable location
which python3

# pip version
pip3 --version

# Verify architecture (useful for Apple Silicon)
python3 -c "import platform; print(platform.machine())"
# arm64 = Apple Silicon, x86_64 = Intel
```

---

## Part 3: Installing Visual Studio Code

Visual Studio Code (VS Code) is a free code editor from Microsoft, extremely popular for its flexibility and extensions. It's ideal for developing oTree projects.

### COMMON Case: Manual Download and Installation

> *"Download Visual Studio Code for macOS. Open the browser's download list and locate the downloaded app. Drag Visual Studio Code.app to the Applications folder, making it available in the macOS Launchpad."*
> — Official Microsoft documentation (code.visualstudio.com/docs/setup/mac)

**Step 1:** Go to https://code.visualstudio.com/download

**Step 2:** Download the Mac version. There are three options:
- **Universal** (recommended): works on any Mac
- **Intel Chip**: only for Macs with Intel processor
- **Apple Silicon**: optimized for M1/M2/M3/M4

**Step 3:** Open the downloaded `.zip` file (usually decompresses automatically).

**Step 4:** Drag `Visual Studio Code.app` to your **Applications** folder.

**Step 5:** Open VS Code from Applications or Launchpad. The first time, macOS will ask if you want to open an application downloaded from the Internet; click **"Open"**.

### MODERATELY COMMON Case: Installation via Homebrew

```bash
brew install --cask visual-studio-code
```

This method has the advantage of automatically configuring the `code` command in Terminal.

### Configure the 'code' Command in Terminal

The `code` command lets you open files and folders in VS Code directly from Terminal. For example, `code .` opens the current folder.

> *"Open the Command Palette (Cmd+Shift+P), type 'shell command', and run the Shell Command: Install 'code' command in PATH command."*
> — VS Code documentation

**Step 1:** Open VS Code.

**Step 2:** Press `Cmd + Shift + P` to open the Command Palette.

**Step 3:** Type "shell command" and select **"Shell Command: Install 'code' command in PATH"**.

**Step 4:** Close and reopen Terminal.

**Step 5:** Verify it works:

```bash
code --version
```

### Troubleshooting Gatekeeper

If VS Code won't open and you see a message about "unidentified developer" (rare with official downloads):

**Method 1 — Control + click:**
1. In Finder, go to Applications
2. Hold the `Control` key and click on Visual Studio Code
3. Select "Open" from the contextual menu
4. Confirm you want to open it

**Method 2 — System Settings:**
1. Go to **System Settings → Privacy & Security**
2. In the "Security" section, look for the message about VS Code
3. Click **"Open Anyway"**

### Recommended Extensions for Python/oTree Development

Open VS Code and go to the Extensions tab (`Cmd + Shift + X`). Install:

- **Python** (ms-python.python): Official Microsoft extension. Provides IntelliSense, debugging, and more.
- **Pylance** (ms-python.vscode-pylance): Language server that improves autocompletion.

Recommended configuration for Python (open Settings with `Cmd + ,` and search "settings json"):

```json
{
  "python.languageServer": "Pylance",
  "editor.formatOnSave": true,
  "[python]": {
    "editor.defaultFormatter": "ms-python.python"
  }
}
```

---

## Part 4: Installing oTree

oTree is an open-source framework for creating behavioral economics experiments, surveys, and interactive games. It runs in the browser and allows for both laboratory and online experiments.

### Prerequisites Verified

Before continuing, confirm you have:

```bash
# Python 3.11 (NOT 3.12 or higher)
python3 --version

# pip working
pip3 --version
```

### COMMON Case: Installation in Virtual Environment

**Virtual environments are essential** for oTree. They isolate project dependencies and avoid conflicts with other Python packages.

> *"You can install many versions of oTree using separate Python virtual environments."*
> — oTree documentation

**Step 1:** Create a folder for your oTree projects:

```bash
mkdir ~/oTree-projects
cd ~/oTree-projects
```

**Step 2:** Create a virtual environment:

```bash
python3 -m venv venv
```

This creates a `venv` folder containing an isolated Python installation.

**Step 3:** Activate the virtual environment:

```bash
source venv/bin/activate
```

Your Terminal prompt will change to show `(venv)` at the beginning, indicating the environment is active.

**Step 4:** Update pip within the virtual environment:

```bash
pip install --upgrade pip
```

**Step 5:** Install oTree:

```bash
pip install otree
```

**Step 6:** Verify installation:

```bash
otree --version
```

You should see something like `5.11.1` (the current stable version).

### Create and Run Your First oTree Project

**Step 1:** Create a new project:

```bash
otree startproject my_experiment
```

When asked if you want to include sample games, answer `y` (yes). These examples are excellent for learning.

**Step 2:** Enter the project folder:

```bash
cd my_experiment
```

**Step 3:** Start the development server:

```bash
otree devserver
```

**Step 4:** Open your browser at http://localhost:8000

You'll see the oTree admin interface with the available sample games.

**To stop the server:** Press `Ctrl + C` in Terminal.

### Daily Workflow with oTree

Each time you want to work on your project:

```bash
# 1. Navigate to your projects folder
cd ~/oTree-projects

# 2. Activate the virtual environment
source venv/bin/activate

# 3. Enter your project
cd my_experiment

# 4. Start the server
otree devserver

# 5. When finished, deactivate the environment
deactivate
```

### oTree Project Structure

When you create a project, oTree generates this structure:

```
my_experiment/
├── _rooms/              → Laboratory room configuration
├── _static/             → CSS, JavaScript, image files
├── _templates/          → Global HTML templates
├── settings.py          → Main project configuration
├── requirements.txt     → Dependency list
└── [app folders]        → Each game/experiment is an app
```

---

## Part 5: Configuring SSH with GitHub

SSH (Secure Shell) allows you to connect to GitHub without entering your username and password each time. Once configured, you can push, pull, and perform other Git operations securely and automatically.

### COMMON Case: Complete Step-by-Step Configuration

#### Step 1: Check for Existing SSH Keys

First, check if you already have SSH keys:

```bash
ls -al ~/.ssh
```

Look for files like `id_ed25519.pub`, `id_rsa.pub`, or `id_ecdsa.pub`. If they exist, you might be able to reuse them. If you see the error "No such file or directory", you don't have keys and must create one.

#### Step 2: Generate a New SSH Key

GitHub recommends the **Ed25519** algorithm for its security and efficiency.

> *"If you are using a legacy system that doesn't support the Ed25519 algorithm, use RSA."*
> — GitHub documentation

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Use the **same email** you have on your GitHub account.

The system will ask you:
1. **File location:** Press `Enter` to accept the default location (`~/.ssh/id_ed25519`)
2. **Passphrase:** Enter a secure password (recommended, though optional). The passphrase protects your private key.

#### Step 3: Start the SSH Agent

The SSH agent manages your keys and saves you from typing the passphrase repeatedly.

```bash
eval "$(ssh-agent -s)"
```

You'll see something like: `Agent pid 59566`

#### Step 4: Create the SSH Configuration File

This file tells SSH how to handle connections to GitHub on macOS.

```bash
touch ~/.ssh/config
nano ~/.ssh/config
```

Add this content (the configuration file **must be exactly like this**):

```text
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

**Important notes:**
- `UseKeychain yes` saves your passphrase in the macOS Keychain
- If you **didn't** put a passphrase on your key, omit the `UseKeychain yes` line

Save and close (`Ctrl + O`, `Enter`, `Ctrl + X`).

#### Step 5: Add Your Key to the SSH Agent

The command varies depending on your macOS version:

**macOS Monterey (12.0) or later:**
```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

**macOS Big Sur (11.0) or earlier:**
```bash
ssh-add -K ~/.ssh/id_ed25519
```

> *"The `--apple-use-keychain` option stores the passphrase in your keychain. In macOS versions prior to Monterey (12.0), the flag used the syntax `-K`."*
> — GitHub documentation

#### Step 6: Copy the Public Key to Clipboard

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

This command copies the content of your public key. **Never share your private key** (the file without `.pub`).

#### Step 7: Add the Key to Your GitHub Account

1. Go to https://github.com and make sure you're logged in
2. Click on your **profile photo** → **Settings**
3. In the sidebar, under "Access", select **SSH and GPG keys**
4. Click **New SSH key**
5. In **Title**, enter a descriptive name (e.g., "Personal MacBook Pro 2025")
6. In **Key type**, select "Authentication Key"
7. In **Key**, paste the key you copied (`Cmd + V`)
8. Click **Add SSH key**
9. Confirm with your GitHub password if requested

#### Step 8: Test the Connection

```bash
ssh -T git@github.com
```

The first time you'll see a warning about the authenticity of the host. Type `yes` to continue.

**Successful response:**
```
Hi your_username! You've successfully authenticated, but GitHub does not provide shell access.
```

Congratulations! SSH is correctly configured.

### Correct SSH File Permissions

If you have permission problems, make sure your files have the correct configuration:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/config
```

---

## APPENDIX A: RARE Cases and Troubleshooting

### Git: Error "xcrun: error: invalid active developer path"

This error frequently appears **after updating macOS**, because the update can invalidate the developer tools.

```
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools),
missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

**Solution:**
```bash
xcode-select --install
```

If it persists:
```bash
sudo xcode-select --reset
```

### Git: Conflict Between Apple and Homebrew Version

**Symptom:** `git --version` shows an old version even though you installed a new one with Homebrew.

**Diagnosis:**
```bash
which -a git
# Will show all installed git locations
```

**Solution:** Make sure Homebrew is earlier in the PATH. Add to `~/.zshrc`:

For Apple Silicon:
```bash
export PATH="/opt/homebrew/bin:$PATH"
```

For Intel:
```bash
export PATH="/usr/local/bin:$PATH"
```

### Python: Error "externally-managed-environment"

On macOS Sonoma (14+) and Python 3.12+, pip may show this error when trying to install packages globally.

**Solution:** **Always use virtual environments** (which you already do if you follow this guide for oTree):

```bash
python3 -m venv my_environment
source my_environment/bin/activate
pip install package
```

### Python: SSL Certificates

If you see SSL certificate errors when using pip or making HTTPS requests:

**Solution:** Run the certificate installer that comes with Python:

```bash
# Adjust version according to what you have installed
/Applications/Python\ 3.11/Install\ Certificates.command
```

### oTree: Error "This version of oTree is only compatible with..."

**Problem:** You tried to install oTree with Python 3.12 or higher.

**Solution:** Install Python 3.11 and create a new virtual environment:

```bash
# If using Homebrew
brew install python@3.11

# Create virtual environment with that specific version
/opt/homebrew/opt/python@3.11/bin/python3.11 -m venv venv

# Activate and reinstall oTree
source venv/bin/activate
pip install otree
```

### oTree: Port 8000 in Use

**Symptom:** "Address already in use" error when running `otree devserver`.

**Solution:**
```bash
# Find which process is using the port
lsof -i :8000

# Kill that process (replace PID with the number that appears)
kill -9 PID

# Or use a different port
otree devserver 8080
```

### SSH: Error "Permission denied (publickey)"

**Common causes and solutions:**

1. **Key not added to agent:**
```bash
ssh-add -l  # List loaded keys
# If empty:
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

2. **Key not added to GitHub:** Go to GitHub Settings → SSH Keys and verify your key appears.

3. **Incorrect permissions:**
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
```

4. **Detailed debug:**
```bash
ssh -vT git@github.com
```

### SSH: Error "ssh-add: illegal option -- apple-use-keychain"

**Cause:** You're using a version of `ssh-add` that's not Apple's (possibly installed by Homebrew).

**Solution:** Explicitly use Apple's version:

```bash
/usr/bin/ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

### VS Code: The 'code' Command Doesn't Work After Reinstalling

**Solution:** Reinstall it from VS Code:
1. Open VS Code
2. `Cmd + Shift + P`
3. Type "Shell Command: Install 'code' command in PATH"
4. Restart Terminal

**Manual alternative** — add to `~/.zshrc`:
```bash
export PATH="$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"
```

---

## APPENDIX B: Apple Silicon vs Intel Differences

| Aspect | Apple Silicon (M1/M2/M3/M4) | Intel |
|--------|----------------------------|-------|
| Homebrew location | `/opt/homebrew` | `/usr/local` |
| Requires PATH configuration for Homebrew | Yes | Generally no |
| Reported architecture | `arm64` | `x86_64` |
| Python from python.org | Universal (runs native) | Universal (runs native) |
| VS Code recommended version | Universal or Apple Silicon | Universal or Intel |

### Check Architecture of Any Program

```bash
file $(which python3)
# Will show if it's arm64, x86_64, or universal
```

---

## APPENDIX C: Quick Reference for macOS Configuration Files

| File | Purpose | When It's Loaded |
|------|---------|------------------|
| `~/.zshrc` | zsh configuration | Every time you open Terminal |
| `~/.zprofile` | Environment variables | At login |
| `~/.ssh/config` | SSH configuration | When using SSH commands |
| `~/.gitconfig` | Global Git configuration | When using Git commands |

### Reload Configuration Without Closing Terminal

```bash
source ~/.zshrc
```

---

## APPENDIX D: Useful Reference Commands

### Git
```bash
git --version              # View version
git config --list          # View configuration
git init                   # Initialize repository
git clone URL              # Clone repository
git status                 # View status of changes
git add .                  # Add all changes
git commit -m "message"    # Create commit
git push                   # Upload changes to GitHub
git pull                   # Download changes from GitHub
```

### Python
```bash
python3 --version           # View version
which python3               # Executable location
python3 -m venv name        # Create virtual environment
source name/bin/activate    # Activate virtual environment
deactivate                  # Deactivate virtual environment
pip install package         # Install package
pip list                    # View installed packages
pip freeze > requirements.txt  # Export dependencies
```

### oTree
```bash
otree startproject name     # Create project
otree startapp name         # Create application
otree devserver             # Start development server
otree devserver 8080        # Use alternative port
otree resetdb               # Reset database
```

### SSH
```bash
ssh-keygen -t ed25519 -C "email"  # Generate key
eval "$(ssh-agent -s)"             # Start agent
ssh-add --apple-use-keychain ~/.ssh/id_ed25519  # Add key
ssh -T git@github.com              # Test connection
```

---

## Conclusion

Upon completing this guide you'll have a professional development environment on your Mac: **Git** for version control, **Python 3.11** compatible with oTree, **Visual Studio Code** as a code editor, **oTree** to create economic experiments, and **SSH** configured to work smoothly with GitHub.

The most critical point to remember is **version compatibility**: oTree requires Python 3.7-3.11, and using virtual environments is not optional but essential to avoid conflicts. With this setup, you can develop, version, and share your experiments professionally and reproducibly.

The next time you need to start a new project, simply: create a folder, create a virtual environment with Python 3.11, activate it, install oTree, and run `otree startproject`. All the ecosystem you configured today will continue working.