# Project Kepler 🚀

**Quality Intelligence Operating Model**

---

## What is this?

Project Kepler transforms QA from reactive testing to proactive quality intelligence. We've built a system where QA understands product vision, design intent, and engineering signals—then makes quality decisions *before* code is written, not after.

**Not a tool. Not a role. A way of working.**

---

## Why "Kepler"?

Johannes Kepler discovered the laws of planetary motion by observing patterns in seemingly chaotic data. Similarly, Quality Intelligence observes patterns in quality signals (hotfixes, rework, late issues), learns from them, and predicts risks before they impact delivery.

**We turn quality data into quality intelligence.**

---

## The Transformation

```
Level 0: Reactive Quality (Where most teams start)
  ↓
Level 1: Preventive Quality          ← Month 1: Context briefs, early QA involvement
  ↓
Level 2: Predictive Quality          ← Month 2: Quality signals, pattern recognition
  ↓
Level 3: Assisted Quality            ← Month 3: AI-enhanced analysis (human-reviewed)
  ↓
Level 4: Autonomous Quality          ← Future state (not current focus)
```

**Current Phase:** Month 1 - Preventive Quality Foundation

---

## Core Principles

1. **Consistency > Completeness** - Document 3 insights every sprint beats perfect documentation once and burnout
2. **Context Before Execution** - Never test without understanding WHY (use [Checklist Before Run](./checklists/checklist-before-run.md))
3. **Human Judgment First** - AI assists, QA decides. Always.
4. **Make Thinking Visible** - Repository captures WHY (thinking), not just WHAT (test cases)
5. **Cross-Functional Hub** - QA connects Product vision + Design intent + Engineering needs

---

## Repository Structure

```
📁 documentation/
  ├── business-units/              # Domain-level quality context
  │   ├── revenue-and-growth/      # Example: Payment processing BU
  │   │   ├── _bu-context.md       # What this BU does, tech stack
  │   │   ├── _quality-flavour.md  # What "good quality" means here
  │   │   ├── _okr-alignment.md    # Business goals + QA contributions
  │   │   └── quality-scopes/      # Feature-level deep context (nested!)
  │   │       └── dynamic-fee-engine/
  │   │           ├── README.md
  │   │           ├── risk-hypotheses/
  │   │           ├── existing-tests-context/
  │   │           ├── design-intent/
  │   │           ├── validation-notes/
  │   │           └── learning-notes/
  │   ├── network/                 # (Ready to scaffold)
  │   └── pivot/                   # (Ready to scaffold)
  └── how-it-works/                # Step-by-step implementation guides
      ├── 00-overview.md           # Big picture
      ├── 03-step1-foundation.md   # Step 1: Everyone uses same repo
      ├── 04-step2-bu-context.md   # Step 2: Start with BU context
      ├── 05-step3-quality-scopes.md  # Step 3: Capture deep context
      ├── 06-step4-checklist.md    # Step 4: Use checklist before run
      ├── 07-step5-ai-assistance.md   # Step 5: AI assists (doesn't decide)
      ├── 08-step6-learning.md     # Step 6: Learning becomes memory
      ├── 09-step7-storytelling.md # Step 7: Board for storytelling
      └── 14-templates.md          # All templates

📁 quality-tracking/
  ├── cross-bu-patterns/     # Patterns spanning multiple BUs
  └── monthly-summaries/     # High-level insights for leadership

📁 benchmarks/
  └── quality-standards/     # Shared quality benchmarks

📁 checklists/
  └── checklist-before-run.md  # 10-point mandatory checklist

📁 automation-sources/       # References to automation projects

📁 boards/                   # Quality Learning Board visualization

📄 CLAUDE.md                 # Operating manual for GM Quality (your handbook)
📄 STATUS.md                 # Implementation tracking + design insights
📄 README.md                 # This file (quick overview)
```

**Key Insight:** Quality Scopes are nested UNDER each Business Unit folder. This maintains hierarchical context (domain → feature).

---

## Quick Start

### For QA Team Members

**Your First Day with Quality Intelligence:**

1. **Understand the foundation** (20 min)
   - Read this README
   - Skim [CLAUDE.md](./CLAUDE.md) (your operating manual)
   - Review [How It Works Overview](./documentation/how-it-works/00-overview.md)

2. **Learn from the example** (30 min)
   - Read [Revenue BU Context](./documentation/business-units/revenue-and-growth/_bu-context.md)
   - Read [Revenue Quality Flavour](./documentation/business-units/revenue-and-growth/_quality-flavour.md)
   - Explore [Dynamic Fee Engine Quality Scope](./documentation/business-units/revenue-and-growth/quality-scopes/dynamic-fee-engine/README.md)

3. **Follow the 7-step workflow** (when working on features)
   - **Step 1:** Use this repository for all quality knowledge
   - **Step 2:** Read your BU context BEFORE testing anything
   - **Step 3:** Create Quality Scope for P0/P1 features
   - **Step 4:** Use [Checklist Before Run](./checklists/checklist-before-run.md) every time
   - **Step 5:** Use AI to assist (never to decide)
   - **Step 6:** Document learning after sprint
   - **Step 7:** Contribute to Quality Learning Board monthly

**Time Investment:** 15-30 minutes per sprint (saves hours of rework later)

### For Stakeholders (Product, Design, Engineering)

**How QA Engages Now:**

| When | What Changes | Why It Matters |
|------|--------------|----------------|
| **Before Grooming** | QA reviews product vision + design intent early | Identifies quality risks before development starts |
| **During Grooming** | QA discusses risks with business context (not just test scope) | Prevents late-stage surprises, reduces rework |
| **During Sprint** | QA provides earlier feedback using Quality Scopes | Faster feedback loop, less context-switching |
| **After Sprint** | QA documents learnings in repository | Knowledge preserved, new QAs onboard faster |

**What You'll Notice:**
- ✅ Fewer "We didn't think about that" moments mid-sprint
- ✅ Quality discussions reference business impact (revenue, compliance, UX)
- ✅ Clearer quality criteria BEFORE development starts
- ✅ New QAs productive faster (they read existing context)

---

## Success Metrics by Phase

**Month 1 - Preventive Quality:**
- 🎯 QA joins ≥80% of grooming sessions
- 🎯 Every P0/P1 feature has QA Context Brief
- 🎯 3+ Quality Scopes documented
- 🎯 Quality gates defined per BU

**Month 2 - Predictive Quality:**
- 🎯 Quality signals tracked weekly (hotfixes, rework, late issues)
- 🎯 Quality Learning Board updated monthly
- 🎯 Team uses AI for context analysis (human-reviewed)
- 🎯 15-30 min QA sharing per sprint

**Month 3 - Assisted Quality:**
- 🎯 Consistent AI usage (risk analysis, test ideation)
- 🎯 Prompting guidelines per BU
- 🎯 Productivity gains visible (time to risk assessment ↓)

**Month 4 - Standardization:**
- 🎯 QA Playbook published
- 🎯 Quality benchmarks used consistently
- 🎯 Monthly Quality Summary routine
- 🎯 Late-stage issues trending down

---

## Key Documents

**Start Here:**
- **[README.md](./README.md)** - This file (5-minute overview)
- **[CLAUDE.md](./CLAUDE.md)** - Operating manual for GM Quality (how to run the system)
- **[STATUS.md](./STATUS.md)** - Implementation tracking + design insights (what we've built, what's next)

**Reference Documents:**
- **[PRD - Quality Intelligence.pdf](./PRD%20-%20Quality%20Intelligence.pdf)** - Complete product requirements (the vision)
- **[How It Works](./documentation/how-it-works/)** - 7-step workflow guides (03-09 step-by-step)
- **[Templates](./documentation/how-it-works/14-templates.md)** - All documentation templates (copy-paste starting points)
- **[Checklist Before Run](./checklists/checklist-before-run.md)** - 10-point mandatory checklist (use before testing)

**Document Roles Clarified:**
- **CLAUDE.md** = Your handbook (what GM Quality does, how to operate)
- **STATUS.md** = Progress tracker (what's done, what's next, design insights)
- **How It Works** = Execution guides (step-by-step workflow)
- **README.md** = Quick overview (you're reading it now)

---

## Why Revenue BU First?

**We started with Revenue & Growth BU as the complete example because:**

1. **High Stakes = Clear Value**
   - Quality directly impacts money (0.01% fee error = Rp 1M monthly loss at Rp 10B GTV scale)
   - Financial correctness is binary (100% or wrong)
   - Every quality decision traceable to revenue

2. **Real Complexity**
   - Multi-currency handling (USD, SGD, IDR)
   - Tiered pricing (Bronze → Diamond merchants)
   - PCI-DSS compliance requirements
   - Third-party payment gateway integrations

3. **Perfect Showcase**
   - Complete example: BU Context + Quality Flavour + OKRs + Quality Scope
   - Realistic feature: Dynamic Fee Engine with 5 detailed risks
   - Quantified impact: Makes quality tangible, not abstract

**Use Revenue BU as your learning template.** Then adapt for your BU (Network, Pivot, etc.)

---

## FAQs

**Q: Is this replacing our existing test automation?**
A: No. Quality Intelligence is about *context and decision-making*. Test automation continues as normal. This system captures WHY you're testing (thinking), not the test execution itself.

**Q: Do I need to document everything perfectly?**
A: No. **Consistency > Completeness.** Write 3 bullet points every sprint, not perfect documentation once. Better to document what you know today and improve it later.

**Q: What if I don't know where to start?**
A: Follow this path:
1. Read [Step 1: Foundation](./documentation/how-it-works/03-step1-foundation.md) (10 min)
2. Read [Step 2: BU Context](./documentation/how-it-works/04-step2-bu-context.md) (15 min)
3. Create your first Quality Scope using [Template 1](./documentation/how-it-works/14-templates.md#template-1-qa-context-brief) (30 min)

**Q: Are we using AI to replace QA?**
A: Never. **AI assists, QA decides.** AI helps with:
- Context analysis (summarize long product briefs)
- Risk brainstorming (generate edge case ideas)
- Test ideation (suggest scenarios)
- Documentation speed (draft learning notes)

But **human judgment is non-negotiable**: QA reviews all AI outputs, makes final decisions, and applies business context AI doesn't have.

**Q: How much extra time does this take?**
A: **Target: 15-30 minutes per sprint** for knowledge capture. The time saved from fewer late-stage issues, faster onboarding, and reusable context far exceeds this investment.

**Q: What if my BU doesn't have context documented yet?**
A: You write it! Use [BU Context Template](./documentation/how-it-works/14-templates.md#template-2-business-unit-context). Start simple:
- 3 bullets for "What This BU Does"
- 3 quality dimensions
- 5 known risk areas
Expand it over time. Revenue BU example shows what "complete" looks like.

---

## Current Status

**Phase:** Month 1 - Preventive Quality Foundation
**Focus:** Early QA involvement, context briefs, basic quality gates
**Completion:** ✅ Foundation complete (20+ docs), Revenue BU example complete
**Next Milestone:** Team onboarding + 80% grooming attendance + context briefs for all P0/P1 features

**See [STATUS.md](./STATUS.md) for detailed implementation tracking.**

---

## Contributing

**Everyone contributes to Quality Intelligence:**

| Role | Contribution | When |
|------|--------------|------|
| **QA** | Document context, risks, learning notes | Every sprint |
| **Product** | Share vision + goals with QA | Before grooming |
| **Design** | Share design intent + UX flows | Before handoff |
| **Engineering** | Collaborate on quality needs | During architecture discussions |

**The Rule:** If it helps future sprints make better quality decisions, it belongs in this repository.

**The Standard:** Follow [How to Update Documents](#document-update-tracking-standard) below when making changes.

---

## Contact

**QA Lead:** [To be assigned]
**Initiative Owner:** QA Team
**Executive Sponsor:** GM Quality

---

## Document Update Tracking Standard

**Every time you update a document in this repository, add an update log entry:**

### Update Log Format

```markdown
## Document Updates

**[YYYY-MM-DD] - [Document Name]**
- **Updated by:** [Your Name]
- **What changed:** [Brief description of changes]
- **Why:** [Reason for update - what was missing or incorrect?]
- **Impact:** [Who should re-read this? What workflow changes?]

---
```

### Example

```markdown
## Document Updates

**2026-02-10 - README.md**
- **Updated by:** Quality Team
- **What changed:**
  - Clarified CLAUDE.md is operating manual (not conversation history)
  - Updated repository structure to show quality-scopes nested under BUs
  - Added "Why Revenue BU First?" section
  - Expanded FAQs with practical guidance
  - Added document update tracking standard
- **Why:** Initial README was outdated after we refined the structure through collaborative design. Missing clarity on document roles (CLAUDE.md vs STATUS.md vs How It Works).
- **Impact:** All team members should re-read README to understand updated structure and document roles.

---
```

### Where to Add Update Logs

| Document Type | Where to Add Update Log |
|---------------|-------------------------|
| Core docs (README, CLAUDE.md, STATUS.md) | At the end of the file under `## Document Updates` |
| BU Context files | At the end under `## Update History` |
| Quality Scopes | In README.md of the Quality Scope under `## Updates` |
| How It Works guides | At the end under `## Revision History` |

**This ensures we can track:**
- What knowledge we gained over time
- What assumptions we corrected
- Why the process evolved
- Who to ask for context on changes

---

*"Quality Intelligence enables QA to move from finding defects to guiding quality decisions."*

**Let's pioneer quality that drives business results.** 💰🚀

---

## Document Updates

**2026-02-10 - README.md**
- **Updated by:** Quality Team
- **What changed:**
  - Clarified CLAUDE.md is operating manual (not conversation history)
  - Updated repository structure to show quality-scopes nested under BUs
  - Added "Why Revenue BU First?" section explaining example choice
  - Expanded FAQs with practical guidance (where to start, time investment, AI role)
  - Added Quick Start with time estimates and 7-step workflow reference
  - Restructured Key Documents section with clear document roles
  - Added Success Metrics by Phase (Month 1-4)
  - Added Document Update Tracking Standard (this section)
- **Why:** Initial README was outdated after we refined structure through collaborative design. Missing clarity on:
  - Document roles (CLAUDE.md vs STATUS.md vs How It Works)
  - Why quality-scopes are nested (hierarchical context)
  - Practical getting-started path for new users
  - How to track document changes systematically
- **Impact:** All team members should re-read README (5-10 min) to understand:
  - Updated structure and document purposes
  - Clear path to get started (7-step workflow)
  - How to update documents going forward (tracking standard)
