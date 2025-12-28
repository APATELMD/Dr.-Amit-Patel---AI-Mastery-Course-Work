# Knowledge Base Preparation Checklist

**Goal**: Prepare 5-10 documents to upload to Google AI Studio (Gemini) during Session 1

**Deadline**: Before Dec 3, 2025 (Session 1)

---

## What is the Knowledge Base?

Think of it as the AI's "reference library." When a patient emails asking a question, the AI searches these documents to find accurate information before drafting a response.

**The better your knowledge base = The better your AI responses**

---

## Document Categories to Prepare

### ✅ Category 1: FAQs (Frequently Asked Questions)

**Status**: Draft started in `docs/faq-draft.md`

**Action Items**:
- [ ] Review the current FAQ draft
- [ ] Add any missing common questions
- [ ] Verify all answers are accurate and current
- [ ] Include questions about:
  - Consultation process
  - Pre-operative preparation
  - Post-operative care and recovery
  - Insurance and payment
  - Results and expectations
  - Emergency contact procedures

**Format**: Question & Answer pairs (see `docs/faq-draft.md` for template)

---

### 📋 Category 2: SOPs (Standard Operating Procedures)

**What to include**:
- [ ] **Patient intake procedure**
  - How new patients schedule
  - What to bring to first appointment
  - Consultation process flow

- [ ] **Pre-operative checklist**
  - Medication restrictions
  - Pre-op testing required
  - Day-of-surgery instructions

- [ ] **Post-operative care protocols**
  - Wound care instructions by procedure type
  - Pain management guidelines
  - Activity restrictions by procedure
  - When to call the office vs. go to ER

- [ ] **Follow-up schedule**
  - Standard follow-up appointment timeline
  - What to expect at each visit

**Format**: Step-by-step instructions or bullet points

---

### 🏥 Category 3: Practice Information

**What to include**:
- [ ] **Services offered**
  - List of procedures (rhinoplasty, facelift, blepharoplasty, etc.)
  - Brief description of each procedure
  - Typical candidates

- [ ] **Office logistics**
  - Office hours
  - Location(s) and directions
  - Parking information
  - How to contact (phone, email, patient portal)

- [ ] **Team directory**
  - Dr. Patel's credentials and specializations
  - Key staff roles (who handles scheduling, insurance, etc.)
  - Emergency contact information

- [ ] **Insurance & Payment**
  - Which insurance plans accepted
  - Payment methods accepted
  - Financing options (CareCredit, etc.)
  - Cost estimates (if you share this info)

**Format**: Organized lists or short paragraphs

---

### 📜 Category 4: Policies

**What to include**:
- [ ] **Scheduling policies**
  - How far in advance to book
  - Cancellation policy
  - No-show policy
  - Rescheduling process

- [ ] **Payment policies**
  - When payment is due
  - Deposit requirements
  - Refund policy (if applicable)

- [ ] **Privacy & Communication**
  - HIPAA compliance statement
  - Secure communication preferences
  - Photo consent (before/after photos)

**Format**: Clear policy statements

---

### 🎯 Category 5: Brand Voice & Examples

**What to include**:
- [ ] **Brand voice guidelines**
  - Tone: Professional? Warm? Clinical?
  - Vocabulary to use/avoid
  - How to address patients

- [ ] **Example emails Dr. Patel has written**
  - Good examples of responses to common questions
  - Shows the AI how to match the practice's style

**Format**: Sample emails or style guide

---

## How to Organize These Documents

### Option 1: Separate Files (Recommended)
Create individual files for each category:
```
knowledge-base/
├── faqs.md
├── pre-op-instructions.md
├── post-op-care.md
├── services-offered.md
├── office-information.md
├── insurance-and-payment.md
└── policies.md
```

### Option 2: One Master Document
Combine everything into sections in one large document.

**Either way works** - Google AI Studio can handle both!

---

## Quality Checklist

Before uploading to the knowledge base, verify each document:

- [ ] **Accurate**: All information is current and correct
- [ ] **Complete**: Covers the topic thoroughly
- [ ] **Clear**: Written in plain language (AI will reference this exactly)
- [ ] **Compliant**: Follows HIPAA and medical practice regulations
- [ ] **Consistent**: Matches other documents (no conflicting info)

---

## Where to Save These Documents

**Recommended location**:
```
~/Developer/Mindvalley/mindvalley-ai-mastery-students/knowledge-base/
```

Create a `knowledge-base` folder in the course repository to keep everything organized.

---

## During Session 1 (Dec 3)

We'll upload these documents to Google AI Studio (Gemini File Search). The process is:

1. Go to Google AI Studio
2. Create a new "File Search" corpus (knowledge base)
3. Upload all your documents
4. Test queries to verify the AI can find information
5. Connect this to your N8N workflow

**Estimated time**: 15-20 minutes to upload and test

---

## Tips for Success

### ✅ DO:
- Start with your most common patient questions
- Use real language patients use ("When can I go back to work?" vs "Return to work timeline")
- Include specific details (exact timeframes, specific instructions)
- Keep documents focused (one topic per document)

### ❌ DON'T:
- Include patient-specific information (keep it general)
- Use medical jargon without explanation
- Create conflicting information across documents
- Include outdated policies or procedures

---

## Questions to Consider

As you prepare documents, think about:

1. **What do patients ask most often?**
2. **What questions does staff repeatedly answer?**
3. **What information is in emails Dr. Patel sends frequently?**
4. **What causes confusion or follow-up questions?**
5. **What emergencies or urgent situations need clear protocols?**

---

## Need Help?

- Use `docs/faq-draft.md` as a template
- Review course materials in `docs/` folder
- Ask Dr. Patel for clarification on medical procedures/policies
- During class, the instructor will help refine these documents

---

**Bottom Line**: Gather 5-10 documents covering FAQs, SOPs, practice info, and policies. Save them in one place. We'll upload them during Session 1 and the AI will use them to answer patient questions accurately.

**Priority**: Focus on the most common patient questions first. You can always add more documents later!
