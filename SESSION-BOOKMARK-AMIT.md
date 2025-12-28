# Session Bookmark - Amit's Stacks Troubleshooting
**Date**: December 25, 2025 (late evening)
**Status**: PAUSED - Resume here next session

## Context
- Amit just finished Class 2 (Session on Dec 10 - Human-in-the-Loop Systems)
- Encountered issues with **Stacks** (KB Manager) during class exercises
- We were investigating n8n/Stacks integration problems

## What We Discovered

### Repository Status
- ✅ Repo is up-to-date with latest fixes (commit 4c72ac7 - Dec 12)
- ✅ All troubleshooting docs are present
- ✅ Team has fixed major Stacks issues (Dec 8-10)

### Key Troubleshooting Documents Found
1. **docs/qa-dec10-comprehensive.md** (lines 114-187) - Stacks issues FIXED
2. **tools/kb-manager/README.md** - Architecture & quick troubleshooting
3. **tools/kb-manager/how-it-works.md** (line 200+) - Complete troubleshooting guide

### Required N8N Workflows (All Present)
- ✅ `gemini-create-store-v1-2025-11-28.json`
- ✅ `gemini-upload-document-v1-2025-11-28.json`
- ✅ `gemini-list-stores-v1-2025-11-28.json`
- ✅ `gemini-list-documents-v2-2025-11-28.json`
- ✅ `gemini-delete-document-v2-2025-11-28.json`

## Where We Left Off

**NEED TO ASK AMIT:**
What specific Stacks error are you seeing?
- "Failed to fetch"?
- "Can't create store"?
- "Upload fails"?
- Something else?

## Next Steps When Resuming

1. **Get specific error details** from Amit
2. **Verify N8N configuration:**
   - Check webhook URL format (should be: `https://your-workspace.n8n.cloud/webhook`)
   - Verify Gemini API key is valid
   - Confirm all 5 Gemini workflows are imported AND active in N8N
3. **Test the connection** using troubleshooting prompts from docs/qa-dec10-comprehensive.md
4. **Alternative solution:** If webhooks fail, use Direct API mode (bypass Stacks UI)

## Important Notes
- Amit has family coming in the morning
- Session was late evening - needs to get to bed
- Wants to pick up exactly here without "reinventing the wheel"

## How to Resume

**Prompt for next session:**
```
Hey Claude, I'm back to continue working on the Stacks/n8n issue from Class 2.
Please read SESSION-BOOKMARK-AMIT.md to see where we left off.

The specific error I'm seeing is: [DESCRIBE ERROR HERE]
```

---

*Bookmark created: 2025-12-25 late evening*
*Ready to resume troubleshooting when Amit returns*
