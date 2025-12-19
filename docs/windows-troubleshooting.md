# Windows Troubleshooting Guide

Complete guide for Windows users. Addresses the most common setup issues, especially the git-bash PATH error.

**On Windows?** Read this guide BEFORE starting setup. It will save you hours.

---

## Table of Contents

1. [Git-Bash PATH Error (Most Common)](#git-bash-path-error)
2. [GitHub CLI Installation](#github-cli-installation)
3. [VS Code + Claude Code Setup](#vs-code--claude-code-setup)
4. [Windows File Paths](#windows-file-paths)
5. [PowerShell vs Git Bash](#powershell-vs-git-bash)
6. [Permissions & Security Warnings](#permissions--security-warnings)
7. [Claude Code Prompts for Self-Diagnosis](#claude-code-prompts)

---

## Git-Bash PATH Error

**This is the #1 Windows issue.** About 80% of Windows users hit this error.

### The Error

```
Error: Claude Code on Windows requires git-bash.
If installed but not in PATH, set environment variable
CLAUDE_CODE_GIT_BASH_PATH=C:\Program Files\Git\bin\bash.exe
```

### Quick Fix (Try This First)

1. **Close VS Code completely** (very important!)

2. **Open System Environment Variables:**
   - Press `Windows + S` to open search
   - Type: "environment variables"
   - Click: "Edit the system environment variables"
   - Click: "Environment Variables" button (bottom right)

3. **Create New Variable:**
   - Under "User variables" (top section), click "New"
   - Variable name: `CLAUDE_CODE_GIT_BASH_PATH`
   - Variable value: `C:\Program Files\Git\bin\bash.exe`
   - Click "OK"

4. **Save Everything:**
   - Click "OK" on Environment Variables window
   - Click "OK" on System Properties window

5. **Restart VS Code** (important!)
   - Close VS Code completely
   - Wait 5 seconds
   - Open VS Code again

6. **Test:**
   - Open Claude Code panel
   - Error should be gone!

### Where to Find Environment Variables

```
Start Menu → Search "environment" →
"Edit the system environment variables" →
"Environment Variables" button →
"New" under User variables
```

### What It Should Look Like

```
Variable name:  CLAUDE_CODE_GIT_BASH_PATH
Variable value: C:\Program Files\Git\bin\bash.exe
```

### If That Didn't Work

The `bash.exe` file might be in a different location on your computer.

**Find bash.exe:**

1. Open File Explorer
2. Go to "This PC" or "Computer"
3. Click the search box (top right)
4. Type: `bash.exe`
5. Wait for search to complete
6. Look for results in folders like:
   - `C:\Program Files\Git\bin\bash.exe`
   - `C:\Program Files (x86)\Git\bin\bash.exe`
   - `C:\Users\[YourName]\AppData\Local\Programs\Git\bin\bash.exe`

7. **Right-click** on the bash.exe file
8. Select "Copy as path"
9. This copies the full path to your clipboard

10. **Use that path** in step 3 above instead of `C:\Program Files\Git\bin\bash.exe`

### Still Not Working? Git Might Not Be Installed

**Install Git for Windows:**

1. Go to: https://git-scm.com/download/win
2. Click: "Click here to download" (64-bit version)
3. Run the downloaded installer
4. **Important settings during installation:**
   - "Select Components": Check "Add a Git Bash Profile to Windows Terminal"
   - "Choosing the default editor": Select "Use Visual Studio Code"
   - "Adjusting your PATH environment": Select "Git from the command line and also from 3rd-party software"
   - Everything else: Use defaults (just click "Next")

5. After installation completes, **restart your computer**
6. Then follow "Quick Fix" steps above

---

## GitHub CLI Installation

### Symptoms
- Claude Code says "GitHub CLI not found"
- Can't clone repositories
- Git commands don't work

### Solution

**Install GitHub CLI for Windows:**

1. Go to: https://cli.github.com/
2. Click: "Download for Windows"
3. Choose: `.msi installer` (easier)
4. Run the downloaded file
5. Follow installer (all defaults are fine)
6. **Restart VS Code** after installation

**Test if it worked:**

1. Open VS Code
2. Open Claude Code
3. Type this message to Claude:

```
Please test if GitHub CLI is installed by running: gh --version

If it's not found, help me diagnose the issue.
```

### Manual Installation (If Installer Fails)

1. Download from: https://github.com/cli/cli/releases/latest
2. Look for: `gh_X.X.X_windows_amd64.msi` (X.X.X = version number)
3. Download and run that file
4. Restart computer
5. Try again

---

## VS Code + Claude Code Setup

### Installation Order (Critical!)

Do these in this **exact order**:

1. **Install Git** (see above)
2. **Install VS Code** from https://code.visualstudio.com/
3. **Install GitHub CLI** (see above)
4. **Restart computer** (important!)
5. **Install Claude Code extension**:
   - Open VS Code
   - Click Extensions icon (left sidebar, looks like blocks)
   - Search: "Claude Code"
   - Click "Install" on "Claude Code" by Anthropic
6. **Configure Claude Code**:
   - Restart VS Code
   - Follow setup prompts

### Common Issue: Extension Won't Install

**Symptoms:**
- Extension shows "Install" but nothing happens
- Error: "Unable to install extension"

**Fix:**

1. Close VS Code completely
2. Open File Explorer
3. Navigate to: `C:\Users\[YourName]\.vscode\extensions`
4. Delete any folders starting with `anthropic.claude-code`
5. Restart VS Code
6. Try installing extension again

### Common Issue: Claude Code Panel Won't Open

**Symptoms:**
- Extension installed but no Claude Code panel
- Can't find Claude Code anywhere

**Fix:**

1. Press `Ctrl + Shift + P` (Command Palette)
2. Type: "Claude Code"
3. Select: "Claude Code: Open Chat"
4. If that doesn't work:
   - View → Command Palette
   - Type: "reload window"
   - Select: "Developer: Reload Window"

---

## Windows File Paths

### Path Separators

**Windows uses backslashes:**
```
C:\Users\YourName\Documents\mindvalley-ai-mastery-students
```

**Mac/Linux use forward slashes:**
```
/Users/YourName/Documents/mindvalley-ai-mastery-students
```

**Claude Code handles both!**

When Claude Code shows you a path, you can use either:
- `C:\Users\YourName\...` - Windows style
- `C:/Users/YourName/...` - Also works on Windows!

### User Folder Location

**Windows:**
- Your user folder: `C:\Users\[YourName]`
- Documents: `C:\Users\[YourName]\Documents`
- Downloads: `C:\Users\[YourName]\Downloads`
- Desktop: `C:\Users\[YourName]\Desktop`

**In commands, use `~` for user folder:**
```
~/Documents/mindvalley-ai-mastery-students
```
This works on Windows, Mac, and Linux!

### Spaces in Path Names

If your Windows username has spaces (like "John Doe"), paths need quotes:

**Wrong:**
```
C:\Users\John Doe\Documents\repo
```

**Right:**
```
"C:\Users\John Doe\Documents\repo"
```

Claude Code will add quotes automatically, but good to know!

---

## PowerShell vs Git Bash

### Which One to Use?

**For this course: Use Git Bash** (comes with Git for Windows)

**Why?**
- Works exactly like Mac/Linux terminal
- All course commands work without changes
- Less confusion

### How to Use Git Bash

**Option 1: In VS Code (Recommended)**

1. Open VS Code
2. Press `` Ctrl + ` `` (backtick key, top left of keyboard)
3. This opens the terminal at the bottom
4. Click the dropdown next to "+" (top right of terminal panel)
5. Select: "Git Bash"
6. Now you have Git Bash in VS Code!

**Option 2: Standalone**

1. Press `Windows + S`
2. Type: "Git Bash"
3. Click: "Git Bash" app
4. A terminal window opens

### Setting Git Bash as Default in VS Code

1. Press `Ctrl + ,` (opens Settings)
2. Search: "default profile"
3. Click: "Terminal > Integrated > Default Profile: Windows"
4. Select: "Git Bash"
5. Close settings
6. New terminals will now use Git Bash automatically!

### Quick Command Reference

These commands work in Git Bash:

| Task | Command |
|------|---------|
| List files | `ls` |
| Change directory | `cd foldername` |
| Go to parent directory | `cd ..` |
| Go to home folder | `cd ~` |
| Show current location | `pwd` |
| Create folder | `mkdir foldername` |
| Clear screen | `clear` |

### PowerShell Users

If you prefer PowerShell, here are equivalent commands:

| Git Bash | PowerShell |
|----------|------------|
| `ls` | `dir` or `ls` (both work!) |
| `cd` | `cd` (same) |
| `pwd` | `pwd` (same) |
| `clear` | `cls` or `clear` |

---

## Permissions & Security Warnings

### "Windows protected your PC" Warning

**When installing Git, GitHub CLI, or VS Code:**

1. You might see a blue warning: "Windows protected your PC"
2. Click: "More info"
3. Click: "Run anyway"
4. This is normal for downloaded installers

### "Do you want to allow this app to make changes?"

**When installing software:**

1. Windows will ask for permission
2. Click: "Yes"
3. This is the User Account Control (UAC)
4. Normal and safe for official software

### Antivirus False Positives

**Some antivirus software flags Claude Code or Git:**

**If this happens:**

1. Claude Code and Git are safe (official software)
2. Add to antivirus exceptions:
   - `C:\Program Files\Git\`
   - `C:\Users\[YourName]\.vscode\extensions\`
3. Or temporarily disable antivirus during installation
4. Re-enable after setup complete

### Windows Defender SmartScreen

**If you see "Windows Defender SmartScreen prevented..."**

1. Click "More info"
2. Click "Run anyway"
3. This happens with newly-released software
4. Claude Code is official Anthropic software - safe to run

---

## Claude Code Prompts

Use these prompts to have Claude Code diagnose and fix Windows issues for you.

### Test Your Setup

```
I'm on Windows. Please check if I have:
1. Git installed
2. GitHub CLI installed
3. Correct terminal (Git Bash preferred)
4. Necessary permissions

Run any tests you need and tell me what's missing.
```

### Fix PATH Issues

```
I'm getting errors about bash or git not found on Windows.

Please:
1. Check my Git installation
2. Help me set the correct environment variables
3. Test that everything works
4. Explain what you fixed

My Windows version is: [10/11]
```

### Diagnose Installation Problems

```
I'm trying to set up for the MindValley AI Mastery course on Windows and something isn't working.

Here's what I've installed:
- [ ] Git
- [ ] VS Code
- [ ] GitHub CLI
- [ ] Claude Code extension

Here's the error I'm seeing:
[paste error message here]

Please help me diagnose and fix this step by step.
```

### Post-Setup Verification

```
I just completed the Windows setup for Claude Code. Please verify everything is working:

1. Test Git: git --version
2. Test GitHub CLI: gh --version
3. Test terminal is Git Bash
4. Confirm I can use Claude Code properly

Let me know if anything needs fixing.
```

---

## Emergency: Complete Reinstall

If you've tried everything and still stuck:

1. **Uninstall everything:**
   - Git
   - GitHub CLI
   - VS Code
   - Claude Code extension

2. **Restart computer**

3. **Reinstall in this order:**
   - Git for Windows
   - VS Code
   - GitHub CLI
   - Restart computer
   - Claude Code extension

4. **Use this Claude Code prompt:**
```
I just did a clean reinstall of all tools on Windows.
Please verify my setup is correct by running diagnostic tests.
Check: Git, GitHub CLI, VS Code, terminal configuration.
```

---

## Still Stuck?

1. **Check other docs:**
   - [Quick Reference](windows-quick-ref.md) - Checklist format
   - [Troubleshooting](troubleshooting-quick-ref.md) - General issues

2. **Post in Slack with:**
   - Your Windows version (10/11)
   - Error message (screenshot if possible)
   - What you've tried
   - What step you're stuck on

3. **Bring to Friday Office Hours** - Live help available

---

*This guide addresses the git-bash PATH error (Q14 from Friday's session) and other Windows-specific issues. Created December 2025.*
