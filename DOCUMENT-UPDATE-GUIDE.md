# Document Update Guide - Quality Intelligence

**Purpose:** Standardize how we track document changes to maintain knowledge continuity

**Last Updated:** 2026-02-10

---

## Why We Track Updates

**The Problem:**
- Documents evolve but we lose track of WHY changes were made
- Future readers don't know what assumptions were corrected
- Knowledge gaps aren't visible (what did we miss before?)
- Hard to understand the evolution of our quality thinking

**The Solution:**
Every document update includes a log entry that captures:
1. **What** changed
2. **Why** it was changed (what was missing or incorrect?)
3. **Who** made the change
4. **Impact** (who should re-read this?)

**This creates an audit trail of our collective learning.**

---

## Update Log Format

### Standard Template

Copy-paste this template at the end of any document you update:

```markdown
## Document Updates

**[YYYY-MM-DD] - [Document Name or Section]**
- **Updated by:** [Your Name]
- **What changed:**
  - [Specific change 1]
  - [Specific change 2]
  - [Specific change 3]
- **Why:** [Reason for update - what was missing, incorrect, or unclear?]
- **Impact:** [Who should re-read? What workflow changes?]

---
```

---

## Where to Add Update Logs

| Document Type | Section Title | Location |
|---------------|---------------|----------|
| **Core Documents** | `## Document Updates` | End of file |
| (README.md, CLAUDE.md, STATUS.md) | | |
| **BU Context Files** | `## Update History` | End of file |
| (_bu-context.md, _quality-flavour.md, _okr-alignment.md) | | |
| **Quality Scopes** | `## Updates` | In README.md of Quality Scope |
| (Quality Scope README.md) | | |
| **How It Works Guides** | `## Revision History` | End of file |
| (Step guides, templates) | | |
| **Templates** | `## Template Change Log` | End of template file |
| **Checklists** | `## Checklist Updates` | End of checklist |

---

## Examples by Document Type

### Example 1: Core Document (README.md)

```markdown
## Document Updates

**2026-02-10 - README.md**
- **Updated by:** Quality Team
- **What changed:**
  - Added "Why Revenue BU First?" section
  - Clarified document roles (CLAUDE.md vs STATUS.md)
  - Updated repository structure to show nested quality-scopes
  - Expanded FAQs with practical guidance
- **Why:** Initial README outdated after structure refinement. Missing clarity on document purposes and getting-started path.
- **Impact:** All team members should re-read README (5-10 min) to understand updated structure.

---
```

### Example 2: BU Context File

```markdown
## Update History

**2026-03-15 - Revenue BU Context**
- **Updated by:** Sarah Chen (QA Lead - Revenue)
- **What changed:**
  - Added new payment gateway: Adyen (alongside Stripe, Midtrans)
  - Updated technical stack: Migrated from PostgreSQL to MySQL
  - Added 3 new domain terms: "Authorization hold", "Pre-auth", "Capture"
- **Why:** Tech stack migration completed in Sprint 24. Adyen integration launched for EU merchants. New terminology needed for cross-border transactions.
- **Impact:** All Revenue BU QAs must re-read Technical Context and Domain Terminology sections. Update existing Quality Scopes if they reference old tech stack.

---

**2026-02-10 - Initial creation**
- **Updated by:** Quality Team
- **What changed:** Created initial BU Context with core responsibility, products, user journeys, tech stack, terminology
- **Why:** Establishing domain-level quality context for Revenue BU as first example
- **Impact:** All Revenue QAs should read to understand domain before testing features

---
```

### Example 3: Quality Scope (Feature-Level)

```markdown
## Updates

**2026-02-28 - Design Changed to 1-Step Checkout**
- **Updated by:** Alice Wong (QA - Revenue)
- **What changed:**
  - Updated design-intent/: Changed from 3-step to 1-step checkout flow
  - Updated risk-hypotheses/risk-analysis.md: Removed "User abandons between steps" risk (no longer applicable)
  - Added new risk: "Form validation must be instant (no submit-wait-error loop)"
- **Why:** Product pivoted to 1-step checkout after A/B test showed 40% higher conversion. Original 3-step flow assumptions no longer valid.
- **Impact:** Re-test validation scenarios. Update test cases to focus on inline validation, not multi-step flow.

---

**2026-02-10 - Initial Quality Scope**
- **Updated by:** Quality Team
- **What changed:** Created Dynamic Fee Engine Quality Scope with README, risk hypotheses, existing test coverage
- **Why:** P0 feature starting Sprint 25. Need quality context before grooming.
- **Impact:** All Revenue QAs review before grooming on 2026-02-12.

---
```

### Example 4: How It Works Guide

```markdown
## Revision History

**2026-03-01 - Added AI Prompting Examples**
- **Updated by:** Quality Team
- **What changed:**
  - Added section "Example Prompts for Revenue BU" with 5 real examples
  - Included good vs bad prompt comparison
  - Added "What AI Can't Know" section (business priorities, stakeholder preferences)
- **Why:** Team requested practical examples after Month 2 retrospective. Generic prompting guidance not actionable enough.
- **Impact:** QAs using AI should review new examples section (10 min). Update your prompts to include more business context.

---

**2026-02-10 - Initial creation**
- **Updated by:** Quality Team
- **What changed:** Created Step 5: AI Assistance guide with principles, usage guidelines, human review requirements
- **Why:** Part of 7-step Quality Intelligence workflow. Establishing AI as assistant (not decision-maker).
- **Impact:** All QAs read before using AI for quality work (Month 2 onwards).

---
```

---

## When to Update Documents

### Mandatory Updates

**You MUST add an update log when:**

1. **BU Context changes** (quarterly or when major changes happen)
   - New products launched
   - Tech stack changes (database migration, new integrations)
   - OKRs change (every quarter)
   - New quality dimensions discovered

2. **Quality Scope changes** (during sprint)
   - Requirements change mid-sprint
   - Design pivots
   - New risks discovered during testing
   - Assumptions proven wrong

3. **Process changes** (when workflow evolves)
   - Step-by-step guides updated
   - Templates improved
   - Checklists expanded

4. **Learnings captured** (post-sprint)
   - Pattern discovered that applies to multiple features
   - Assumption corrected (we thought X, but learned Y)
   - New best practice established

### Optional Updates (But Encouraged)

**Consider adding update log when:**
- Fixing typos (if they changed meaning)
- Clarifying ambiguous sections
- Adding examples to existing sections
- Reorganizing structure for clarity

**Rule of Thumb:** If someone reading this document 3 months from now would benefit from knowing what changed and why, log it.

---

## What Makes a Good Update Log?

### ✅ Good Update Logs

**Example 1: Specific and Actionable**
```markdown
**2026-03-10 - Revenue Quality Flavour**
- **Updated by:** Mark Liu (QA Manager)
- **What changed:**
  - Updated "Financial Correctness" from 99.9% to 100% (zero tolerance)
  - Added new quality dimension: "Regulatory Compliance" (PCI-DSS Level 1)
  - Removed "Speed" as quality dimension (deprioritized this quarter)
- **Why:** Finance team escalated: 0.1% error = Rp 10M loss annually. PCI-DSS audit failed last month. Speed not a current business priority.
- **Impact:** All Revenue QAs: Financial correctness now 100% (no exceptions). New compliance checklist mandatory before release.
```

**Why it's good:**
- Specific changes (not vague)
- Clear reasoning (business impact)
- Actionable impact (what QAs must do)

### ❌ Bad Update Logs

**Example: Vague and Unhelpful**
```markdown
**2026-03-10 - Updated some stuff**
- **Updated by:** Someone
- **What changed:** Made changes to quality dimensions
- **Why:** Needed to update
- **Impact:** Please read
```

**Why it's bad:**
- "Some stuff" - not specific
- "Needed to update" - no reasoning
- "Please read" - not actionable
- No date, unclear who made change

---

## How to Review Update Logs

### Weekly Review (QA Team)

**Every Monday, QA Lead reviews:**
1. What documents were updated last week?
2. Do update logs explain changes clearly?
3. Are impacted team members aware?

**Action:** If update log is vague, ask author to clarify.

### Monthly Review (GM Quality)

**End of each month, GM Quality reviews:**
1. What patterns in updates? (Are we correcting same assumptions repeatedly?)
2. Are documents staying current? (Or growing stale?)
3. Are teams using update logs to track changes?

**Action:** Add patterns to [cross-BU patterns](./quality-tracking/cross-bu-patterns/) if applicable.

### Quarterly Review (All Stakeholders)

**Every quarter:**
1. BU Contexts reviewed (tech stack, OKRs, quality dimensions current?)
2. Quality Scopes archived (>6 months old → move to /archive/)
3. How It Works guides updated (process improvements identified?)

**Action:** Update logs help identify what changed and why. Use them to inform quarterly planning.

---

## Tools for Tracking Updates

### If Using Git

**Commit messages should mirror update logs:**

```bash
git commit -m "Update Revenue BU Context: Add Adyen integration, update tech stack

- Added Adyen payment gateway (EU merchant support)
- Updated tech stack: PostgreSQL → MySQL migration
- Added new domain terms for cross-border transactions

Why: Tech stack migration completed Sprint 24. Adyen integration launched.
Impact: All Revenue QAs re-read Technical Context section."
```

### If Using Shared Drive (No Git)

**Add update log directly in document:**
- Always at the end under standardized section title
- Keep chronological order (newest at top)
- Use separator line (`---`) between entries

---

## Common Questions

### Q: Do I need to log every tiny change?

**A:** Use judgment. Log changes that:
- Affect how someone uses the document
- Correct previous assumptions
- Add significant new information
- Change workflow or process

**Don't log:**
- Typo fixes (unless they changed meaning)
- Formatting adjustments
- Minor wording improvements

**Rule of Thumb:** If someone 3 months from now would ask "Why did this change?", log it.

---

### Q: What if I don't know the impact?

**A:** Make your best guess. Examples:
- "Impact: Unknown - please review if you work on [feature area]"
- "Impact: Potentially affects [BU] - QA Lead to confirm"
- "Impact: No workflow change, informational only"

**It's okay to not know.** Logging the change is more important than perfect impact analysis.

---

### Q: Can I update someone else's update log?

**A:** Yes, if:
- You're clarifying unclear entries
- You're adding missing context
- You're correcting factual errors

**Always note who made the clarification:**

```markdown
**2026-03-15 - Revenue Quality Flavour**
- **Updated by:** Sarah Chen
- **What changed:** [original entry]
- **Why:** [original reasoning]
- **Impact:** [original impact]
- **[Clarification added 2026-03-20 by Mark Liu]:** The "100% financial correctness" applies to fee calculation only, not all revenue features. Proration logic still allows 99.9% tolerance for rounding.
```

---

### Q: What if I disagree with a documented change?

**A:** Add a note in the update log:

```markdown
**2026-03-20 - Note on Financial Correctness Standard**
- **Updated by:** David Park (QA - Revenue)
- **Note:** Questioning whether 100% financial correctness is achievable for all scenarios. Some third-party APIs (currency conversion) return values with inherent rounding. Proposed: Discuss in next team meeting.
- **Status:** Open discussion
```

Then discuss in team meeting. Update document based on consensus.

---

## Update Log Hygiene

### Monthly Cleanup

**If update logs get too long (>10 entries), consider:**

1. **Archive old entries** (move to separate file: `_update-history-2026.md`)
2. **Keep recent entries** (last 3-6 months in main document)
3. **Summarize patterns** (if same thing updated repeatedly, document the pattern)

**Example:**
```markdown
## Document Updates

**[Recent entries - last 6 months]**

---

**Older Updates:** See [_update-history-2026.md](./_update-history-2026.md) for updates before 2026-06-01.
```

### Quarterly Review

**Every quarter, GM Quality:**
1. Reviews all update logs across repository
2. Identifies patterns (what keeps changing?)
3. Updates process if same corrections repeat
4. Celebrates learning (what did we correct? What did we learn?)

---

## Success Criteria

**You're using update logs well when:**

✅ Any team member can read a document and understand its evolution
✅ New QAs can see what assumptions were corrected over time
✅ You can answer "Why did we change this?" months later
✅ Patterns emerge from multiple update logs (shared learnings)
✅ Updates feel lightweight (2-3 min to log), not burdensome

**Red Flags:**
❌ Update logs are vague ("Updated stuff")
❌ Documents change without logs (lost context)
❌ No one reads update logs (not useful)
❌ Logging feels like bureaucracy (too complex)

---

## Quick Reference

### Update Log Checklist

Before closing your document edit:

- [ ] Added `## [Section Title]` if first update to this document
- [ ] Included date (YYYY-MM-DD) and your name
- [ ] Listed specific changes (not vague)
- [ ] Explained WHY (what was missing or wrong?)
- [ ] Identified IMPACT (who should re-read?)
- [ ] Used `---` separator between entries
- [ ] Placed newest entry at TOP (reverse chronological)

**Time Investment:** 2-3 minutes per update

**Value:** Preserves context forever, prevents knowledge loss

---

## Templates by Document Type

### Core Document Template
```markdown
## Document Updates

**YYYY-MM-DD - [Document Name]**
- **Updated by:** [Your Name]
- **What changed:**
  - [Change 1]
  - [Change 2]
- **Why:** [Reason - what was missing or incorrect?]
- **Impact:** [Who should re-read? What changes?]

---
```

### BU Context Template
```markdown
## Update History

**YYYY-MM-DD - [BU Name] Context**
- **Updated by:** [Your Name]
- **What changed:**
  - [Change 1]
  - [Change 2]
- **Why:** [Reason - tech stack change? New product? OKR shift?]
- **Impact:** [Who in this BU should re-read? What sections?]

---
```

### Quality Scope Template
```markdown
## Updates

**YYYY-MM-DD - [What Changed]**
- **Updated by:** [Your Name]
- **What changed:**
  - [Change 1]
  - [Change 2]
- **Why:** [Reason - requirements changed? Design pivoted? Risk discovered?]
- **Impact:** [Re-test needed? Update test cases? Inform stakeholders?]

---
```

---

## Final Reminder

**Update logs are not bureaucracy. They're institutional memory.**

When you leave the team, your update logs teach the next person what you learned. When requirements change, your logs explain what assumptions were made. When patterns emerge, your logs reveal them.

**Make your thinking visible. Log your changes. Preserve knowledge.**

---

**Document Ownership:** QA Team (all members)
**Review Frequency:** Weekly (QA Lead), Monthly (GM Quality), Quarterly (All Stakeholders)
**Questions:** Ask in team channel or update this guide with clarifications

---

## Document Updates

**2026-02-10 - DOCUMENT-UPDATE-GUIDE.md**
- **Updated by:** Quality Team
- **What changed:** Created comprehensive guide for tracking document updates across Quality Intelligence repository
- **Why:** Standardize how we track changes to maintain knowledge continuity. Without update tracking, we lose context on why documents evolved, what assumptions were corrected, and what we learned.
- **Impact:** All team members must follow this guide when updating ANY document in the repository. Add this guide to onboarding materials.

---
