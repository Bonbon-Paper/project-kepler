# Step 6: Learning Becomes Shared Memory

**Goal:** Capture quality insights so knowledge compounds across sprints

**When:** After every sprint (5-10 minutes per sprint)

**Time Investment:** 10-15 minutes post-sprint (prevents repeating mistakes)

---

## Why This Step Exists

**Without Learning Capture:**
- Same bugs repeated across sprints
- New QAs ask same questions veterans answered months ago
- Patterns invisible (you fixed bug X in Sprint 5, similar bug Y appears in Sprint 15)
- Knowledge lost when people leave

**With Learning Capture:**
- Bugs teach us (documented patterns prevent recurrence)
- New QAs reference past learnings (faster onboarding)
- Patterns emerge (Sprint 5 learning informs Sprint 15 decisions)
- Knowledge preserved (institutional memory outlives individuals)

---

## What to Capture

### ✅ Capture This:

1. **Surprises:**
   - "We thought X would be simple, but Y edge case surprised us"
   - "Design changed mid-sprint because of Z constraint we didn't know"

2. **Risks That Materialized:**
   - "We predicted risk A might happen → It did → Here's how we handled it"
   - "Bug B occurred even though we tested for it → Root cause was C"

3. **Risks That Didn't Materialize:**
   - "We worried about D, but it was a non-issue because E"
   - "Assumption F turned out to be wrong"

4. **Patterns:**
   - "This is the 3rd time rounding errors caused bugs → Pattern recognized"
   - "Payment gateway timeouts always happen during peak hours"

5. **What We'd Do Differently:**
   - "Next time, we'll test G scenario earlier"
   - "We should involve finance team in fee calculation validation before development"

### ❌ Don't Capture This:

- Individual bug reports (those live in Jira)
- Daily status updates (those are ephemeral)
- Meeting notes (unless they contain quality insights)

---

## How to Capture Learning (Step-by-Step)

### Action 1: At End of Sprint, Create Learning Notes

**Location:** `documentation/business-units/[bu]/quality-scopes/[feature]/learning-notes/sprint-[number].md`

**Example:**
```bash
cd documentation/business-units/revenue-and-growth/quality-scopes/dynamic-fee-engine/learning-notes/
touch sprint-24.md
```

**Time:** 1 minute

---

### Action 2: Use the Template

**Template:** [Learning Notes Template](./14-templates.md#template-6-learning-notes-post-sprint)

**Fill in:**

```markdown
# Learning Notes - Sprint 24

**Sprint:** Sprint 24 (Feb 5-16, 2026)
**QA:** [Your Name]

## What We Built
Dynamic fee engine MVP: Bronze, Silver, Gold tier support. Fee calculation logic implemented.

## What Went Well
- Early collaboration with finance team prevented fee formula misunderstanding
- Unit tests caught rounding bug before integration testing
- Risk hypothesis about boundary conditions (GTV = Rp 10M exactly) was validated

## What Surprised Us
- Merchant tier assignment cron job took 45 minutes (expected 10 minutes) → performance issue
- Product wanted tier change notifications 30 days in advance (we assumed 7 days)
- Design added tier badges mid-sprint → visual testing scope expanded

## Risks That Materialized
- **Risk:** Fee calculation rounding errors → YES
  - Found: 2.7% of Rp 123,456 = Rp 3,333.312 → System rounded to Rp 3,333.32 (wrong)
  - Fix: Updated rounding logic to banker's rounding (round to even)
  - Test added: Rounding edge case now in regression suite

## Risks That Didn't Materialize
- **Risk:** Race condition (tier change during payment) → NO
  - Why: Tier snapshot at payment initiation design prevented this
  - Learning: Early architectural decision solved potential bug

## What We'd Do Differently
- **Involve finance earlier:** They caught fee formula misunderstanding in week 1 (could've been day 1)
- **Performance test upfront:** GTV calculation job slowness discovered late (should test in Sprint 23)
- **Clarify notification timing earlier:** 30-day requirement discovered mid-sprint (grooming gap)

## Pattern Recognized
**Pattern:** Rounding errors in financial calculations
**Occurrences:** 
- Sprint 18: Subscription proration rounding bug
- Sprint 21: Currency conversion rounding bug  
- Sprint 24: Fee calculation rounding bug (this sprint)

**Root Cause:** Using standard rounding (round half up) instead of banker's rounding
**Fix:** Standardize on banker's rounding for all financial calculations
**Action:** Add to Revenue BU quality standards

## Action Items for Next Sprint
- [ ] Add banker's rounding to Revenue BU coding standards
- [ ] Performance test tier assignment job before Sprint 25 development
- [ ] Create notification timing checklist for grooming (clarify SLAs upfront)
```

**Time:** 10-15 minutes

---

### Action 3: Identify Cross-BU Patterns

**If the learning applies beyond this feature:**

**Create/Update:** `quality-tracking/cross-bu-patterns/[pattern-name].md`

**Example:**
```bash
cd quality-tracking/cross-bu-patterns/
touch rounding-errors-pattern.md
```

**Content:**
```markdown
# Pattern: Rounding Errors in Financial Calculations

**First Observed:** Sprint 18 (Subscription Proration)
**BUs Affected:** Revenue & Growth, potentially Network (if they handle payments)

## The Pattern
Financial calculations involving percentages or currency conversion use standard rounding (round half up) instead of banker's rounding, causing accumulating errors.

## Examples
- Sprint 18: Subscription proration → User charged Rp 50,001 instead of Rp 50,000
- Sprint 21: USD→IDR conversion → $100 * 15,234.50 = Rp 1,523,450.50 rounded to 1,523,451 (should be 1,523,450)
- Sprint 24: Fee calculation → 2.7% of Rp 123,456 = Rp 3,333.312 rounded to 3,333.32 (should be 3,333.31)

## Root Cause
Default rounding in many languages rounds 0.5 up (e.g., 3,333.315 → 3,333.32). Over many transactions, this creates bias toward higher amounts.

## Solution
Use **banker's rounding** (round half to even):
- 3,333.315 → 3,333.32 (5 rounds to nearest even: 2)
- 3,333.325 → 3,333.32 (5 rounds to nearest even: 2)

**Python:**
```python
from decimal import Decimal, ROUND_HALF_EVEN
amount = Decimal('3333.315')
rounded = amount.quantize(Decimal('0.01'), rounding=ROUND_HALF_EVEN)
# Result: Decimal('3333.32')
```

## Recommendation
**All BUs handling money:** Use banker's rounding for financial calculations.
Add to BU quality standards and code review checklist.

**Applies to:** Revenue & Growth, any BU processing payments
```

**Time:** 15 minutes (first time), 5 minutes (update existing pattern)

---

## When to Update Learning

**After every sprint:**
- Write learning notes for Quality Scopes you worked on

**When you discover a pattern:**
- Add to cross-BU patterns

**Monthly:**
- Contribute to monthly quality summary (aggregated learnings)

---

## How Learning Compounds

**Sprint 24:** Document rounding error pattern
**Sprint 25:** New QA testing subscription feature → Reads cross-BU pattern → Tests banker's rounding proactively → Catches bug before release
**Sprint 26:** Engineering adds banker's rounding to coding standards → Pattern becomes prevention

**This is the value: Knowledge → Prevention → Institutional Improvement**

---

## Success Criteria

**You've completed Step 6 when:**

- [ ] You've written learning notes for at least one sprint
- [ ] You've identified at least one pattern (even if specific to one feature)
- [ ] You understand: Learning captured = mistakes not repeated

**Time:** 10-15 minutes per sprint

---

**Next:** [Step 7: Board is for Storytelling](./09-step7-storytelling.md)
