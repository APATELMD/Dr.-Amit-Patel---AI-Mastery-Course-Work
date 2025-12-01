# Onboarding Guide

> Complete these steps to arrive at class ready to build.
> **Estimated time:** 42 minutes

---

## Before You Start

- [ ] Complete the [Pre-Class Checklist](SETUP.md) first
- [ ] Have your API keys and passwords ready
- [ ] Set aside 45 uninterrupted minutes

---

## Step 1: Clone the Repo (2 min)

Open your terminal (Mac: Terminal app, Windows: Command Prompt or PowerShell).

```bash
git clone https://github.com/8Dvibes/mindvalley-ai-mastery-students.git
cd mindvalley-ai-mastery-students
```

**Verify it worked:**
```bash
ls -la
```

You should see:
```
SETUP.md
ONBOARDING.md
setup.sh
docs/
workflows/
```

**Troubleshooting:**
- `git: command not found` → Install Git from [git-scm.com](https://git-scm.com/downloads)
- `Repository not found` → Check the URL, or ask in Slack for access

---

## Step 2: Run Setup Script (1 min)

```bash
chmod +x setup.sh
./setup.sh
```

**Expected output:**
```
========================================
  MindValley AI Mastery - Setup Check
========================================

Checking directory structure...
   ✓ docs/ exists
   ✓ workflows/ exists

Checking required files...
   ✓ SETUP.md
   ✓ ONBOARDING.md

✅ Setup verified! You're ready for Step 3.
```

**Troubleshooting:**
- `Permission denied` → Run `chmod +x setup.sh` first
- Missing directories → Re-run `git clone` command from Step 1

---

## Step 3: Configure Claude Desktop (5 min)

> Skip this step if you're using ChatGPT Plus instead of Claude Pro.

### 3.1 Open Claude Desktop
- Launch the Claude Desktop app (not the web browser version)
- If not installed, download from [claude.ai/download](https://claude.ai/download)

### 3.2 Add Project Instructions
1. Click the **folder icon** in the bottom-left (or go to Settings → Projects)
2. Create a new project called "AI Mastery"
3. Click "Add custom instructions"
4. Copy the contents of `docs/claude-desktop-instructions.txt` and paste

### 3.3 Verify Setup
- You should see "AI Mastery" in your projects sidebar
- Claude now understands your course context

**Troubleshooting:**
- Can't find Projects? → Update Claude Desktop to the latest version
- Instructions too long? → Paste in sections, starting with the "Core Context" section

---

## Step 4: Configure Claude Code (5 min)

### 4.1 Open VS Code
1. Launch VS Code
2. File → Open Folder → select `mindvalley-ai-mastery-students`

### 4.2 Verify Claude Code Extension
1. Look for the Claude Code icon in the left sidebar (speech bubble icon)
2. Click it to open the Claude Code panel
3. Sign in if prompted

### 4.3 Test Claude Code
In the Claude Code input box, type:
```
/help
```

**Expected:** You should see a help menu with available commands.

**Troubleshooting:**
- No Claude icon? → Go to Extensions, search "Claude Code", install
- Sign in failed? → Verify Claude Pro subscription is active at claude.ai
- Extension not responding? → Restart VS Code

---

## Step 5: Test Bridge Round-Trip (15 min)

The "bridge" is how you'll work throughout this course: You orchestrate (decide what to build), AI executes (writes the code and configs).

### 5.1 Read the Sample Task
Open `docs/sample-task-spec.md` in VS Code.

This is a task specification - the format you'll use to give AI clear instructions.

### 5.2 Execute with Claude Code
1. Open Claude Code panel (click the icon)
2. Paste this prompt:

```
Read the task spec at docs/sample-task-spec.md and execute it.
Write the results to docs/faq-draft.md
```

3. Let Claude Code work. It will:
   - Read the task spec
   - Research your company's FAQs (or create examples)
   - Write the output file

### 5.3 Verify Results
1. Check that `docs/faq-draft.md` was created
2. Review the content
3. This is the workflow you'll use throughout the course!

**Expected time:** 10-15 minutes (Claude does the work)

**Troubleshooting:**
- Claude Code doesn't respond? → Restart VS Code
- "Permission denied" errors? → Check file permissions in the folder
- Results file not created? → Ask Claude Code "What went wrong?"

---

## Step 6: Verify N8N Connection (10 min)

### 6.1 Log into N8N Cloud
1. Go to your N8N instance URL (e.g., `https://your-name.app.n8n.cloud`)
2. Log in with your credentials

### 6.2 Import Test Workflow
1. Click **Add workflow** (or the + button)
2. Click the menu (···) → **Import from file**
3. Select `workflows/00-test-connection.json` from your cloned repo

### 6.3 Run the Workflow
1. Click **Test workflow** button (or click the play icon)
2. The workflow runs immediately (it's a manual trigger)

### 6.4 Verify Success
**Expected output:**
```json
{
  "message": "Hello from N8N!",
  "status": "connected",
  "workflow": "Test Connection Successful"
}
```

You should see these three fields in the output panel of the "Create Success Message" node.

**Troubleshooting:**
- Import fails? → Check N8N plan (Starter tier required)
- Workflow errors? → Screenshot the error, ask in Slack
- "Node not found"? → You may need to install community nodes (Settings → Community Nodes)

---

## Step 7: Verify Gemini Setup (5 min)

### 7.1 Get Your API Key
1. Go to [aistudio.google.com](https://aistudio.google.com)
2. Click **Get API Key** in the left sidebar
3. Create a new key (or copy existing)

### 7.2 Test the API
Open your terminal and run:

```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=YOUR_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{"contents":[{"parts":[{"text":"Say hello in exactly 3 words"}]}]}'
```

Replace `YOUR_API_KEY` with your actual key.

**Expected output:**
```json
{
  "candidates": [{
    "content": {
      "parts": [{"text": "Hello to you!"}]
    }
  }]
}
```

### 7.3 Save Your Key
Store your API key somewhere secure:
- Password manager (recommended)
- Encrypted notes app
- `.env` file (we'll set this up in class)

**NEVER commit API keys to Git!**

**Troubleshooting:**
- `400 Bad Request` → Check for extra spaces in the API key
- `403 Forbidden` → Verify key is from aistudio.google.com
- `curl: command not found` → Install curl or use an API testing tool like Postman

---

## ✅ You're Ready!

Congratulations! You've completed the onboarding process.

### What you've verified:
- [x] Repository cloned and folder structure correct
- [x] Claude Desktop configured with project context
- [x] Claude Code working in VS Code
- [x] Bridge workflow tested (task spec → execution → results)
- [x] N8N Cloud connected and workflows importable
- [x] Gemini API key working

### What to expect in Session 1:
1. We'll populate your knowledge base with real content
2. You'll customize the agent prompts for your business
3. You'll send your first AI-handled customer email

### Need help before class?
- Post in the course Slack channel
- Include screenshots of any error messages
- Tag Tyler for urgent issues

---

**Total time:** ~42 minutes

See you in Session 1!
