# Step 4: Use Checklist Before Running Anything

**Goal:** Ensure you have sufficient context before testing or using AI

**When:** Before every testing session, before AI analysis, before release decisions

**Time Investment:** 5-10 minutes (saves hours of wasted effort)

---

## Why This Step Exists

**The Problem: Testing on Autopilot**

You receive a ticket → Immediately start testing → Miss critical context → Find bugs late or miss them entirely

**The Solution: Context-First Checklist**

Use [Checklist Before Run](../../checklists/checklist-before-run.md) → Verify you have context → Test with insight

---

## The 7-Point Checklist (Non-Negotiable)

### ✅ 1. I understand the product vision

**Question:** Can you explain in one sentence why this feature exists?

**If NO:** Read product brief, BU context, Quality Scope README
**If YES:** Proceed

---

### ✅ 2. I understand the design intent

**Question:** Do you know the intended UX flow and key interactions?

**If NO:** Review design specs (Figma), Quality Scope design-intent/
**If YES:** Proceed

---

### ✅ 3. I understand the technical context

**Question:** Do you know which components are involved and key dependencies?

**If NO:** Read engineering design doc, Quality Scope technical context
**If YES:** Proceed

---

### ✅ 4. I know what "good quality" means here

**Question:** What quality dimensions matter most for this feature?

**If NO:** Read BU Quality Flavour, review quality gates
**If YES:** Proceed

---

### ✅ 5. I have documented my risk hypotheses

**Question:** Have you written down what could go wrong?

**If NO:** Create risk-hypotheses/ in Quality Scope
**If YES:** Proceed

---

### ✅ 6. I know what tests already exist

**Question:** Do you know existing coverage to avoid duplication?

**If NO:** Check existing-tests-context/ in Quality Scope
**If YES:** Proceed

---

### ✅ 7. I have a clear validation approach

**Question:** Do you know your test scenarios and pass/fail criteria?

**If NO:** Document validation approach in Quality Scope
**If YES:** Ready to test!

---

## When to Use This Checklist

**Always use before:**
- ❗ Starting a testing session
- ❗ Using AI for test ideation or risk analysis
- ❗ Making a release decision (approve/block)

**Optional for:**
- ✅ Quick bug reproduction (you already have context)
- ✅ Trivial changes (typo fix, copy change)

**Rule:** If the feature touches money, security, or compliance → **Always use checklist**

---

## How to Use the Checklist

**Step 1:** Open [Checklist Before Run](../../checklists/checklist-before-run.md)

**Step 2:** Go through each item. If you can't answer "YES", stop and fill the gap.

**Step 3:** Once all items checked, proceed with testing.

**Time:** 5-10 minutes (or signals you need more context)

---

## Success Criteria

**You've completed Step 4 when:**

- [ ] You've used the checklist at least once
- [ ] You understand: Testing without context = guessing
- [ ] You know when the checklist is mandatory vs optional

**Time:** 5-10 minutes per testing session

---

**Next:** [Step 5: AI Assistance (Only After Context Exists)](./07-step5-ai-assistance.md)
