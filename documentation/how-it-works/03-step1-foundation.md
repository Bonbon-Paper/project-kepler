# Step 1: Everyone Uses the Same Repository (Foundation)

**Goal:** Establish a single source of truth for quality knowledge

**Status:** ✅ You're already here! The repository exists.

**Time Investment:** 10 minutes to understand

---

## Why This Step Exists

**The Problem We're Solving:**

Before Quality Intelligence, quality knowledge was fragmented:
- QA insights lived in individual heads (lost when people leave)
- Test context buried in tickets (hard to find later)
- Lessons learned forgotten by next sprint
- New QAs had no reference (slow onboarding)

**The Solution:**

One repository where ALL quality thinking lives:
- Context: Why features exist, what quality risks matter
- Learning: What worked, what didn't, patterns discovered
- Benchmarks: Shared standards for "good enough"

**The Rule:**

> If it's not in the repository, it doesn't exist.

---

## What "Using the Repository" Means

### ✅ DO Use the Repository For:

1. **Quality Context**
   - Business Unit domain understanding
   - Feature-level quality scopes
   - Design intent and product vision notes

2. **Quality Decisions**
   - Risk hypotheses (what could go wrong?)
   - Test strategies (how will we validate?)
   - Quality gate criteria (when can we ship?)

3. **Quality Learning**
   - Patterns discovered across sprints
   - Bugs that taught us something
   - Process improvements identified

4. **Quality Communication**
   - Monthly summaries for leadership
   - Cross-BU patterns to share
   - Benchmarks for "good quality"

### ❌ DON'T Use the Repository For:

1. **Test Execution**
   - Test cases live in test management tools (Jira, TestRail)
   - Test automation lives in code repositories
   - Test runs/results live in CI/CD pipelines

2. **Bug Tracking**
   - Individual bugs live in issue trackers (Jira)
   - Repository captures **patterns** of bugs, not individual instances

3. **Daily Communication**
   - Chat (Slack/Teams) for quick questions
   - Meetings for real-time collaboration
   - Repository for durable knowledge

---

## How to Use This Repository

### The Mental Model

Think of this repository as **QA's shared brain**:

```
Individual QA Brain          Shared QA Brain (Repository)
├── Context (in your head)   ├── documentation/business-units/
├── Risks (your hunches)     ├── documentation/quality-scopes/
├── Learning (you remember)  ├── quality-tracking/
└── Standards (you know)     └── benchmarks/
```

**Your individual knowledge** + **The repository** = **Quality Intelligence**

---

## Step-by-Step: Your First Day with the Repository

### Action 1: Clone or Access the Repository

**If using Git:**
```bash
git clone [repository-url]
cd project-kepler
```

**If using shared drive:**
- Navigate to the shared folder
- Bookmark it for quick access

**Time:** 2 minutes

---

### Action 2: Read the README

**File:** [README.md](../../README.md)

**What you'll learn:**
- What Quality Intelligence is
- Why we're doing this
- Quick start guide

**Time:** 5 minutes

---

### Action 3: Explore the Structure

**Navigate through:**

1. **Business Units:** `documentation/business-units/`
   - Pick one BU you work with (e.g., Revenue & Growth)
   - Read `_bu-context.md` (understand the domain)
   - Read `_quality-flavour.md` (what "good" means here)

2. **Quality Scopes:** `documentation/business-units/[bu]/quality-scopes/`
   - Open one example (e.g., Dynamic Fee Engine)
   - See how features are documented

3. **How It Works:** `documentation/how-it-works/`
   - You're reading this now!

**Time:** 10 minutes

---

### Action 4: Bookmark Key Files

**Must-bookmark:**
- [README.md](../../README.md) - Start here always
- [Checklist Before Run](../../checklists/checklist-before-run.md) - Use before testing
- [Templates](./14-templates.md) - Copy-paste starting points
- Your BU folder - `documentation/business-units/[your-bu]/`

**Time:** 1 minute

---

## Rules for Repository Usage

### Rule 1: Consistency Over Perfection

**Don't:**
- ❌ Spend 4 hours writing perfect documentation
- ❌ Wait until you have "all the information"
- ❌ Aim for completeness before contributing

**Do:**
- ✅ Write 3 bullet points now, add more later
- ✅ Document what you know today
- ✅ Update incrementally as you learn

**Mantra:** "Consistent small contributions > Perfect one-time documentation"

---

### Rule 2: Write for Future You

**When documenting, ask:**
- "If I read this 3 months from now, would I understand my thinking?"
- "If a new QA joined and found this, could they make sense of it?"
- "If I get context-switched to another project, could I pick this up again?"

**Write as if you'll forget everything tomorrow** (because you probably will).

---

### Rule 3: Make Thinking Visible

**The goal isn't documentation. The goal is shared thinking.**

**Example - Bad:**
```
Tested payment flow. Found bug. Fixed.
```

**Example - Good:**
```
Tested payment flow with focus on multi-currency edge cases.
Found: USD→IDR conversion uses stale exchange rate (yesterday's rate instead of real-time).
Impact: Fee calculation 0.5% off → ~$50 revenue variance per $10K transaction.
Fix: Updated FX rate API call to use transaction timestamp.
Learning: Always validate timestamp-sensitive calculations (applies to proration logic too).
```

The second example **teaches**. The first just records.

---

### Rule 4: Update, Don't Duplicate

**Before creating a new document, check if one exists:**

```bash
# Search for existing context
grep -r "payment flow" documentation/
```

**If it exists:** Update it (add new insights, correct outdated info)
**If it doesn't:** Create it

**Why:** Scattered knowledge is useless knowledge.

---

### Rule 5: Tag and Reference

**Use cross-references to connect knowledge:**

```markdown
## Risk: Fee Calculation Errors

See also:
- [Revenue Quality Flavour](../../../business-units/revenue-and-growth/_quality-flavour.md) - Why fee accuracy matters
- [Existing Test Coverage](./existing-tests-context/current-coverage.md) - What tests we have
- [Cross-BU Pattern: Rounding Errors](../../../../quality-tracking/cross-bu-patterns/rounding-errors.md) - Similar issues in other BUs
```

**Why:** Knowledge is a graph, not a list.

---

## Common Questions

### Q: How often should I update the repository?

**A:** Whenever you have a learning worth sharing.

**Minimum:**
- After each sprint: Add learning notes to Quality Scopes
- Before grooming: Create QA Context Brief for new features
- When you discover a pattern: Add to cross-BU patterns

**Ideal:**
- Daily: Quick notes as you test
- Weekly: Update Quality Scope with validation progress
- Monthly: Contribute to monthly quality summary

---

### Q: What if I make a mistake in documentation?

**A:** It's fine. This is version-controlled (or easy to edit).

**Process:**
1. Find the mistake
2. Update the document
3. (Optional) Add a note: "Updated [date]: Fixed incorrect assumption about X"

**Remember:** Wrong documentation that gets corrected is better than no documentation.

---

### Q: What if I don't know where to put something?

**A:** Make your best guess, then add a note.

**Example:**
```markdown
# Payment Flow Validation

> **Note:** Not sure if this belongs in Revenue BU or Network BU.
> Tagging both for now. - [Your Name], 2026-02-10
```

Someone will see it and suggest the right place, or it'll stay there and be useful anyway.

---

### Q: Do I need to read everything in the repository?

**A:** No. Start with your BU, expand as needed.

**Essential reading:**
- [README.md](../../README.md)
- Your BU context (`documentation/business-units/[your-bu]/`)
- Quality Scopes for features you're testing

**Optional reading:**
- Other BUs (helpful for cross-BU patterns)
- How It Works guides (when you need detailed process)
- Monthly summaries (for high-level trends)

---

### Q: What if the repository becomes too large?

**A:** We'll archive old content when needed.

**Strategy:**
- Keep current sprint/quarter content active
- Archive older Quality Scopes (move to `/archive/`)
- Retain cross-BU patterns and benchmarks (these are timeless)

**Trust the process:** We'll handle scale when we get there.

---

## Success Criteria for This Step

**You've completed Step 1 when:**

- [ ] You can navigate to your BU folder without help
- [ ] You've read at least one Quality Scope example
- [ ] You've bookmarked key files for quick access
- [ ] You understand: "If it's not here, it doesn't exist"
- [ ] You're ready to contribute (even if just 3 bullet points)

**Time to complete:** 15-20 minutes

---

## What Happens If We Skip This Step?

**Without a shared repository:**
- QA knowledge fragments across individuals (knowledge loss when people leave)
- Same mistakes repeated (no learning loop)
- Inconsistent quality decisions (no shared benchmarks)
- Slow onboarding (new QAs start from zero)
- No compounding value (work is disposable, not durable)

**With a shared repository:**
- QA knowledge compounds (each sprint adds to collective intelligence)
- Patterns emerge (learnings from Sprint 1 inform Sprint 10)
- Quality decisions become data-driven (benchmarks guide judgment)
- Fast onboarding (new QAs reference existing context)
- Work has lasting value (documentation becomes institutional memory)

---

## Next Step

Once you're comfortable navigating the repository, move to:

**[Step 2: Start with Business Unit Context](./04-step2-bu-context.md)**

You'll learn how to understand domain-level quality context before diving into features.

---

**Remember:** The repository is a tool, not a burden.

Use it to make your thinking visible, your knowledge shareable, and your work have lasting impact.

**If it feels like bureaucracy, we're doing it wrong. Speak up and we'll simplify.**
