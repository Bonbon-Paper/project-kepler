# Step 7: Board is for Storytelling (Not Raw Data)

**Goal:** Use Quality Learning Board to communicate trends and insights to stakeholders

**When:** Month 2+ (after quality signals tracked)

**Time Investment:** 30 minutes monthly (leadership visibility)

---

## What is the Quality Learning Board?

**Location:** `boards/index.html`

**Purpose:** Visual dashboard showing quality trends, not individual bugs.

**Audience:**
- **Primary:** QA team (track progress, spot patterns)
- **Secondary:** Leadership (understand quality health)
- **Tertiary:** Product/Engineering (cross-functional transparency)

---

## What Goes on the Board

### ✅ Board Shows:

1. **Trends:**
   - Payment success rate over time (94.2% → 95.1% → 96.3%)
   - Late-stage bugs discovered per sprint (decreasing trend)
   - Quality signals (hotfixes, rework, production incidents)

2. **Insights:**
   - "3 rounding errors found across Revenue BU → Pattern recognized → Banker's rounding standard added"
   - "Early QA involvement (Month 1) reduced rework by 30%"

3. **Progress:**
   - "Month 1: QA joined 82% of grooming sessions (target: 80%)"
   - "Month 2: 5 Quality Scopes documented"

### ❌ Board Does NOT Show:

- Individual bug reports (those are in Jira)
- Test case execution status (those are in test management tools)
- Daily/weekly micro-updates (board is for monthly storytelling)

---

## How to Update the Board

### Action 1: Collect Quality Signals (Ongoing)

**Quality Signals = Indicators of quality health**

**Track these monthly:**
- Hotfixes deployed (how many? for what?)
- Late-stage bugs (discovered after development "done")
- Rework requests (features sent back for quality fixes)
- Production incidents (P0/P1 bugs that escaped)

**Store in:** `quality-tracking/monthly-summaries/[YYYY-MM].md`

---

### Action 2: Aggregate Learning (Monthly)

**At month-end:**

1. Review all Quality Scopes' learning notes from this month
2. Identify cross-cutting themes
3. Calculate quality metrics (success rates, reconciliation accuracy, etc.)
4. Write monthly quality summary

**Template:** [Monthly Quality Summary Template](./14-templates.md#template-8-monthly-quality-summary)

---

### Action 3: Update Board Visualization (Monthly)

**File:** `boards/data.js`

**Add data point:**
```javascript
{
  month: "2026-02",
  metrics: {
    paymentSuccessRate: 95.1, // Up from 94.2%
    feeAccuracy: 99.98, // Up from 99.95%
    lateStageIssues: 8, // Down from 12
    grooming Attendance: 85 // Up from 80%
  },
  insights: [
    "Rounding error pattern identified → Banker's rounding standard added",
    "Early QA involvement reduced rework by 30%"
  ]
}
```

**Time:** 15 minutes

---

### Action 4: Share with Stakeholders (Monthly)

**Who:** Email or present monthly summary to:
- QA team (retrospective input)
- Engineering leads (quality partnership)
- Product leads (delivery predictability)
- Leadership (strategic visibility)

**Format:**
- Link to board visualization
- 1-page summary (wins, challenges, next month focus)

**Time:** 15 minutes

---

## The Storytelling Framework

**Board tells a story:**

**Month 1 (Baseline):**
- "QA joined 82% grooming sessions"
- "Payment success: 94.2%, Fee accuracy: 99.95%"
- "12 late-stage issues discovered"

**Month 2 (Progress):**
- "QA joined 85% grooming sessions (+3%)"
- "Payment success: 95.1% (+0.9%), Fee accuracy: 99.98% (+0.03%)"
- "8 late-stage issues discovered (-33%)"
- "Pattern recognized: Rounding errors → Standard added"

**Month 3 (Impact):**
- "QA joined 88% grooming sessions"
- "Payment success: 96.3% (on track for 97% target)"
- "5 late-stage issues discovered (-58% from Month 1)"
- "Rounding standard prevented 2 bugs proactively"

**The story:** Quality Intelligence is working. Metrics improving, patterns captured, prevention happening.

---

## Success Criteria

**You've completed Step 7 when:**

- [ ] You've contributed to at least one monthly quality summary
- [ ] You understand: Board = trends/insights, not individual bugs
- [ ] You know: Storytelling makes quality work visible to leadership

**Time:** 30 minutes monthly

---

## Final Step Complete!

**You've now learned all 7 steps of Quality Intelligence:**

1. ✅ Everyone uses the same repository
2. ✅ Start with Business Unit context
3. ✅ Capture deep context in Quality Scopes
4. ✅ Use checklist before running anything
5. ✅ AI assists (but doesn't decide)
6. ✅ Learning becomes shared memory
7. ✅ Board tells the quality story

**What's next?**

- **Month 1:** Focus on Steps 1-4 (foundation, context, scopes, checklist)
- **Month 2:** Add Steps 5-6 (AI assistance, learning capture)
- **Month 3:** Add Step 7 (board storytelling)
- **Month 4:** Standardize (playbook, benchmarks, sustainability)

---

**Congratulations! You're ready to pioneer Quality Intelligence.** 🚀
