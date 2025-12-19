# Windows Quick Reference

Fast checklist and common fixes for Windows users.

**Full guide:** [windows-troubleshooting.md](windows-troubleshooting.md)

---

## Most Common Windows Issues

| Issue | Quick Fix |
|-------|-----------|
| **Git-bash PATH error** | Set environment variable: `CLAUDE_CODE_GIT_BASH_PATH` |
| **GitHub CLI not found** | Download from https://cli.github.com/ |
| **Claude Code won't start** | Restart VS Code after installing Git |
| **Paths don't work** | Use forward slashes: `C:/Users/...` |
| **Can't clone repo** | Install GitHub CLI, then restart VS Code |

---

## Setup Checklist for Windows

**Do these in order:**

- [ ] Install Git for Windows (https://git-scm.com/download/win)
- [ ] Install VS Code (https://code.visualstudio.com)
- [ ] Install GitHub CLI (https://cli.github.com)
- [ ] Restart computer
- [ ] Install Claude Code extension
- [ ] Set environment variable (if git-bash error)
- [ ] Set Git Bash as default terminal in VS Code
- [ ] Test with: "Claude, show me the current directory"

---

## Git-Bash Error Fix (1 Minute)

1. Press `Windows + S`, type "environment variables"
2. Click "Edit the system environment variables"
3. Click "Environment Variables" button
4. Under "User variables", click "New"
5. Name: `CLAUDE_CODE_GIT_BASH_PATH`
6. Value: `C:\Program Files\Git\bin\bash.exe`
7. Click OK three times
8. Restart VS Code

---

## Set Git Bash as Default Terminal

1. In VS Code, press `Ctrl + ,`
2. Search: "default profile"
3. Set "Terminal > Integrated > Default Profile: Windows" to "Git Bash"

---

## Windows-Specific Claude Code Prompts

### Test Setup
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
[paste error message or screenshot]

Please help me diagnose and fix this step by step.
```

---

## Emergency: Nothing Works

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

## Path Quick Reference

| Windows Style | Also Works |
|---------------|------------|
| `C:\Users\Name\...` | `C:/Users/Name/...` |
| `\` (backslash) | `/` (forward slash) |
| `%USERPROFILE%` | `~` (tilde) |

---

## Common Commands (Git Bash)

| Task | Command |
|------|---------|
| List files | `ls` |
| Change directory | `cd folder` |
| Go up one folder | `cd ..` |
| Go to home | `cd ~` |
| Show location | `pwd` |
| Clear screen | `clear` |

---

## Still Stuck?

1. **Full guide:** [windows-troubleshooting.md](windows-troubleshooting.md)
2. **Post in Slack** with error message + Windows version
3. **Bring to Friday Office Hours**

---

*Quick reference for Windows users. See full guide for detailed steps.*
