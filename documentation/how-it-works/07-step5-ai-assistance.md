# Step 5: AI Assistance (Only After Context Exists)

**Goal:** Use AI to enhance QA productivity, not replace QA judgment

**When:** Month 2-3 (after Level 1-2 established)

**Time Investment:** Varies (AI can save 30-50% time on analysis/documentation)

---

## CRITICAL: AI is an Assistant, NOT a Decision-Maker

**What AI Does:**
- ✅ Helps brainstorm edge cases
- ✅ Generates test scenario ideas
- ✅ Analyzes complex requirements
- ✅ Speeds up documentation
- ✅ Suggests risk hypotheses

**What AI DOES NOT Do:**
- ❌ Make quality decisions (QA decides)
- ❌ Approve/block releases (QA decides)
- ❌ Prioritize risks (QA decides based on business context)
- ❌ Replace human judgment (AI lacks business context)

**The Rule:** AI suggests, QA decides.

---

## When to Use AI (Month 2+)

**Level 2 - Predictive Quality (Month 2):**
Use AI manually (ChatGPT, Claude) for:
- Context analysis (summarize long product briefs)
- Risk brainstorming (generate edge case ideas)
- Test ideation (suggest test scenarios)
- Documentation (draft learning notes faster)

**Level 3 - Assisted Quality (Month 3):**
Consistent AI usage with:
- Internal prompting guidelines
- Human review of all AI outputs
- Documented AI-assisted insights

---

## How to Use AI Effectively

### Step 1: Provide Context (Critical!)

**Bad Prompt:**
```
Generate test cases for payment feature
```
**Why bad:** No context = generic output

**Good Prompt:**
```
I'm testing a dynamic fee calculation feature for a payment platform.

Context:
- Business goal: Enable tiered pricing based on merchant GTV (Gross Transaction Volume)
- Critical risk: Fee calculation errors directly impact revenue (0.01% error = $1K monthly loss)
- Technical: Fee calculated in Python, involves rounding to 2 decimals, multi-currency support
- Current issue: I'm worried about boundary conditions (merchant GTV exactly at tier threshold)

Help me brainstorm edge cases for tier assignment logic, specifically focusing on:
1. Boundary value testing (GTV exactly at tier threshold)
2. Rounding edge cases
3. Multi-currency GTV calculation

BU Context: Revenue & Growth
Quality Dimension: 100% financial correctness required
```

**Why good:** Specific context = tailored output

---

### Step 2: Review AI Output Critically

**AI says:** "Test with amount Rp 100,000"

**QA reviews:**
- ✅ Good baseline scenario
- ❌ Missing: Rounding edge case (e.g., Rp 123,456 where 2.7% = Rp 3,333.312)
- ❌ Missing: Boundary at tier threshold (GTV exactly Rp 10,000,000)
- ❌ Missing: Multi-currency conversion accuracy

**QA adds:**
```
AI suggested: Rp 100,000 (baseline) ✓
QA added:
- Rp 123,456 (rounding edge case: 2.7% = 3,333.312 → rounds to?)
- GTV = Rp 10,000,000.00 (boundary: Silver tier starts here or ends at Bronze?)
- $1,000 USD transaction (FX rate snapshot: which rate used?)
```

**Human judgment applied.**

---

### Step 3: Document AI-Assisted Insights

**In your Quality Scope:**

```markdown
## Risk Hypotheses (AI-Assisted)

**Method:** Used Claude to brainstorm edge cases for tier assignment logic.

**AI Suggestions:**
- Boundary value testing (GTV at tier thresholds)
- Timezone handling (tier changes at midnight)
- Concurrent transactions (tier changes mid-payment)

**QA Validation:**
- ✅ Boundary testing: CRITICAL (added to P0 scenarios)
- ✅ Timezone handling: IMPORTANT (added to P1 scenarios)
- ⚠️ Concurrent transactions: LOW LIKELIHOOD (tier snapshot at payment initiation mitigates)

**Final Risk List:** [Human-curated based on AI input + business context]
```

**This documents:**
- What AI suggested (transparency)
- What QA validated (judgment applied)
- Final decision (human-owned)

---

## Prompting Guidelines (Month 3)

**Create:** `prompting-guidelines.md` in your BU folder

**Example Guidelines:**

```markdown
# AI Prompting Guidelines - Revenue BU

## Context Template

When using AI for Revenue BU features, always provide:

1. **Business Context:**
   - "This BU handles payment processing and fee calculation"
   - "Revenue impact: X% error = Y monetary loss"

2. **Quality Standards:**
   - "Financial correctness: 100% accuracy required"
   - "Compliance: PCI-DSS Level 1"

3. **Technical Constraints:**
   - "Fee calculation in Python (FastAPI)"
   - "Rounding: 2 decimals, banker's rounding"

4. **Specific Ask:**
   - "Help me brainstorm [specific topic]"
   - "Suggest test scenarios for [specific risk]"

## Review Checklist

Before using AI output:
- [ ] Does it align with BU quality flavour?
- [ ] Does it consider domain-specific risks?
- [ ] Does it account for technical constraints?
- [ ] Have I added business context AI doesn't know?
```

---

## What AI is Good At

**Use AI for:**

1. **Brainstorming:**
   - "What edge cases should I consider for X?"
   - "What could go wrong with Y?"

2. **Pattern Recognition:**
   - "I found bugs A, B, C in past sprints. What's the pattern?"

3. **Documentation Speed:**
   - "Summarize this 20-page PRD into key quality risks"
   - "Draft learning notes from these test session logs"

4. **Test Ideation:**
   - "Suggest test scenarios for Z based on these requirements"

---

## What AI is Bad At

**Don't rely on AI for:**

1. **Business Priority Judgment:**
   - AI doesn't know which OKRs matter most
   - AI doesn't understand political/stakeholder dynamics

2. **Domain-Specific Nuance:**
   - AI doesn't know your BU's quality flavour
   - AI doesn't know your company's risk appetite

3. **Final Decisions:**
   - Should we ship? (Human decides based on risk tolerance)
   - Is this P0 or P1? (Human decides based on business impact)

---

## Success Criteria

**You've completed Step 5 when:**

- [ ] You've used AI at least once for test ideation (Month 2+)
- [ ] You reviewed AI output critically (didn't copy-paste blindly)
- [ ] You documented: "AI suggested X, I validated Y, final decision Z"
- [ ] You understand: AI assists, QA decides

**Time:** Ongoing (Month 2-3)

---

**Next:** [Step 6: Learning Becomes Shared Memory](./08-step6-learning.md)
