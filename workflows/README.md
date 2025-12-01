# N8N Workflows

This folder contains all the N8N workflow JSONs you'll use throughout the course. Import them into your N8N Cloud instance.

## How to Import

1. Open N8N Cloud
2. Click **Add Workflow** > **Import from File**
3. Select the JSON file
4. Click **Import**
5. Configure credentials when prompted

## Workflow Index

### Getting Started
| File | Purpose | Session |
|------|---------|---------|
| `00-test-connection.json` | Verify N8N is working | Pre-class |

### Core System Workflows
| File | Purpose | Session |
|------|---------|---------|
| `gemini-ingestion-engine-v2-2025-11-27.json` | Ingest documents into Gemini File Search | 1 |
| `echo-brand-voice-v1-2025-11-27.json` | Analyze and match brand voice | 1-2 |
| `email-filter-v1-2025-11-27.json` | Classify incoming emails | 2 |
| `librarian-tool-v1-2025-11-27.json` | Search knowledge base | 2 |

### KB Management (Gemini File Search)
| File | Purpose | Session |
|------|---------|---------|
| `gemini-drive-watcher-v1-2025-11-27.json` | Auto-sync Google Drive to KB | 1 |
| `gemini-list-documents-v1-2025-11-27.json` | List docs in knowledge base | 1 |
| `gemini-delete-document-v1-2025-11-27.json` | Remove docs from KB | 1 |

### Stacks Proxy Workflows (KB Management UI)
| File | Purpose | Session |
|------|---------|---------|
| `gemini-create-store-v1-2025-11-28.json` | Create new knowledge store | 1 |
| `gemini-upload-document-v1-2025-11-28.json` | Upload document to store | 1 |
| `gemini-list-stores-v1-2025-11-28.json` | List all stores | 1 |
| `gemini-list-documents-v2-2025-11-28.json` | List docs (v2 with pagination) | 1 |
| `gemini-delete-document-v2-2025-11-28.json` | Delete document (v2) | 1 |

### Human-in-the-Loop (HITL) System
| File | Purpose | Session |
|------|---------|---------|
| `w1-email-processing-pipeline-v1-2025-11-28.json` | Main email processing pipeline | 3 |
| `w2-approval-handler-v1-2025-11-28.json` | Handle Slack approvals | 3 |
| `sub-revision-processor-v1-2025-11-28.json` | Process revision requests | 3 |

## Naming Convention

All workflows follow: `{purpose}-v{version}-{date}.json`

- **purpose**: What the workflow does
- **version**: Increment when workflow changes
- **date**: When this version was created

## Credential Requirements

Most workflows require these credentials (configured in N8N):

| Credential | Used By |
|------------|---------|
| Google Gemini API | All `gemini-*` workflows |
| Slack OAuth | HITL approval workflows |
| Gmail OAuth | Email processing workflows |

See [docs/api-keys-setup.md](../docs/api-keys-setup.md) for setup instructions.

## Demo Workflows

The `demo/hattieb/workflows/` folder contains complete working examples specific to the Hattie B's Hot Chicken demo case.
