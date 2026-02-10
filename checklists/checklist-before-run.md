# Checklist Before Run

**Purpose:** This checklist ensures you have enough context before testing, running AI analysis, or making quality decisions.

**When to use:** Before starting any testing session, before using AI for analysis, before validating a feature.

---

## Context Checklist

### [ ] 1. I understand the product vision

**Questions to answer:**
- What problem does this feature solve for users?
- What is the success criteria from a product perspective?
- How does this align with current OKRs?

**Where to find it:**
- Product brief (link in feature ticket)
- Business Unit context: `documentation/business-units/[bu]/_bu-context.md`
- OKR alignment: `documentation/business-units/[bu]/_okr-alignment.md`

**Red flag:** If you can't explain the "why" in one sentence, you don't have enough context yet.

---

### [ ] 2. I understand the design intent

**Questions to answer:**
- What is the intended user experience?
- What are the key UX flows?
- Are there specific visual or interaction requirements?

**Where to find it:**
- Design specs (Figma link in ticket)
- Quality Scope: `documentation/business-units/[bu]/quality-scopes/[feature]/design-intent/`

**Red flag:** If you don't know what the happy path looks like, you can't test deviations.

---

### [ ] 3. I understand the technical context

**Questions to answer:**
- What components are involved (backend, frontend, third-party integrations)?
- What are the key dependencies?
- Are there performance requirements (SLA, latency)?

**Where to find it:**
- Engineering design doc (link in ticket)
- Quality Scope: `documentation/business-units/[bu]/quality-scopes/[feature]/README.md` (Technical Context section)

**Red flag:** If you don't know where the code lives, you can't validate the implementation.

---

### [ ] 4. I know what "good quality" means for this feature

**Questions to answer:**
- What quality dimensions matter most (correctness, performance, security, UX)?
- What are the non-negotiable quality gates?
- How does this BU define quality differently from others?

**Where to find it:**
- BU quality flavour: `documentation/business-units/[bu]/_quality-flavour.md`
- Quality benchmarks: `benchmarks/quality-standards/`

**Red flag:** If you're applying generic quality thinking instead of BU-specific context, you'll miss critical risks.

---

### [ ] 5. I have documented my quality risk hypotheses

**Questions to answer:**
- What could go wrong with this feature?
- What edge cases am I worried about?
- What assumptions am I making?

**Where to document:**
- Quality Scope: `documentation/business-units/[bu]/quality-scopes/[feature]/risk-hypotheses/`

**Red flag:** If you haven't written down your risks, you're testing on autopilot.

---

### [ ] 6. I know what tests already exist

**Questions to answer:**
- What automated tests cover this area?
- What was tested manually in the past?
- Are there known gaps in coverage?

**Where to find it:**
- Quality Scope: `documentation/business-units/[bu]/quality-scopes/[feature]/existing-tests-context/`
- Test repository (link in README)

**Red flag:** If you're duplicating existing test coverage, you're wasting time.

---

### [ ] 7. I have a clear validation approach

**Questions to answer:**
- How will I validate quality (manual, automated, both)?
- What are my test scenarios (happy path, edge cases, error cases)?
- What is my pass/fail criteria?

**Where to document:**
- Quality Scope: `documentation/business-units/[bu]/quality-scopes/[feature]/validation-notes/`

**Red flag:** If you're "just exploring," you won't know when you're done.

---

## AI-Specific Checklist (If Using AI for Analysis)

### [ ] 8. I will use AI as an assistant, not a decision-maker

**Commitments:**
- AI will help brainstorm risks, generate test ideas, analyze context
- I (QA) will make final decisions on what matters and what doesn't
- I will review all AI outputs before acting on them

**Why it matters:**
AI can suggest, but only humans understand business context and stakeholder priorities.

---

### [ ] 9. I will provide AI with relevant context

**What to include in AI prompts:**
- Product vision (copy from product brief)
- Design intent (key UX flows)
- Technical constraints (from engineering doc)
- Quality risks I've already identified

**Why it matters:**
AI without context generates generic advice. AI with context generates tailored insights.

**Example Good Prompt:**
```
I'm testing a dynamic fee calculation feature for a payment platform.
- Product goal: Enable tiered pricing based on merchant transaction volume
- Key risk: Fee calculation errors directly impact revenue (0.01% error = $1K monthly)
- Technical context: Fee calculated in Python, involves rounding to 2 decimals
- My concern: Boundary conditions (merchant GTV exactly at tier threshold)

Help me brainstorm edge cases for tier assignment logic.
```

**Example Bad Prompt:**
```
Generate test cases for fee calculation.
```

---

### [ ] 10. I will document AI-assisted insights (not just copy-paste)

**What to capture:**
- AI suggestions that I found valuable (with my validation notes)
- AI suggestions that I rejected (with reasoning why)
- My own insights that AI helped me reach

**Where to document:**
- Quality Scope: `documentation/business-units/[bu]/quality-scopes/[feature]/validation-notes/`

**Why it matters:**
Future you (and other QAs) need to understand your thinking process, not just AI output.

---

## Quick Self-Check

**Before starting testing or analysis, ask yourself:**

1. **Can I explain the "why"?**
   - Why does this feature exist?
   - Why does quality matter here?
   - Why am I testing this specific scenario?

2. **Do I have assumptions documented?**
   - What am I assuming about user behavior?
   - What am I assuming about system behavior?
   - What am I assuming about business rules?

3. **Do I know my exit criteria?**
   - When will I consider testing complete?
   - What would make me block release?
   - What would make me approve with confidence?

**If you answered "no" or "not sure" to any of these, go back and fill the gaps.**

---

## When to Skip This Checklist

**You can skip if:**
- ✅ You're doing a quick bug reproduction (already have full context)
- ✅ You're fixing a typo or trivial change (low risk)
- ✅ You're re-testing something you validated yesterday (context fresh)

**You CANNOT skip if:**
- ❌ This is your first time seeing this feature
- ❌ You're about to make a release decision
- ❌ You're using AI to generate test scenarios
- ❌ This feature touches money, security, or compliance

---

## Post-Checklist: You're Ready When...

✅ You can explain the feature to a non-technical person in 2 sentences
✅ You have documented at least 3 quality risks specific to this feature
✅ You know where to find product, design, and technical docs
✅ You have a validation plan (not just "I'll test it")
✅ You know what "done" looks like

---

**Reminder:** This checklist isn't bureaucracy. It's protection against wasted effort.

Testing without context is guessing. Guessing wastes time and misses critical risks.

**Time investment:** 15-20 minutes upfront saves hours of rework later.

---

**Next:** Start testing with confidence, knowing you have the context to make good quality decisions.
