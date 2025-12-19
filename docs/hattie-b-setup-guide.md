# Hattie B Email System - Complete Setup Guide

**Version:** 1.0
**Last Updated:** 2025-12-12
**System:** Email Processing Pipeline with Human-in-the-Loop

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Import Instructions](#import-instructions)
4. [Post-Import Configuration Checklist](#post-import-configuration-checklist)
5. [Critical Settings to Verify](#critical-settings-to-verify)
6. [Common Issues & Fixes](#common-issues--fixes)
7. [Testing Protocol](#testing-protocol)
8. [Troubleshooting](#troubleshooting)

---

## Overview

### What the Hattie B System Does

The Hattie B Email System is a complete AI-powered customer service pipeline that:

1. **Receives customer emails** automatically via Gmail
2. **Analyzes sentiment** using CinnaMon agent
3. **Retrieves knowledge** from your business documents via Librarian tool
4. **Drafts responses** using Sugar agent (in your brand voice)
5. **Quality checks** drafts using Bishop agent
6. **Escalates to humans** via Slack when AI needs help
7. **Handles human feedback** to revise or approve emails
8. **Logs everything** to Google Sheets for audit trail

### The Three Workflows

| Workflow | Purpose | Trigger |
|----------|---------|---------|
| **W1: Email Processing Pipeline** | Main automation - receives emails, drafts responses, checks quality | Gmail (new email arrives) |
| **W2: Approval Handler** | Processes human decisions from Slack | Slack (human replies to approval message) |
| **SUB: Revision Processor** | Handles revision requests with human feedback | Called by W2 when human chooses "revise" |

### The Six Agents

| Agent | Role | Location | Model |
|-------|------|----------|-------|
| **CinnaMon** | Sentiment analyzer - detects customer emotion | W1 | Claude Sonnet 4.5 |
| **Hatch** | Expert analyst - retrieves knowledge and context | W1 | Claude Sonnet 4.5 |
| **Sugar** | Email drafter - writes in your brand voice | W1, SUB | Claude Sonnet 4.5 |
| **Bishop** | QA reviewer - checks quality and accuracy | W1, SUB | Claude Sonnet 4.5 |
| **Holler** | HITL notifier - formats Slack messages | W1 | N8N formatting |
| **Librarian** | Knowledge retrieval tool (not an agent) | W1, SUB | Gemini 2.5 Flash |

---

## Prerequisites

Before importing workflows, ensure you have:

### Required Accounts & Services

- [ ] **N8N Cloud** - Starter tier or higher (AI Workflow Builder required)
- [ ] **Gmail account** - For receiving and sending emails
- [ ] **Google Sheets** - For logging drafts and tracking
- [ ] **Slack workspace** - For human approval notifications
- [ ] **Google AI Studio API key** - For Gemini File Search (Librarian)
- [ ] **LLM API key** - OpenRouter (recommended) or Claude/OpenAI API

### Required Setup Steps

1. **Google Sheets created** with exact column names (see below)
2. **Slack app configured** with bot token and channel ID
3. **Gmail labels created**: `AI-Replied` (for tracking sent emails)
4. **Gemini File Search corpus** created with business knowledge
5. **API credentials** ready to paste into N8N

### Google Sheets Setup

Create a new Google Sheet named "Email Drafts" with these exact columns (A through Q):

```
execution_id | gmail_thread_id | gmail_message_id | status | sender_email |
original_subject | original_body | draft_subject | draft_body | qa_score |
qa_status | revision_count | cinnamon_analysis | hatch_analysis |
created_at | updated_at | sent_at
```

**Important:** Column names must match exactly - the workflow looks for these specific headers.

---

## Import Instructions

### Step 1: Download Workflow Files

Workflow JSONs are located in the student repo at:
```
workflows/
├── w1-email-processing-pipeline-v1-YYYY-MM-DD.json
├── w2-approval-handler-v1-YYYY-MM-DD.json
└── sub-revision-processor-v1-YYYY-MM-DD.json
```

### Step 2: Import to N8N

For each workflow:

1. Log into your N8N Cloud instance
2. Click **"Add workflow"** (+ button in top-right)
3. Click the **three-dot menu (⋯)** → **"Import from file"**
4. Select the JSON file
5. Click **"Save"** (do NOT activate yet)

### Step 3: Import Order

Import in this order to avoid dependency errors:
1. **SUB** first (no dependencies)
2. **W2** second (may reference SUB)
3. **W1** last (main workflow)

---

## Post-Import Configuration Checklist

After importing all three workflows, configure credentials and settings in this order:

### W1: Email Processing Pipeline

**Workflow ID:** Check your n8n URL after opening the workflow

#### 1. Gmail Trigger Node
- [ ] **Credential:** Add your Gmail OAuth2 connection
- [ ] **Label:** Configure to watch inbox (or specific label)
- [ ] **Test:** Click "Fetch test event" - should show a recent email

#### 2. Settings Node (near start of workflow)
Verify these values:
```
QA_PASS_THRESHOLD: 70
MAX_REVISION_COUNT: 2
```

#### 3. All Model Nodes (7 total)
**CRITICAL:** Each model node MUST have `maxTokensToSample` configured:

| Node Name | Temperature | maxTokensToSample |
|-----------|-------------|-------------------|
| CinnaMon Model | 0.25 | 2000 |
| Hatch Model | 0.3 | 8000 |
| Sugar v1 Model | 0.7 | 8000 |
| Bishop v1 Model | 0 | 6000 |
| Sugar v2 Model | 0.7 | 8000 |
| Bishop v2 Model | 0 | 6000 |
| Holler Model | 0.7 | 2000 |

**How to set:**
1. Click the model node
2. In right panel, find **"Options"** section
3. Click **"Add Option"**
4. Select **"Max Tokens to Sample"**
5. Enter the value from table above
6. Save

**Why this matters:** Without maxTokensToSample, models will truncate output mid-sentence, causing Bishop to reject drafts.

#### 4. All Agent Nodes (5 total)
**CRITICAL:** Each agent node MUST have sufficient `maxIterations`:

| Node Name | maxIterations |
|-----------|---------------|
| Sugar v1 (Draft) | 10 |
| Bishop v1 (QA) | 10 |
| Sugar v2 (Revision) | 10 |
| Bishop v2 (Re-eval) | 10 |
| Holler (Briefer) | 5 |

**How to set:**
1. Click the agent node
2. In right panel, find **"Max Iterations"** field
3. Enter the value from table above
4. Save

**Why this matters:** Low maxIterations causes agents to timeout before completing their work.

#### 5. Librarian Tool Nodes (5 total)
Each needs Gemini API configuration:

- [ ] **Credential:** Google AI (Gemini) API key from aistudio.google.com
- [ ] **Model:** `gemini-2.5-flash` (MUST use Gemini - File Search is Gemini-only)
- [ ] **Corpus ID:** Your Gemini File Search corpus ID (from Google AI Studio)

**Find your corpus ID:**
1. Go to aistudio.google.com
2. Click "File Search" in left sidebar
3. Click on your corpus
4. Copy the ID from the URL (format: `corpora/abc123...`)

#### 6. Slack Nodes (2 total)
Both "Post to Slack" and "Escalate to Human" nodes:

- [ ] **Credential:** Slack OAuth2 (bot token starting with `xoxb-`)
- [ ] **Resource:** `message`
- [ ] **Operation:** `post`
- [ ] **Select:** `channel`
- [ ] **Channel ID:** Your Slack channel ID (e.g., `C0A0MBQ2L8P`)

**Get Slack channel ID:**
1. In Slack, right-click your channel name
2. Click "Copy link"
3. Channel ID is the last part of URL (starts with `C`)

**Verify Slack app permissions:**
Your Slack bot needs these scopes:
- `chat:write` (to post messages)
- `channels:read` (to list channels)

#### 7. Google Sheets Nodes (2 total)
Both "Log to Sheets" nodes:

- [ ] **Credential:** Google Sheets OAuth2
- [ ] **Operation:** `Append or Update Row`
- [ ] **Document ID:** Your Google Sheet ID (from URL)
- [ ] **Sheet Name:** `Sheet1` (or whatever you named your sheet)
- [ ] **Mapping:** Should show 17 fields (see below)

**Critical field mapping for "Prepare for Storage" nodes:**

The workflow outputs these 17 fields to Google Sheets:
```
execution_id, gmail_thread_id, gmail_message_id, status,
sender_email, original_subject, original_body, draft_subject,
draft_body, qa_score, qa_status, revision_count,
cinnamon_analysis, hatch_analysis, created_at, updated_at, sent_at
```

**Verify mapping:**
1. Click "Prepare for Storage" node
2. Check that all 17 fields are mapped correctly
3. Repeat for "Prepare for Storage v2" node

---

### W2: Approval Handler

#### 1. Slack Trigger Node
- [ ] **Credential:** Same Slack OAuth2 as W1
- [ ] **Events:** `message.channels`
- [ ] **Channel:** Same channel as W1 notifications

#### 2. Gmail Nodes (Send Email)
- [ ] **Credential:** Same Gmail OAuth2 as W1
- [ ] **Operation:** `send`
- [ ] **Add Label:** Configure to add `AI-Replied` label after sending

#### 3. Google Sheets Nodes (Update Status)
- [ ] **Credential:** Same Google Sheets OAuth2 as W1
- [ ] **Operation:** `Update Row`
- [ ] **Document ID:** Same sheet as W1
- [ ] **Match Column:** `execution_id`

#### 4. Bot Filter
**CRITICAL:** Block the Slack bot from triggering itself

Find the "Bot Filter" node and verify it checks:
```javascript
{{ $json.user !== "U0A0GJN18JK" }}
```

**Replace with YOUR bot's user ID:**
1. In Slack, click your bot's name
2. Click "View full profile"
3. Click the three dots → "Copy member ID"
4. Paste into the filter expression

#### 5. SUB Workflow Node (if present)
- [ ] **Workflow:** Select "SUB: Revision Processor" from dropdown
- [ ] **Data to Pass:** Should include `execution_id` and `human_feedback`

---

### SUB: Revision Processor

#### 1. Model Nodes (Sugar v2, Bishop v2)
Same configuration as W1:
- [ ] `maxTokensToSample` configured (8000 for Sugar, 6000 for Bishop)

#### 2. Agent Nodes (Sugar v2, Bishop v2)
Same configuration as W1:
- [ ] `maxIterations: 10` for both

#### 3. Librarian Tool Node
Same configuration as W1:
- [ ] Gemini API credential
- [ ] Corpus ID configured

---

## Critical Settings to Verify

After configuration, verify these settings that commonly cause failures:

### 1. Check All IF Nodes

| Node Name | Condition | Value |
|-----------|-----------|-------|
| QA Pass? | `{{ $json.qa_score_v1 >= 70 }}` | Must check `qa_score_v1` (not `qa_score`) |
| QA Pass v2? | `{{ $json.qa_score_v2 >= 70 }}` | Must check `qa_score_v2` |

**Common bug:** Using wrong field name causes false negatives.

### 2. Verify Connection Flow

Check that these paths are connected:

**v1 Success Path:**
```
QA Pass? (TRUE) → Prepare for Storage → Log to Sheets → Holler → Post to Slack → Success
```

**v2 Success Path:**
```
QA Pass v2? (TRUE) → Prepare for Storage v2 → Log to Sheets → Holler → Post to Slack → Success
```

**Escalation Path:**
```
QA Pass v2? (FALSE) → Escalate to Human → Escalation End
```

### 3. Check Expression Syntax

**N8N does NOT support optional chaining (`?.`)**

If you see expressions like:
```javascript
{{ $json.hatch_analysis?.recommended_approach?.tone }}
```

Replace with:
```javascript
{{ ($json.hatch_analysis || {}).recommended_approach?.tone }}
```

Or better yet:
```javascript
{{ (($json.hatch_analysis || {}).recommended_approach || {}).tone }}
```

### 4. Verify Holler Output Includes draft_id

The "Holler" node must output `[draft_id: {{ $json.execution_id }}]` in its message.

**Why:** W2 extracts this ID from Slack messages to retrieve the draft.

**Check:**
1. Click "Holler (Briefer)" node
2. In the prompt template, verify this text appears:
```
[draft_id: {{ $json.execution_id }}]
```

---

## Common Issues & Fixes

These are issues we discovered during development and their solutions:

### Issue 1: Token Starvation - Truncated Output

**Symptoms:**
- Sugar generates incomplete emails (cuts off mid-sentence)
- Bishop scores draft as 0/100
- Execution logs show partial responses

**Root Cause:** No `maxTokensToSample` set on model nodes

**Fix:** Add `maxTokensToSample` to all 7 model nodes (see table above)

---

### Issue 2: Agent Iteration Timeout

**Symptoms:**
- Agent nodes show "Max iterations reached" in execution logs
- Agents don't complete their thinking/planning phases
- Quality of output degrades

**Root Cause:** Default `maxIterations` (often 1-3) too low for complex tasks

**Fix:** Increase `maxIterations` to 10 for all agents (see table above)

---

### Issue 3: QA Threshold Mismatch

**Symptoms:**
- Drafts pass QA but trigger revision loop
- Workflow doesn't proceed to Slack notification

**Root Cause:** IF node checks wrong field or wrong threshold

**Fix:**
1. Open "QA Pass?" node
2. Verify condition: `{{ $json.qa_score_v1 >= 70 }}`
3. Open "QA Pass v2?" node
4. Verify condition: `{{ $json.qa_score_v2 >= 70 }}`
5. Open "Settings" node
6. Verify: `QA_PASS_THRESHOLD: 70`

---

### Issue 4: v2 Success Path Not Connected

**Symptoms:**
- v2 revisions succeed but don't appear in Slack or Sheets
- Workflow stops after Bishop v2 approval

**Root Cause:** Missing connection from "QA Pass v2? (TRUE)" to "Prepare for Storage v2"

**Fix:**
1. Click "QA Pass v2?" node
2. Find the TRUE output (green dot)
3. Drag to connect to "Prepare for Storage v2" input
4. Save workflow

---

### Issue 5: Google Sheets Missing Fields

**Symptoms:**
- Workflow fails at "Log to Sheets" with "Column not found" error
- Only 4 fields logged instead of 17

**Root Cause:** "Prepare for Storage" nodes not expanded to full 17-field mapping

**Fix:**
1. Click "Prepare for Storage" node
2. Verify it outputs all 17 fields (see checklist above)
3. Repeat for "Prepare for Storage v2"
4. Ensure Google Sheet has matching column headers

---

### Issue 6: Slack Bot Loops (W2 triggers itself)

**Symptoms:**
- W2 workflow runs continuously
- Slack channel fills with duplicate messages
- Multiple executions with same draft_id

**Root Cause:** Bot filter not configured with correct user ID

**Fix:**
1. Get YOUR bot's Slack user ID (see instructions above)
2. Update "Bot Filter" node condition:
```javascript
{{ $json.user !== "YOUR_BOT_USER_ID" }}
```

---

### Issue 7: Librarian Tool Node Type Error

**Symptoms:**
- Validation shows: `Unknown node type: n8n-nodes-base.httpRequestTool`
- Hatch can't search knowledge base

**Root Cause:** Incorrect node type in Librarian tool configuration

**Fix:**
This is a known issue from development. The workflow should still function - Hatch will use other context. To fully fix:
1. Check n8n community nodes for updated Librarian/KB search nodes
2. Or build custom tool using HTTP Request node with Gemini API

**Impact:** Non-blocking - affects Hatch quality but not workflow completion

---

### Issue 8: Optional Chaining Syntax Errors

**Symptoms:**
- Execution logs show `undefined` for nested values
- Expressions with `?.` don't work as expected

**Root Cause:** N8N doesn't fully support optional chaining in all contexts

**Fix:** Replace optional chaining with null-safe patterns:

**Before (may fail):**
```javascript
{{ $json.hatch_analysis?.recommended_approach?.tone }}
```

**After (safe):**
```javascript
{{ (($json.hatch_analysis || {}).recommended_approach || {}).tone }}
```

---

### Issue 9: W2 Can't Extract draft_id from Slack

**Symptoms:**
- W2 fails to find draft in Google Sheets
- Error: "No matching row found"

**Root Cause:** Holler output doesn't include `[draft_id: ...]` marker

**Fix:**
1. Open W1 workflow
2. Click "Holler (Briefer)" agent node
3. Add to user prompt template:
```
[draft_id: {{ $json.execution_id }}]
```
4. Save and test

---

## Testing Protocol

After configuration, test the complete system:

### Pre-Flight Checklist

Before sending test email:
- [ ] All credentials configured and tested
- [ ] Google Sheets has correct columns
- [ ] Slack bot invited to approval channel
- [ ] Gmail labels created
- [ ] Gemini corpus populated with knowledge
- [ ] All workflows saved (not activated yet)

### Test 1: W1 Email Processing

1. **Activate W1 workflow**
   - Open W1 in n8n
   - Click "Activate" toggle (top-right)

2. **Send test email**
   - Send email to your Gmail address
   - Subject: "Question about your menu"
   - Body: "Hi! Do you have gluten-free options? Thanks!"

3. **Monitor execution**
   - Go to "Executions" tab in n8n
   - Watch for new execution to appear
   - Click to view details

4. **Verify each stage:**
   - [ ] Email received (Gmail Trigger shows data)
   - [ ] CinnaMon analyzed sentiment
   - [ ] Hatch retrieved knowledge
   - [ ] Sugar generated draft
   - [ ] Bishop scored draft (check score ≥70 or <70)
   - [ ] If passed: Holler formatted notification
   - [ ] Slack message appeared in approval channel
   - [ ] Google Sheet received new row with 17 fields

**Expected outcome:**
- Slack notification with draft and approval buttons
- Sheet row with execution_id, draft, and qa_score

### Test 2: W2 Approval Flow

1. **Activate W2 workflow**

2. **Reply in Slack**
   - In the approval message thread, reply: `ship`

3. **Monitor W2 execution**
   - Check Executions for W2 workflow
   - Verify it triggered from Slack event

4. **Verify outcome:**
   - [ ] Gmail sent the email
   - [ ] `AI-Replied` label added to thread
   - [ ] Google Sheet row updated (status: "sent", sent_at: timestamp)

**Expected outcome:**
- Customer receives email
- Sheet shows "sent" status

### Test 3: Revision Flow

1. **Send another test email**

2. **When Slack notification arrives, reply:**
   ```
   revise: Make the response more friendly and include pricing info
   ```

3. **Monitor execution:**
   - [ ] W2 detects "revise" command
   - [ ] W2 calls SUB workflow
   - [ ] SUB runs Sugar v2 with feedback
   - [ ] SUB runs Bishop v2 on revision
   - [ ] If passed: New Slack notification with v2 draft

**Expected outcome:**
- Second Slack notification with revised draft
- Sheet shows revision_count: 1

### Test 4: Escalation Path

1. **Trigger a scenario that fails QA twice**
   - Send complex email (allergen concern + complaint)
   - If v1 fails → v2 runs
   - If v2 also fails → escalation

2. **Verify escalation:**
   - [ ] "Escalate to Human" Slack message appears
   - [ ] Different format (minimal alert vs full briefing)
   - [ ] Sheet shows status: "escalated" or "failed_qa_twice"

**Expected outcome:**
- Human gets notified that AI can't handle this
- Email not sent automatically

---

## Troubleshooting

### Execution Failed at Gmail Trigger

**Check:**
1. Gmail credential is OAuth2 (not API key)
2. Account has granted permissions
3. Re-authenticate if needed: Settings → Credentials → Gmail → Reconnect

### Execution Failed at Agent Node

**Check:**
1. `maxIterations` set (not 0 or blank)
2. `maxTokensToSample` set in linked model node
3. LLM API credential valid and has credit
4. Model ID correct (e.g., `anthropic/claude-sonnet-4-5`)

### Execution Failed at Google Sheets

**Check:**
1. Sheet ID correct (from URL between `/d/` and `/edit`)
2. Sheet name matches exactly (case-sensitive)
3. Column headers match field names exactly
4. OAuth2 credential has edit permissions

### Execution Failed at Slack

**Check:**
1. Bot user ID in filter is correct
2. Bot invited to channel (`/invite @YourBot`)
3. Channel ID correct (starts with `C`)
4. Bot has `chat:write` permission

### Slack Message Appears but No Buttons

**Check:**
1. W2 workflow is activated
2. Slack app has interactive messages enabled
3. Request URL configured (if using interactive components)

**Note:** Simple text replies like "ship" should work without interactive components.

### No Row Appears in Google Sheets

**Check:**
1. "Prepare for Storage" node executed (check execution graph)
2. "Log to Sheets" node executed after Prepare
3. Field mapping includes all 17 fields
4. Sheet permissions allow writes

### Draft Scores 0/100 Every Time

**Check:**
1. Sugar output is complete (not truncated) - check execution output
2. `maxTokensToSample` set on Sugar model nodes
3. Bishop prompt receiving full context (click Bishop node → see input)

### W2 Can't Find Draft by execution_id

**Check:**
1. Slack message includes `[draft_id: ...]` marker
2. "Parse Thread Data" node in W2 extracts ID correctly
3. Google Sheets "lookup" operation uses `execution_id` column (not `draft_id`)
4. execution_id format matches (no extra spaces/characters)

---

## Advanced Configuration

### Customizing Agent Prompts

To modify how agents behave:

1. **Export current prompts**
   - Click agent node → See system prompt in right panel
   - Copy to text file for backup

2. **Edit prompt**
   - Modify voice, tone, instructions
   - **DO NOT** change output format (Bishop expects specific JSON structure)

3. **Test changes**
   - Save workflow
   - Send test email
   - Verify agent still produces valid output

### Adjusting QA Thresholds

To make Bishop more/less strict:

1. Open "Settings" node
2. Change `QA_PASS_THRESHOLD` (default: 70)
   - Higher = stricter (fewer auto-approvals)
   - Lower = more lenient (more auto-approvals)
3. Update IF node conditions to match

### Adding Additional Gmail Labels

To tag emails at different stages:

1. Create labels in Gmail
2. Add "Gmail: Add Label" nodes after relevant stages
3. Configure with label name and message ID

---

## Getting Help

If you encounter issues not covered here:

1. **Check execution logs** - Click failed execution → see which node failed
2. **Check node output** - Click node → "Output" tab → see actual data
3. **Search this guide** - Use Ctrl/Cmd+F
4. **Ask in course Slack** - Include:
   - Which workflow (W1, W2, SUB)
   - Which node failed
   - Screenshot of error
   - Screenshot of node configuration

---

## Appendix: Field Reference

### Google Sheets Columns (17 total)

| Column | Type | Source | Description |
|--------|------|--------|-------------|
| execution_id | String | Generated | Unique ID for this email/draft |
| gmail_thread_id | String | Gmail | Thread ID for conversation |
| gmail_message_id | String | Gmail | Specific message ID |
| status | String | Workflow | draft/sent/failed_qa_twice/escalated |
| sender_email | String | Gmail | Customer's email address |
| original_subject | String | Gmail | Customer's subject line |
| original_body | String | Gmail | Customer's message body |
| draft_subject | String | Sugar | AI-generated subject |
| draft_body | String | Sugar | AI-generated email body |
| qa_score | Number | Bishop | QA score 0-100 (latest version) |
| qa_status | String | Bishop | PASS/FAIL |
| revision_count | Number | Workflow | How many revisions (0, 1, 2) |
| cinnamon_analysis | JSON String | CinnaMon | Sentiment analysis output |
| hatch_analysis | JSON String | Hatch | Expert analysis output |
| created_at | DateTime | Workflow | When draft was created |
| updated_at | DateTime | Workflow | When draft was last updated |
| sent_at | DateTime | W2 | When email was sent (null if not sent) |

### Agent Output Formats

**CinnaMon:**
```json
{
  "sentiment": "positive|negative|neutral|mixed",
  "emotional_spectrum": { "primary": "", "secondary": "", "intensity": 0-10 },
  "urgency": 1-5,
  "themes": ["theme1", "theme2"],
  "business_impact": "low|medium|high",
  "recommended_tone": "",
  "summary": ""
}
```

**Hatch:**
```json
{
  "customer_analysis": { "core_question": "", "emotional_state": "", "urgency": 1-5 },
  "information_retrieved": { "from_kb": [], "from_web": [] },
  "recommended_approach": { "tone": "", "key_points": [], "resolution": "", "caveats": [] },
  "draft_guidance": "",
  "temporal_flags": { "time_sensitive_info_used": true|false, "verified_via": "", "caveats": [] }
}
```

**Sugar:**
```json
{
  "email_draft": {
    "subject": "",
    "body": "",
    "tone_assessment": { "scenario": "", "sliders": {}, "emotional_match": "" },
    "confidence": { "score": 0.0-1.0, "reasoning": "" },
    "version": "v1|v2"
  }
}
```

**Bishop:**
```json
{
  "evaluation_report": {
    "overall_status": "PASS|FAIL",
    "overall_score": 0-100,
    "rubric_scores": {
      "SafetyCompliance": { "score": 1-5, "weight": 25, "notes": "" },
      "BrandVoiceCompliance": { "score": 1-5, "weight": 25, "notes": "" },
      "FactualAccuracy": { "score": 1-5, "weight": 20, "notes": "" },
      "CustomerImpactCompliance": { "score": 1-5, "weight": 15, "notes": "" },
      "ToneAppropriatenessCompliance": { "score": 1-5, "weight": 15, "notes": "" }
    },
    "findings": [],
    "summary": "",
    "revision_guidance": ""
  }
}
```

---

## Version History

**v1.0 (2025-12-12)**
- Initial release
- Documents fixes from TASK-089 (token starvation, iteration timeout, data flow bugs)
- Includes W1, W2, and SUB workflows
- Based on production deployment and student testing

---

**End of Guide**

Questions? Ask in the course Slack channel or during office hours.
