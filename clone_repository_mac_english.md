# Clone the Workshop Repository (macOS)

## What does "cloning" a repository mean?

Cloning simply means downloading a complete copy of a project from GitHub to your computer. It's like doing "copy-paste" of a folder, but with superpowers:

- 📁 Copies all project files
- 📜 Includes complete change history
- 🔗 Maintains connection with GitHub for future updates

---

## What repository are we going to clone?

We'll download the workshop project:

| Information | Details |
|-------------|---------|
| **Repository name** | taller-otree-pgg |
| **URL** | https://github.com/DonovanDiazcide/taller-otree-pgg |
| **Content** | Files and examples for the oTree workshop |

---

## Step 1: Decide where to save the project

Before cloning, think about where you want to save the project on your computer.

**Recommended location:** Documents folder

Example: `/Users/YourUser/Documents/taller-otree`

💡 **Tip:** Avoid locations with:
- Spaces in folder names (prefer `my-project` over `my project`)
- Excessive special characters
- Synced folders like iCloud Drive or Dropbox (they can cause conflicts)

---

## Step 2: Open Terminal

1. Press `Cmd + Space` to open **Spotlight**
2. Type "Terminal"
3. Press Enter

Or alternatively:
- Go to **Applications → Utilities → Terminal**

---

## Step 3: Navigate to your Documents folder

In Terminal, run:

```bash
cd ~/Documents
```

Press Enter. This will take you to your Documents folder.

---

## Step 4: (Optional) Create a specific folder for the workshop

If you want to organize your files better, create a specific folder:

```bash
mkdir taller-otree
cd taller-otree
```

**What do these commands do?**
- `mkdir taller-otree` = **Make Directory** → Creates a folder named "taller-otree"
- `cd taller-otree` = **Change Directory** → Enters that folder

---

## Step 5: Clone the repository

Now let's clone! Run this command in **Terminal**:

```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**What does this command mean?**

| Command part | Meaning |
|--------------|---------|
| `git clone` | Instruction to clone a repository |
| `git@github.com:` | SSH connection to GitHub |
| `DonovanDiazcide/` | Repository owner's username |
| `taller-otree-pgg.git` | Repository name |

**What should you see?**

```
Cloning into 'taller-otree-pgg'...
remote: Enumerating objects: XX, done.
remote: Counting objects: 100% (XX/XX), done.
remote: Compressing objects: 100% (XX/XX), done.
remote: Total XX (delta X), reused XX (delta X), pack-reused X
Receiving objects: 100% (XX/XX), XX.XX KiB | XX.XX MiB/s, done.
Resolving deltas: 100% (X/X), done.
```

✅ **If you see this message** = The repository was cloned successfully!

---

## Step 6: Enter the project folder

After cloning, a new folder will be created with the repository name:

```bash
cd taller-otree-pgg
```

**Verify you're in the correct folder:**

```bash
ls
```

You should see the project files (like `settings.py`, experiment folders, etc.)

---

## Step 7: Open the project in VS Code

Now that you have the project on your computer, open it in VS Code:

```bash
code .
```

The dot (`.`) means "current folder".

**What should happen?**

VS Code will open showing all the project files in the left panel.

**Manual alternative:**
1. Open VS Code
2. Click **File → Open Folder**
3. Navigate to the `taller-otree-pgg` folder
4. Click **Open**

---

## ✅ Verification: Confirm everything is ready

### 1. Verify you're in the correct folder

In Terminal:

```bash
pwd
```

**What should you see?** A path ending in `taller-otree-pgg`, for example:
```
/Users/YourUser/Documents/taller-otree-pgg
```

### 2. View the project files

```bash
ls -la
```

You should see files like `settings.py` and other project folders.

---

## 🎉 Congratulations!

If you got here, you now have:

✅ The workshop repository cloned on your computer  
✅ Access to all project files  
✅ The project open in VS Code  
✅ Everything ready to run oTree experiments

**You're completely ready for the workshop.**

---

## 🔧 Troubleshooting

### Problem: "Permission denied (publickey)"

```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Cause:** Your SSH key is not correctly configured with GitHub.

**Solution:**
1. Verify you configured SSH correctly (previous tutorial section)
2. Test your SSH connection:
   ```bash
   ssh -T git@github.com
   ```
3. If it doesn't work, repeat the SSH configuration steps

### Problem: "Repository not found"

```
ERROR: Repository not found.
fatal: Could not read from remote repository.
```

**Cause:** The repository URL is misspelled.

**Solution:**
Copy and paste exactly this command:
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

### Problem: "destination path already exists"

```
fatal: destination path 'taller-otree-pgg' already exists and is not an empty directory.
```

**Cause:** A folder with that name already exists in the current location.

**Solution:**

**Option A:** Delete the existing folder and clone again:
```bash
rm -rf taller-otree-pgg
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**Option B:** Clone with a different name:
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git taller-otree-new
```

### Alternative: Clone using HTTPS (if SSH doesn't work)

If you have SSH problems and need to clone urgently, you can use HTTPS:

```bash
git clone https://github.com/DonovanDiazcide/taller-otree-pgg.git
```

⚠️ **Note:** With HTTPS you'll be asked for your GitHub username and password each time you interact with the repository. That's why we recommend SSH for regular use.

---

## Command Summary

Here are all the commands in order:

```bash
# 1. Go to your Documents folder
cd ~/Documents

# 2. (Optional) Create folder for the workshop
mkdir taller-otree
cd taller-otree

# 3. Clone the repository
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git

# 4. Enter the project folder
cd taller-otree-pgg

# 5. Open in VS Code
code .
```
