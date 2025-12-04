# Installation and Verification Guide

This document will guide you step-by-step through installing the tools you need for the workshop. Don't worry if you've never used these tools before: we'll explain everything clearly and simply.

## What are we installing and what does each thing do?

Before we begin, here's a summary of what you'll install:

| Tool | What it does
|------|--------------|
| **Git** | Save different versions of your code and collaborate with others | Like "infinite ctrl+Z" + Google Docs for code |
| **Python** | The programming language we’ll use is the channel you use to talk to the computer and ask it to follow your instructions.|
| **VS Code** | Editor where you'll write code | Like Microsoft Word, but for programmers |
| **oTree** | Tool for creating economic experiments | A "ready-to-use template" that makes your work easier |
| **SSH with GitHub** | Connect your computer to GitHub without passwords | Like saving your Netflix password so you don't type it every time |

## About "terminals" or "command lines"

Throughout this tutorial, you'll see us asking you to open different types of "terminals" or "command windows". A **terminal** (also called a **command line**) is a window where you type text instructions for your computer.

Different types exist depending on your operating system:

### On Windows:
- **Command Prompt** (or "cmd"): Windows' basic terminal. It looks like a black window where you type commands.
- **Git Bash**: A special terminal that installs with Git and works similarly to Mac/Linux terminals.
- **PowerShell**: A more advanced Windows terminal (we won't use it much in this tutorial).

**📌 How to find each one on Windows?**
- For **Command Prompt**: Press the Windows key, type "cmd" or "command prompt", and click the application.
- For **Git Bash**: After installing Git, search "Git Bash" in the start menu.

### On macOS/Linux:
- **Terminal**: Your system's native terminal application. On Mac you find it in Applications → Utilities → Terminal.

**🎯 Important rule**: Every time we tell you "open [type of terminal]", make sure to open exactly that type. Some commands only work in specific terminals (especially on Windows).

## Tips before starting

✅ **Read the complete instructions before executing each step**  
✅ **Copy and paste commands when possible** (reduces errors)  
✅ **If something doesn't work, read the "Troubleshooting" section at the end**  
✅ **Close and reopen your terminal after installing something new**

---

# 1. Install Git

## What is Git and why do I need it?

**Git** is like a "supercharged change history" for your code files. Imagine writing a document where you can:
- 📸 See all previous versions
- ⏮️ Go back to any past version
- 👥 Work with other people without overwriting their work

In this workshop, we'll use it to save and share our code.

---

## Step 1: Install Git

### 📥 On Windows

1. **Download the installer**
   - Open your browser and go to: **https://git-scm.com/download/win**
   - The download will start automatically (a `.exe` file)

2. **Run the installer**
   - Double-click the downloaded file
   - You'll see several screens with options. **Don't worry**, you can leave everything at default options (just click "Next")

3. **Important options** (if you want to understand them):
   - When you see "Adjusting your PATH environment", make sure **"Git from the command line and also from 3rd-party software"** is selected
     - *Why?* This allows you to use Git from any terminal
   - If it asks which editor to use, you can choose **Visual Studio Code** (we'll install it later)
   - For other options, just accept the recommended values

4. **Complete the installation**
   - Click "Install" and wait for it to finish

---

### 📥 On macOS

**Simplest option (recommended)**:

1. Open **Terminal**
   - Go to Applications → Utilities → Terminal
   
2. Type this and press Enter:
   ```bash
   git --version
   ```

3. If Git isn't installed, macOS will ask: *"Do you want to install command line developer tools?"*
   - Click **"Install"**
   - Wait for it to finish

**Alternative option (if you use Homebrew)**:

If you already have Homebrew installed, type in Terminal:
```bash
brew install git
```

---

### 📥 On Linux (Ubuntu/Debian)

Open **Terminal** and copy these commands one by one:

```bash
sudo apt update
sudo apt install git
```

It will ask for your password (the same one you use to log into your computer).

---

## ✅ Verify Git installed correctly

**Where to do this**:
- Windows: Open **Command Prompt** or **Git Bash**
- Mac/Linux: Open **Terminal**

**Command**:
```bash
git --version
```

**What should you see?**  
Something like: `git version 2.45.1`

✅ If you see a version number = success!  
❌ If it says "command not found" = go to "Troubleshooting" at the end

---

# 2. Install Python

## What is Python?

Python is a **programming language** (like English or Spanish, but for computers). We'll use it to:
- Write the instructions for our experiment
- Make oTree work

---

## Installation on Windows

**Step 1: Download**
1. Go to: **https://www.python.org/downloads/**
2. Click the big yellow button that says "Download Python 3.X.X"

**Step 2: Install (VERY IMPORTANT)**

1. Double-click the downloaded file
2. **⚠️ BEFORE clicking "Install Now"**:
   - Check the box ✅ **"Add python.exe to PATH"**
   - This is THE most important option (without it, Python won't work from the terminal)

3. Click **"Install Now"**
4. Wait for it to finish

**Why is "Add to PATH" important?**  
Without this option, your computer won't know where Python is when you type commands. It's like having a phone but it's not in your contacts list.

---

## Installation on macOS

**Option 1: From the official site**
1. Go to: **https://www.python.org/downloads/**
2. Download the Mac installer
3. Double-click and install normally

**Option 2: With Homebrew** (if you already have it):
```bash
brew install python
```

---

## Installation on Linux

Many distributions already come with Python installed. To make sure you have everything, open **Terminal** and run:

```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip
```

---

## ✅ Verify Python installed correctly

**Where to do this**:
- Windows: **Command Prompt** (close and open a new one)
- Mac/Linux: **Terminal**

**Command**:
```bash
python --version
```

Or if that doesn't work, try:
```bash
python3 --version
```

**What should you see?**  
`Python 3.10.5` (or any version 3.8 or higher)

✅ If you see the version = success!  
❌ If it says "command not found" = close and open a new terminal. If it persists, go to "Troubleshooting"

---

# 3. Install Visual Studio Code (VS Code)

## What is VS Code?

VS Code is a **code editor** (like Microsoft Word, but designed for programmers). It helps you:
- Write code with colors that make reading easier
- Detect errors before running your code
- Organize your project files

---

## Installation (Windows, Mac, Linux)

**Step 1: Download**
1. Go to: **https://code.visualstudio.com/**
2. Click the big download button
   - The site will automatically detect your operating system

**Step 2: Install on Windows**
1. Run the downloaded installer
2. **Important**: During installation, check these options:
   - ✅ "Add to PATH"
   - ✅ "Create a desktop icon" (optional, but useful)

**Step 2: Install on Mac**
1. Open the downloaded file
2. Drag VS Code to your Applications folder

**Step 2: Install on Linux**
1. For Ubuntu/Debian, download the `.deb` file
2. Double-click to install with the software center

---

## Configure the `code` command (to open VS Code from the terminal)

### On Mac/Linux:

1. Open VS Code
2. Press `Cmd+Shift+P` (Mac) or `Ctrl+Shift+P` (Linux)
   - This opens the "Command Palette" (an internal search bar)
3. Type: `shell command`
4. Select: **"Shell Command: Install 'code' command in PATH"**
5. Press Enter
6. It will ask for your administrator password
7. Close and reopen Terminal

### On Windows:

If you checked "Add to PATH" during installation, the command should already work.

If it doesn't work:
1. Search "environment variables" in the start menu
2. Edit the user "Path" variable
3. Add this path (adjust according to your installation):
   ```
   C:\Users\YourUsername\AppData\Local\Programs\Microsoft VS Code\bin
   ```

---

## ✅ Verify the installation

**Where**:
- Windows: Command Prompt
- Mac/Linux: Terminal

**Command**:
```bash
code --version
```

**What should you see?**  
```
1.85.0
5437499feb04f7a586f677b155b039bc2b3669eb
x64
```

✅ If you see version numbers = success!

**Test opening a folder**:
```bash
code .
```
(The dot means "current folder")

This should open VS Code in the folder where you are.

---

# 4. Install oTree

## What is oTree?

oTree is a **framework** (set of tools) built with Python that makes it easy to create economic experiments and interactive games online. Instead of programming everything from scratch, oTree gives you ready-to-use templates and functions.

---

## Installation

**Where to run these commands**:
- Windows: Command Prompt
- Mac/Linux: Terminal

**Step 1: Update pip**

`pip` is Python's "package installer" (like an app store for Python programs).

```bash
pip install --upgrade pip
```

*On Linux, if you get an error, use*:
```bash
pip install --upgrade pip --break-system-packages
```

**Step 2: Install oTree**

```bash
pip install "otree>=5,<6"
```

This will download and install oTree version 5 (may take 1-2 minutes).

*On Linux, if you get an error, use*:
```bash
pip install "otree>=5,<6" --break-system-packages
```

---

## ✅ Verify the installation

**Command**:
```bash
otree --version
```

**What should you see?**  
`5.10.4` (or any version starting with 5)

✅ If you see a number 5.X.X = success!

---

# 5. Configure SSH with GitHub

## What is SSH and why do I need it?

**SSH** is a secure method to connect your computer with GitHub. Once configured:
- ✅ You can upload and download code without typing your password
- ✅ It's more secure than using passwords
- ✅ GitHub recommends it

**Simple analogy**: It's like registering your fingerprint on your phone. The first time takes a bit of time to set up, but then it's automatic and more secure.

---

## Step 1: Check if you already have SSH keys

**Where to run**:
- Windows: **Git Bash** (DON'T use Command Prompt for SSH)
- Mac/Linux: Terminal

**Command**:
```bash
ls ~/.ssh
```

**What does this mean?**  
- `ls` = "list" (show)
- `~/.ssh` = a hidden folder where SSH keys are stored

**Possible results**:

1. **You see files like** `id_ed25519` and `id_ed25519.pub`  
   → You already have SSH keys, you can skip to Step 4

2. **It says "No such file or directory"**  
   → You don't have SSH keys, continue to Step 2

---

## Step 2: Create new SSH keys

**Where to run**:
- Windows: **Git Bash**
- Mac/Linux: Terminal

**Command** (replace with YOUR GitHub email):
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

**What does this command mean?**
- `ssh-keygen` = program to create keys
- `-t ed25519` = type of encryption (the most modern and secure)
- `-C "your_email@example.com"` = a label to identify this key

**What will happen**:

1. It will ask: `Enter a file in which to save the key`
   - Simply press **Enter** (accept the default location)

2. It will ask: `Enter passphrase`
   - You can:
     - **Leave empty** (press Enter twice) = more convenient, less secure
     - **Write a password** = more secure, but you'll have to type it when using the key

3. You'll see a "randomart" (character drawing) = Successfully created!

**Result**: You now have TWO files in `~/.ssh/`:
- `id_ed25519` = private key (NEVER share this file)
- `id_ed25519.pub` = public key (you WILL share this with GitHub)

---

## Step 3: Add your key to the "ssh-agent"

**What is the ssh-agent?**  
It's a program that "remembers" your SSH keys so you don't have to type the passphrase every time.

**Where to run**:
- Windows: Git Bash
- Mac/Linux: Terminal

**Step 3a: Start the ssh-agent**

```bash
eval "$(ssh-agent -s)"
```

**What should you see?**  
`Agent pid 12345` (some number)

**Step 3b: Add your key**

```bash
ssh-add ~/.ssh/id_ed25519
```

(If you used RSA instead of ed25519, replace `id_ed25519` with `id_rsa`)

**What should you see?**  
`Identity added: /Users/your_username/.ssh/id_ed25519`

---

## Step 4: Copy your public key

We need to copy the content of your PUBLIC key (the `.pub` file) to paste it into GitHub.

**Where to run**:
- Windows: Git Bash
- Mac/Linux: Terminal

### Option A: Copy automatically (Mac)
```bash
pbcopy < ~/.ssh/id_ed25519.pub
```
Done! The content is already copied.

### Option B: View and copy manually (Windows/Linux)
```bash
cat ~/.ssh/id_ed25519.pub
```

You'll see something like:
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMx... your_email@example.com
```

**Select ALL the text** (from `ssh-ed25519` to your email) and copy it.

---

## Step 5: Add the key to your GitHub account

**In your browser**:

1. Go to **github.com** and log in

2. Click on your **profile picture** (top right corner)

3. Select **"Settings"**

4. In the left sidebar, click **"SSH and GPG keys"**

5. Click the green button **"New SSH key"**

6. Fill out the form:
   - **Title**: Give it a descriptive name, for example:
     - "Personal Laptop"
     - "Work Computer"
     - "MacBook CIDE"
   - **Key**: Paste the content you copied here (your public key)

7. Click **"Add SSH key"**

8. GitHub will ask for your password to confirm

Done! Your SSH key is now registered on GitHub.

---

## Step 6: Test the connection

Let's verify everything works correctly.

**Where to run**:
- Windows: Git Bash
- Mac/Linux: Terminal

**Command**:
```bash
ssh -T git@github.com
```

**The first time**, you'll see this message:
```
The authenticity of host 'github.com' can't be established.
Are you sure you want to continue connecting (yes/no)?
```

Type **`yes`** and press Enter.

**If everything is fine, you'll see**:
```
Hi your_github_username! You've successfully authenticated, but GitHub does not provide shell access.
```

✅ This means: Your SSH configuration works perfectly!

❌ If you see an error, check:
- Did you copy the entire public key?
- Is the ssh-agent running?
- Did you add the key to the ssh-agent?

---

# Complete Final Verification

You're almost done! Now let's verify everything works.

**Open a new terminal** (Command Prompt on Windows or Terminal on Mac/Linux) and run these commands one by one:

## 1. Git
```bash
git --version
```
✅ You should see: `git version 2.45.1` (or similar)

## 2. Python
```bash
python --version
```
✅ You should see: `Python 3.10.5` (or higher than 3.8)

## 3. VS Code
```bash
code --version
```
✅ You should see: `1.85.0` (or similar)

## 4. oTree
```bash
otree --version
```
✅ You should see: `5.10.4` (or any 5.X.X)

## 5. GitHub SSH
```bash
ssh -T git@github.com
```
✅ You should see: `Hi your_username! You've successfully authenticated...`

---

# 🎉 If all the above commands work:

**Congratulations! You have everything installed and configured.**

Now you can:
- ✅ Clone repositories from GitHub
- ✅ Write code in VS Code
- ✅ Run oTree
- ✅ Collaborate using Git without passwords

**You're ready to start the workshop.**

---

# 🔧 Common Troubleshooting

## Problem: "command not found" or "not recognized as a command"

**Cause**: The program is not in PATH or you didn't restart the terminal.

**Solution**:
1. **Completely close** your terminal
2. Open a **new** terminal
3. Try the command again

If it persists:
- Verify you checked "Add to PATH" during installation
- On Windows: search "environment variables" and verify the program's path is in PATH

---

## Problem: Python works by typing `python3` but not `python`

**Cause**: On some systems, Python 3 is called `python3` to distinguish it from Python 2.

**Solution**:
- Use `python3` instead of `python` for all commands
- Example: `python3 --version` or `python3 -m pip install otree`

---

## Problem: On Linux pip says "error: externally-managed-environment"

**Cause**: Ubuntu and other modern distributions protect the system Python.

**Solution**:
Add `--break-system-packages` at the end of your pip commands:
```bash
pip install otree --break-system-packages
```

---

## Problem: SSH doesn't work in Windows Command Prompt

**Cause**: Command Prompt doesn't have SSH commands by default.

**Solution**:
- Use **Git Bash** for all SSH commands
- Git Bash was automatically installed with Git

---

## Problem: VS Code doesn't open with `code .`

**Cause**: It wasn't added to PATH or you didn't restart the terminal.

**Solution**:
1. Close all terminals
2. Open a new one
3. Try again

If it persists:
- **Mac**: Follow the steps for "Configure the `code` command" in Section 3
- **Windows**: Verify you checked "Add to PATH" during installation

---

# References and Additional Resources

This tutorial is based on official documentation:

- **Git**: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
- **Python**: https://docs.python.org/3/using/
- **VS Code**: https://code.visualstudio.com/docs
- **oTree**: https://otree.readthedocs.io/
- **GitHub SSH**: https://docs.github.com/en/authentication/connecting-to-github-with-ssh

---

**Last updated**: December 2024  
**Version**: 3.0 - Simplified for beginners

---

## Glossary of Technical Terms

If you encounter any term you don't understand, here's a quick guide:

| Term | Simple meaning |
|------|----------------|
| **Terminal / Command line** | Window where you type text instructions |
| **PATH** | List of folders where your computer looks for programs |
| **SSH** | Secure method to connect to servers |
| **Framework** | Set of tools that make programming easier |
| **Repository** | Project folder saved on GitHub |
| **Clone** | Copy a project from GitHub to your computer |
| **pip** | Package installer for Python |
| **Package** | Additional program or tool for Python |
