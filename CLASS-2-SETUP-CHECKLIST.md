# Class 2 Setup Checklist
## Session 2: Human-in-the-Loop Systems & Brand Voice

**Date Created**: December 26, 2025
**Student**: Dr. Amit Patel
**Status**: Starting Setup

---

## ✅ What You've Already Done

1. ✅ **Knowledge Base Created** - Corpus ID: `corpora/dr-amit-patel-facial-plasti-4f1zf2fyv7w9`
2. ✅ **24 Post-Op PDFs Uploaded** to Gemini File Search
3. ✅ **Gemini API Key** configured (new key rotated for security)
4. ✅ **N8N Account** active at https://apmd.app.n8n.cloud
5. ✅ **OpenRouter API Key** configured
6. ✅ **Slack Workspace** set up

---

## 📋 Class 2 Overview

**What You're Building:**
- **Echo Brand Voice Analyzer** - Analyzes your writing to create a brand voice profile
- **W2 Approval Handler** - Human-in-the-loop system for reviewing AI-generated email drafts
- **Gmail Integration** - Send/receive emails through n8n
- **Google Sheets** - Track approvals and customer interactions
- **Slack Integration** - Get notified when emails need approval

---

## 🔧 Required N8N Workflows

### Echo Workflows (Brand Voice Analysis)
1. **Echo Trigger** (`echo-trigger-v2-2025-12-08.json`)
   - Receives your writing samples (megadoc)
   - Triggers the brand voice analysis

2. **Echo Processor** (`echo-processor-v2-2025-12-10.json`) ⚠️ USE THIS VERSION
   - Analyzes 14 dimensions of your writing style
   - Generates brand voice XML profile
   - **IMPORTANT**: Use Dec 10 version (has auth fixes)

### W2 Approval Handler
3. **W2 Approval Handler** (`w2-approval-handler-2025-12-12.json`) ⚠️ USE THIS VERSION
   - Reviews AI-generated email drafts
   - Sends to Slack for your approval
   - Sends approved emails via Gmail

---

## 🎯 Setup Tasks

### Task 1: Import N8N Workflows ⏰ 15 minutes

**Files to import** (in this order):
1. `workflows/echo-trigger-v2-2025-12-08.json`
2. `workflows/echo-processor-v2-2025-12-10.json`
3. `workflows/w2-approval-handler-2025-12-12.json`

**How to import:**
1. Go to https://apmd.app.n8n.cloud
2. Click "Add Workflow" → "Import from File"
3. Select the JSON file
4. Click Import
5. **Activate** the workflow (toggle in top-right)

**Customization needed in each workflow:**
- ✏️ Replace `student@example.com` with `amit@amitpatelmd.com`
- ✏️ Update OpenRouter API key references
- ✏️ Configure Gmail OAuth credentials
- ✏️ Configure Google Sheets credentials
- ✏️ Configure Slack webhook

---

### Task 2: Configure Gmail OAuth ⏰ 10 minutes

**Which Gmail account to use:**
- ✅ **Primary**: `amit@amitpatelmd.com` (for sending/receiving customer emails)

**Steps:**
1. In n8n, go to **Credentials** → **Add Credential**
2. Select **Gmail OAuth2 API**
3. Click "Connect my account"
4. Sign in with `amit@amitpatelmd.com`
5. Grant permissions
6. Test the connection

**Where it's used:**
- W2 Approval Handler (sending approved emails)
- Echo workflows (receiving brand voice results)

---

### Task 3: Configure Google Sheets ⏰ 10 minutes

**Purpose:**
- Track customer emails
- Log approval decisions
- Store customer interaction history

**Steps:**
1. Create a new Google Sheet named: `AI Customer Service Tracker`
2. Add these column headers:
   ```
   Date | Customer Email | Subject | AI Draft | Status | Approved By | Notes
   ```
3. In n8n, add **Google Sheets** credential
4. Select OAuth2
5. Connect with `amit@amitpatelmd.com`
6. Copy the Sheet ID from the URL
7. Update workflows with your Sheet ID

**Sheet URL Format:**
`https://docs.google.com/spreadsheets/d/[SHEET_ID]/edit`

---

### Task 4: Configure Slack Integration ⏰ 5 minutes

**Which Slack account:**
- ✅ **Workspace**: AI Mastery workspace
- ✅ **Email**: `patelassist@gmail.com`
- ✅ **Channel**: `#customer-service-hitl`

**What's already configured:**
- Slack workspace URL saved in .env
- Channel created

**What you need to do:**
1. In Slack, go to your workspace
2. Create a Slack webhook for `#customer-service-hitl` channel
3. Copy the webhook URL
4. In n8n W2 Approval Handler workflow:
   - Find the Slack node
   - Paste your webhook URL
   - Test the connection

---

### Task 5: Create Your Brand Voice Megadoc ⏰ 30 minutes

**What is a megadoc?**
A collection of YOUR authentic writing samples (not AI-generated) so Echo can learn your voice.

**What to include:**
- ✅ Emails you've written to patients (remove PHI!)
- ✅ Instagram captions you wrote
- ✅ Blog posts or website text you wrote
- ✅ Slack/text messages to your team
- ✅ Patient FAQs you answered

**What NOT to include:**
- ❌ AI-generated content
- ❌ Templates or boilerplate text
- ❌ Other people's writing
- ❌ Protected Health Information (PHI)

**Format:**
```
# Dr. Amit Patel - Writing Samples for Brand Voice Analysis

## Sample 1: Patient Email Response
[Your actual email here...]

## Sample 2: Instagram Post
[Your actual caption here...]

## Sample 3: FAQ Answer
[Your actual answer here...]

... continue with 10-15 samples ...
```

**Where to save it:**
`~/Desktop/megadoc-amit-patel.txt`

---

## 🔍 Workflow Customization Checklist

### Echo Trigger Workflow
- [ ] Replace sample email with `amit@amitpatelmd.com`
- [ ] Configure Gmail OAuth credential
- [ ] Test with sample document

### Echo Processor Workflow
- [ ] Update OpenRouter API key (already in .env)
- [ ] Verify output email is `amit@amitpatelmd.com`
- [ ] Test brand voice analysis with megadoc

### W2 Approval Handler Workflow
- [ ] Replace `student@example.com` with `amit@amitpatelmd.com`
- [ ] Configure Gmail OAuth for sending
- [ ] Configure Google Sheets credential
- [ ] Update Sheet ID to your tracker sheet
- [ ] Configure Slack webhook for `#customer-service-hitl`
- [ ] Update Slack channel name
- [ ] Test approval flow end-to-end

---

## 📧 Account Summary

| Service | Account | Purpose |
|---------|---------|---------|
| **Gmail** | amit@amitpatelmd.com | Customer emails, Echo results |
| **Google Sheets** | amit@amitpatelmd.com | Track approvals & interactions |
| **Slack** | patelassist@gmail.com | Approval notifications |
| **N8N** | amit@amitpatelmd.com | Workflow automation |
| **Gemini** | (API Key) | Knowledge base queries |
| **OpenRouter** | (API Key) | Claude API access |

---

## 🚀 Testing Your Setup

### Test 1: Echo Brand Voice Analysis
1. Create megadoc with 10-15 writing samples
2. Submit via Echo Trigger form
3. Wait 5-10 minutes
4. Check `amit@amitpatelmd.com` for email with:
   - `brand-voice-profile.xml`
   - `full-analysis.md`
   - `quick-reference.md`

### Test 2: W2 Approval Handler
1. Send test email to your Gmail
2. AI drafts response
3. Slack notification appears in `#customer-service-hitl`
4. Approve in Slack
5. Email sends automatically
6. Logs in Google Sheet

---

## 📝 Next Steps After Setup

1. ✅ Complete all workflow imports
2. ✅ Configure all OAuth credentials
3. ✅ Create and submit megadoc
4. ✅ Test Echo brand voice analysis
5. ✅ Test W2 approval flow
6. 📚 Integrate brand voice XML into your agent prompts

---

## 🆘 If You Get Stuck

**Troubleshooting docs:**
- `docs/qa-dec10-comprehensive.md` - All known issues and fixes
- `docs/troubleshooting-quick-ref.md` - Quick reference table
- `docs/troubleshooting/echo-workflow-v2-update.md` - Echo-specific fixes

**Ask Claude Code:**
```
I'm stuck on [describe issue]. Can you help me troubleshoot based on the
docs in this repo?
```

---

## ✨ What Success Looks Like

By the end of Class 2 setup, you should have:

1. ✅ **Brand Voice Profile** - XML file that captures your writing style
2. ✅ **Automated Email Workflow** - AI drafts → You approve → Auto-sends
3. ✅ **Human-in-the-Loop** - You control what gets sent
4. ✅ **Tracking System** - All interactions logged in Google Sheets
5. ✅ **Slack Notifications** - Get pinged when approval needed

**Your AI assistant will:**
- Write emails that sound like YOU
- Reference your knowledge base (24 post-op PDFs)
- Wait for your approval before sending
- Learn from your corrections

---

*Created: 2025-12-26 | Updated: 2025-12-26*
*Dr. Amit Patel - AI Mastery Class 2 Setup*
