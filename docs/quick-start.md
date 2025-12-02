# Quick Start (5 Minutes)

Get up and running fast. For detailed setup, see [SETUP.md](../SETUP.md).

## Step 1: Get the Repository

**Option A: GitHub CLI (Recommended)**
```bash
mkdir -p ~/GitHub && cd ~/GitHub
gh repo clone 8Dvibes/mindvalley-ai-mastery-students
cd mindvalley-ai-mastery-students
```

**Option B: ZIP Download**
1. Go to https://github.com/8Dvibes/mindvalley-ai-mastery-students
2. Click green "Code" button → "Download ZIP"
3. Unzip to ~/GitHub/ (NOT Downloads!)
4. Open the folder in VS Code

## Step 2: Verify Your Environment

```bash
./setup.sh
```

This checks:
- Required files are present
- Directory structure is correct
- No broken dependencies

**Expected output:** All checks pass (green checkmarks)

## Step 3: Test N8N Connection

1. Open [N8N Cloud](https://app.n8n.cloud)
2. Click **Add Workflow** > **Import from File**
3. Select `workflows/00-test-connection.json`
4. Click **Import**
5. Click **Test Workflow** (play button)
6. See "Success" in the output

## Step 4: Verify API Access

Test your Gemini API key:

1. Open [Google AI Studio](https://aistudio.google.com)
2. Click **Get API Key**
3. Create a key (or copy existing)
4. Paste into N8N credential (Google Gemini API)

## You're Ready!

If all steps completed successfully, you're ready for class.

### Next Steps

- Review [ONBOARDING.md](../ONBOARDING.md) - 60-90 minute full setup
- Browse `demo/hattieb/` - See a complete working example
- Check `guides/session-1/` - Read Session 1 materials

### Troubleshooting

| Issue | Solution |
|-------|----------|
| `setup.sh` permission denied | Run `chmod +x setup.sh` first |
| N8N import fails | Check JSON file isn't corrupted |
| Gemini API error | Verify billing is enabled on Google Cloud |
| "Credential not found" in N8N | Create credential before testing workflow |

See [troubleshooting-quick-ref.md](troubleshooting-quick-ref.md) for more.
