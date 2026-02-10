# Step 3: Capture Deep Context Using Quality Scopes

**Goal:** Document feature-level quality context that builds on BU understanding

**When:** For every significant feature (P0/P1), epic, refactor, or risky change

**Time Investment:** 30-45 minutes upfront per feature

---

## What is a Quality Scope?

**Quality Scope = Change-level deep context**

While BU Context is domain-level (stable over time), Quality Scope is feature-level (specific to one change).

**A Quality Scope represents anything that needs deep context:**
- New feature development
- Epic spanning multiple sprints
- Refactoring that changes behavior
- High-risk changes (touching money, security, compliance)
- Long-running initiatives

**What goes in a Quality Scope:**
- Why this feature exists (product vision)
- Design intent (UX flows, visual requirements)
- Technical context (architecture, dependencies)
- Risk hypotheses (what could go wrong?)
- Validation approach (how we'll test)
- Learning notes (what we discovered)

---

## Why Quality Scopes Matter

**The Problem: Test Cases Without Context**

```
Test Case #1: User can add item to cart
Test Case #2: User can remove item from cart
Test Case #3: User can checkout
```

**What's missing:**
- WHY does this feature exist?
- WHAT design decisions were made?
- WHAT assumptions are we making?
- WHAT risks did we consider?

**The Solution: Quality Scope + Test Cases**

```
Quality Scope: Shopping Cart Redesign
├── README.md (Why: Increase conversion by reducing cart abandonment)
├── design-intent/ (UX: One-click checkout, saved payment methods)
├── risk-hypotheses/ (Risk: Race condition when items go out of stock mid-checkout)
├── existing-tests-context/ (Coverage: 40% unit tests, no integration tests for concurrent updates)
├── validation-notes/ (Tested: Happy path ✓, Edge case: Out of stock during checkout ✓)
└── learning-notes/ (Learned: Users abandon if checkout >3 clicks)
```

**Value:** Future QAs understand **context**, not just **cases**.

---

## Step-by-Step: Creating a Quality Scope

### Action 1: Decide If You Need a Quality Scope

**Create a Quality Scope if:**
- ✅ Feature is P0 or P1 priority
- ✅ Feature will take >1 sprint
- ✅ Feature touches critical paths (money, auth, data integrity)
- ✅ Feature has regulatory/compliance requirements
- ✅ Feature involves third-party integrations
- ✅ Feature changes existing behavior (refactor, redesign)

**Skip Quality Scope if:**
- ❌ Trivial change (typo fix, copy change)
- ❌ Internal tool/admin feature (low user impact)
- ❌ Quick experiment (A/B test running 1 week)

**When in doubt:** Create it. Better to have context than lose it.

**Time:** 2 minutes

---

### Action 2: Create the Folder Structure

```bash
cd documentation/business-units/[your-bu]/quality-scopes/
mkdir [feature-name]
cd [feature-name]
```

**Example:**
```bash
cd documentation/business-units/revenue-and-growth/quality-scopes/
mkdir dynamic-fee-engine
cd dynamic-fee-engine
```

**Folder name guidelines:**
- Use kebab-case (lowercase with hyphens)
- Be descriptive (not generic)
- ✅ Good: `user-authentication-sso`, `payment-retry-logic`, `dashboard-performance-refactor`
- ❌ Bad: `feature-123`, `new-stuff`, `update`

**Time:** 1 minute

---

### Action 3: Create the README.md (Main Overview)

**File:** `README.md` inside your Quality Scope folder

**Use the template:** [QA Context Brief Template](./14-templates.md#template-1-qa-context-brief)

**Must-have sections:**

```markdown
# [Feature Name] - Quality Scope

## Why This Exists
[1-2 sentences: What problem does this solve?]

## Product Vision
[Link to product brief + key success criteria]

## Design Intent
[Link to design specs + key UX flows]

## Technical Context
[Components involved, dependencies, performance requirements]

## Quality Risks (Top 3-5)
1. [Risk 1]
2. [Risk 2]
3. [Risk 3]

## Validation Approach
[How will we test? Manual? Automated? Both?]

## Quality Gates
- [ ] [Gate 1]
- [ ] [Gate 2]

## Open Questions
- [ ] [Question for Product]
- [ ] [Question for Engineering]
```

**How to fill it:**

1. **Copy product brief summary** → Paste into "Why This Exists"
2. **Link to Figma** → Add to "Design Intent"
3. **Review engineering design doc** → Summarize in "Technical Context"
4. **Brainstorm risks** → List in "Quality Risks" (use BU Quality Flavour to guide)
5. **Plan testing approach** → Document in "Validation Approach"

**Time:** 15-20 minutes

---

### Action 4: Document Risk Hypotheses (Most Important!)

**Folder:** `risk-hypotheses/` inside your Quality Scope

**File:** `risk-hypotheses/risk-analysis.md`

**Use the template:** [Risk Analysis Template](./14-templates.md#template-5-risk-analysis)

**For each risk:**

```markdown
## Risk [Number]: [Risk Title]

### Description
[What could go wrong?]

### Severity: [Critical P0 / High P1 / Medium P2 / Low P3]
- **Financial Impact:** [Revenue, cost impact]
- **Customer Impact:** [UX, trust impact]

### Likelihood: [High / Medium / Low]
- **Why [Likelihood]:** [Reasoning]

### Root Causes (Hypothesized)
1. **[Root Cause 1]**
   - **Scenario:** [When this manifests]
   - **Example:** [Concrete example]
   - **Impact:** [What happens]

### Test Scenarios Required
| Scenario | Input | Expected Output | Priority |
|----------|-------|-----------------|----------|
| [Scenario] | [Input] | [Output] | P0 |

### Mitigation Strategy
**Pre-Development:**
- [ ] [Action]

**Development:**
- [ ] [Action]
```

**Real Example (Revenue BU - Fee Calculation):**

```markdown
## Risk 1: Fee Calculation Errors

### Description
Dynamic fee calculation incorrectly computes fees, causing revenue leakage (undercharging) or overcharging.

### Severity: Critical (P0)
- **Financial Impact:** 0.01% error = Rp 1M monthly at Rp 10B GTV scale
- **Customer Impact:** Incorrect charges destroy trust

### Likelihood: Medium (20%)
- **Why Medium:** Complex logic (tier, currency, promo codes, rounding)

### Root Causes
1. **Rounding Errors**
   - **Scenario:** 2.7% of Rp 123,456 = Rp 3,333.312
   - **Example:** Should round to Rp 3,333.31, not Rp 3,333.32
   - **Impact:** Compounds across thousands of transactions

### Test Scenarios
| Scenario | Input | Expected Output | Priority |
|----------|-------|-----------------|----------|
| Standard Silver tier | Amount: Rp 100K, Tier: Silver | Fee: Rp 4,700 (2.8% + Rp 1,900) | P0 |
| Rounding edge case | Amount: Rp 123,456, Tier: Gold | Fee: Rp 5,133.31 (3,333.31 + 1,800) | P0 |
```

**Why this matters:**
- Documents **your thinking** (not just test steps)
- Helps others understand **why** you're testing something
- Becomes **institutional memory** (prevents repeating mistakes)

**Time:** 20-30 minutes (for 3-5 risks)

---

### Action 5: Document Existing Test Coverage

**Folder:** `existing-tests-context/` inside your Quality Scope

**File:** `existing-tests-context/current-coverage.md`

**What to capture:**

```markdown
# Existing Test Coverage

## Current Tests
- **Unit tests:** [What exists? Where? Coverage %?]
- **Integration tests:** [What exists? What flows covered?]
- **Manual tests:** [What was tested before?]

## Coverage Gaps
- **Gap 1:** [What's not tested?]
- **Gap 2:** [What's not tested?]

## What Needs to Be Added
- [ ] [New test 1]
- [ ] [New test 2]
```

**Why this matters:**
- Prevents duplication (don't re-test what's already covered)
- Identifies gaps (what's missing?)
- Tracks test evolution (how coverage improves over time)

**Time:** 10-15 minutes

---

### Action 6: Fill in Context as You Go

**During development:**

**Folder:** `design-intent/`
- Save screenshots of key UI states
- Document design decisions made during development
- Note UX changes from original spec

**Folder:** `validation-notes/`
- Create test session logs: `test-session-2026-02-10.md`
- Document what you tested, results, bugs found
- Use template: [Validation Notes Template](./14-templates.md#template-7-validation-notes-during-testing)

**After sprint:**

**Folder:** `learning-notes/`
- Create sprint learning notes: `sprint-24.md`
- What went well? What surprised us?
- What would we do differently?
- Use template: [Learning Notes Template](./14-templates.md#template-6-learning-notes-post-sprint)

**Time:** 10-15 minutes per sprint

---

## Quality Scope Lifecycle

### Phase 1: Pre-Development (Sprint Planning)
1. Create Quality Scope folder
2. Write README.md (overview)
3. Document risk hypotheses
4. Note existing test coverage
5. **Output:** QA Context Brief ready for grooming

### Phase 2: During Development (Sprint Execution)
1. Fill in design-intent/ (as design evolves)
2. Document validation-notes/ (as you test)
3. Update risk-hypotheses/ (as new risks discovered)
4. **Output:** Living documentation of quality work

### Phase 3: Post-Development (Sprint Retrospective)
1. Write learning-notes/ (what we learned)
2. Identify patterns (add to cross-BU patterns if applicable)
3. Archive or mark as complete
4. **Output:** Knowledge captured for future reference

---

## When to Reference a Quality Scope

**Before testing:**
- Read README.md (understand context)
- Review risk-hypotheses/ (know what to look for)
- Check existing-tests-context/ (don't duplicate)

**During testing:**
- Update validation-notes/ (log findings)
- Add to design-intent/ (capture design changes)

**After sprint:**
- Write learning-notes/ (share insights)
- Update README if major changes happened

**3 months later:**
- New QA assigned to this feature
- They read the Quality Scope
- They understand context in 15 minutes (vs 3 hours of tribal knowledge transfer)

**This is the value.**

---

## Common Questions

### Q: Do I need a Quality Scope for every feature?

**A:** Only for significant features.

**Yes (create Quality Scope):**
- P0/P1 features
- Features touching money, auth, compliance
- Multi-sprint epics
- Refactors changing behavior

**No (skip Quality Scope):**
- Trivial changes (typo, copy, small UI tweak)
- Internal tools (low impact)
- Short experiments (<1 week)

**Rule of thumb:** If the feature will exist >1 month and has non-trivial risk, create a Quality Scope.

---

### Q: Can I create a Quality Scope after development starts?

**A:** Yes! Better late than never.

**Ideal:** Create before development (use in grooming)
**Acceptable:** Create during development (capture context while fresh)
**Still valuable:** Create after development (preserve knowledge for future)

---

### Q: What if requirements change mid-sprint?

**A:** Update the Quality Scope README.

**Add a note:**
```markdown
## Updates

**2026-02-15:** Design changed from 3-step checkout to 1-step checkout. Updated risk hypotheses to reflect new UX flow. - [Your Name]
```

**Quality Scopes are living documents.** Change is expected.

---

## Success Criteria for This Step

**You've completed Step 3 when:**

- [ ] You've created at least one Quality Scope for a current feature
- [ ] README.md explains why the feature exists
- [ ] Risk hypotheses documented (at least 3 risks)
- [ ] Existing test coverage noted
- [ ] You understand: Quality Scope = feature-level context (not test execution)

**Time to complete:** 30-45 minutes per feature (first time)

---

## Real-World Impact

**Without Quality Scope:**
- QA tests feature → Finds bug → Asks "Is this expected behavior?" → Waits for Product to respond → Loses 2 hours
- New QA joins → Asks "Why are we testing this edge case?" → Senior QA explains verbally → Knowledge not preserved

**With Quality Scope:**
- QA reads README → Understands feature purpose → Tests with context → Finds bug → References design-intent/ → Knows if it's expected or not → Immediate decision
- New QA joins → Reads Quality Scope → Understands context in 15 minutes → Productive immediately → No tribal knowledge lost

**ROI:** 45 minutes documenting saves hours of confusion.

---

## Next Step

Once you've created your first Quality Scope, move to:

**[Step 4: Use Checklist Before Running Anything](./06-step4-checklist.md)**

You'll learn when and how to use the pre-testing checklist to ensure you have enough context.

---

**Remember:** Test cases are outputs. Quality Scopes capture the **thinking** behind the outputs.

Make your thinking visible.
