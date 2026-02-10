# Step 2: Start with Business Unit Context (Not Tests)

**Goal:** Understand domain-level quality context before testing anything

**When:** Before working on ANY feature in a Business Unit for the first time

**Time Investment:** 20-30 minutes upfront, saves hours of confusion later

---

## Why This Step Exists

**The Anti-Pattern: Test-First Without Context**

```
QA receives ticket → Immediately writes test cases → Misses critical risks
```

**Why this fails:**
- You don't know what "good quality" means in this domain
- You don't understand business priorities (what matters most?)
- You miss domain-specific edge cases (assumptions you didn't know to question)
- You apply generic testing thinking (instead of BU-specific wisdom)

**The Quality Intelligence Way: Context-First**

```
QA reads BU context → Understands domain risks → Tests with business insight
```

**Why this works:**
- You know what quality dimensions matter (correctness? speed? compliance?)
- You understand business constraints (regulatory? financial? operational?)
- You catch domain-specific edge cases (because you know the domain)
- You test like a domain expert (not a generic QA)

---

## What is Business Unit (BU) Context?

**Business Unit (BU):** A group of products/features serving a specific business function

**Examples:**
- **Revenue & Growth:** Payment processing, fees, subscriptions, payouts
- **Network:** User connections, social features, messaging, notifications
- **Pivot:** Experimental features, A/B tests, new market initiatives

**BU Context = Domain Knowledge Required for Quality Decisions**

Includes:
- What this BU does (core responsibility)
- Who the stakeholders are (Product, Engineering, Design leads)
- What products/features this BU owns
- Core user journeys
- Technical stack and integrations
- Domain-specific terminology
- What "good quality" means **in this specific domain**

---

## Step-by-Step: How to Absorb BU Context

### Action 1: Navigate to Your BU Folder

```bash
cd documentation/business-units/[your-bu]/
```

**Example:**
```bash
cd documentation/business-units/revenue-and-growth/
```

**What you'll find:**
- `_bu-context.md` - Domain understanding
- `_quality-flavour.md` - What "good quality" means here
- `_okr-alignment.md` - Business objectives this quarter
- `quality-scopes/` - Feature-level documentation

**Time:** 1 minute

---

### Action 2: Read `_bu-context.md`

**File:** `documentation/business-units/[your-bu]/_bu-context.md`

**What to focus on:**

1. **"What This BU Does" section**
   - What is the core responsibility?
   - What business problem does this BU solve?

2. **"Main Products/Features Owned" section**
   - What are the key products?
   - Which ones are most critical to the business?

3. **"Core User Journeys" section**
   - What are the happy path flows?
   - Which journeys generate revenue or drive engagement?

4. **"Technical Context" section**
   - What tech stack is used?
   - What third-party integrations exist?
   - Where are the technical dependencies?

5. **"Domain-Specific Terminology" section**
   - What jargon do I need to know?
   - What do these terms mean in this context?

**How to read:**
- **First pass (5 min):** Skim for general understanding
- **Second pass (10 min):** Deep read the sections most relevant to your upcoming feature
- **Third pass (ongoing):** Reference when you encounter unfamiliar terms

**Time:** 15 minutes (first time), 5 minutes (refresher)

---

### Action 3: Read `_quality-flavour.md`

**File:** `documentation/business-units/[your-bu]/_quality-flavour.md`

**This is the most important file for QA.**

**What to focus on:**

1. **"Core Quality Philosophy" section**
   - What makes quality different in this BU?
   - What quality dimensions are non-negotiable?

2. **"Quality Dimensions" section (usually 3-5 dimensions)**
   - Dimension 1: What it means, why it matters, how we measure it
   - Dimension 2: ...
   - Dimension 3: ...

   **Example for Revenue BU:**
   - Dimension 1: Financial Correctness (100% accuracy required)
   - Dimension 2: Transaction Reliability (>95% success rate)
   - Dimension 3: Regulatory Compliance (PCI-DSS, no exceptions)

3. **"Quality Gates Specific to This BU" section**
   - What must be true before we ship?
   - What are the non-negotiable checkpoints?

4. **"Known Risk Areas" section**
   - What has caused problems before?
   - What should I always be paranoid about?

**Why this matters:**

**Without reading this:** You apply generic QA thinking
- "I'll test happy path, error cases, and edge cases"

**After reading this:** You apply domain-specific QA thinking
- "In Revenue BU, fee calculation errors directly impact revenue. I need to test rounding edge cases with exact expected amounts. 0.01% error = Rp 1M monthly loss."

**Time:** 10 minutes

---

### Action 4: (Optional) Skim `_okr-alignment.md`

**File:** `documentation/business-units/[your-bu]/_okr-alignment.md`

**When to read:**
- When you need to understand business priorities
- When deciding how much time to invest in quality for a feature
- When trade-offs arise (speed vs thoroughness)

**What you'll find:**
- Current quarter OKRs (Objectives and Key Results)
- How QA contributes to each OKR
- Quality priorities aligned to business goals

**Example insight:**
- OKR: "Achieve 97% payment success rate (currently 94.2%)"
- QA contribution: "Test timeout scenarios, retry logic, idempotency"
- Your takeaway: Payment reliability is a **top priority** this quarter → invest extra time here

**Time:** 5 minutes (skim), 10 minutes (deep read if needed)

---

## How to Apply BU Context to Your Work

### Scenario: You're Testing a New Payment Method Integration

**Without BU Context:**
```
✅ Test: Payment goes through
✅ Test: Payment fails with invalid card
✅ Test: Payment page loads
```

**With Revenue BU Context:**

From `_bu-context.md`, you know:
- Revenue BU charges 2.9% + Rp 2,000 per transaction
- This BU depends on third-party payment gateways (Stripe, Midtrans)
- Cross-border transactions require multi-currency handling

From `_quality-flavour.md`, you know:
- **Financial Correctness:** Fee calculation must be 100% accurate
- **Transaction Reliability:** Payment success rate target >95%
- **Regulatory Compliance:** PCI-DSS requires no card data in logs

**Now your test approach:**
```
✅ Test: Fee calculation for new payment method (exact expected amount)
✅ Test: Multi-currency transaction (USD → IDR conversion correct?)
✅ Test: Payment gateway timeout scenario (retry logic works?)
✅ Test: Idempotency (duplicate webhook doesn't double-charge)
✅ Test: PCI-DSS compliance (card data never logged)
✅ Test: Boundary conditions (minimum transaction amount handled?)
```

**See the difference?** BU context = smarter testing.

---

## Red Flags: You Need More BU Context If...

**❌ You don't know what "good" looks like**
- "I tested it and it works" ← What does "works" mean in this domain?
- **Fix:** Read Quality Flavour to understand success criteria

**❌ You don't know the business impact of a bug**
- "I found a bug in fee calculation, is it P1 or P2?"
- **Fix:** Read BU Context to understand revenue impact

**❌ You don't understand the jargon**
- "What's a 'chargeback'? What's 'proration'?"
- **Fix:** Check Domain-Specific Terminology section

**❌ You can't explain why this BU exists**
- "I don't know what Revenue BU's purpose is"
- **Fix:** Read "What This BU Does" section

**❌ You're applying copy-paste test cases from another BU**
- "I'll use the same test approach as Network BU"
- **Fix:** Each BU has different quality risks (read Quality Flavour)

---

## When to Update BU Context

**BU Context is living documentation.** Update when:

### You should update:

1. **New product/feature launched**
   - Add to "Main Products/Features Owned"
   - Update technical stack if new integrations added

2. **New quality dimension discovered**
   - Found a risk type we didn't consider before?
   - Add to Quality Flavour

3. **OKRs change (quarterly)**
   - Update OKR Alignment file with new quarter's objectives

4. **Technical stack changes**
   - Migrated database? New payment provider?
   - Update Technical Context section

### You shouldn't update:

- ❌ Daily changes (BU context is stable)
- ❌ Feature-specific details (those go in Quality Scopes, not BU Context)
- ❌ Individual bugs (those go in issue tracker, not BU docs)

**Update frequency:** Quarterly reviews + ad-hoc when major changes happen

---

## Common Questions

### Q: Do I need to read BU context for EVERY feature?

**A:** Read it once per BU, then reference as needed.

**First time in a BU:** Read all three files (Context, Quality Flavour, OKRs) - 30 minutes
**Subsequent features:** Quick skim to refresh (5 minutes)
**When stuck:** Reference specific sections

---

### Q: What if my BU doesn't have context documented yet?

**A:** You write it! (Or pair with a senior QA to write it together)

**Use the template:**
- [BU Context Template](./14-templates.md#template-2-business-unit-context)
- [Quality Flavour Template](./14-templates.md#template-3-quality-flavour)

**Start simple:**
- 3 bullets for "What This BU Does"
- 3 quality dimensions
- 5 known risk areas

**You can expand it over time.**

---

### Q: What if I disagree with something in BU Context?

**A:** Discuss with the team, then update the document.

**Process:**
1. Note your disagreement (e.g., "I think compliance is more critical than stated")
2. Bring it up in team meeting or 1-on-1 with QA Lead
3. If consensus reached, update the document
4. If no consensus, document both perspectives

**Example:**
```markdown
## Quality Dimension: Speed vs Correctness

**Current stance:** Correctness > Speed (we can delay release for quality)
**Alternate view (QA2):** For experimental features, speed > correctness (fail fast, learn quickly)
**Resolution:** Apply on case-by-case basis (prod features = correctness first; experiments = speed first)
```

---

### Q: Can BU Context be wrong?

**A:** Yes, and that's okay. Fix it when you discover errors.

**Example:**
```markdown
## Update Log

**2026-02-15:** Corrected fee calculation formula. Was 2.9% + Rp 2,000, now variable based on merchant tier. - [Your Name]
```

**Trust the process:** Living documentation gets better over time.

---

## Success Criteria for This Step

**You've completed Step 2 when:**

- [ ] You've read your BU's Context file (15 min)
- [ ] You've read your BU's Quality Flavour (10 min)
- [ ] You can explain what this BU does in 2 sentences
- [ ] You can name 3 quality dimensions that matter in this BU
- [ ] You know at least 5 domain-specific terms
- [ ] You can identify why quality is different in this BU vs others

**Time to complete:** 20-30 minutes (first time per BU)

---

## Real-World Example: Revenue BU

**Before reading BU Context:**
"I'll test the new payment feature like any other feature."

**After reading BU Context:**

From `_bu-context.md`:
- "Revenue BU charges percentage-based fees on transactions"
- "Multi-currency support required (USD, SGD, IDR)"
- "PCI-DSS Level 1 certification required"

From `_quality-flavour.md`:
- "Financial Correctness = 100% (every Rupiah counts)"
- "0.01% fee error = Rp 1M monthly revenue impact at scale"
- "Compliance is non-negotiable (violations shut down platform)"

**Now I know:**
- ✅ I must test fee calculation with exact expected amounts (not approximations)
- ✅ I must test multi-currency edge cases (FX rate handling, rounding)
- ✅ I must validate PCI-DSS compliance (no card data in logs, secure transmission)
- ✅ I must test at scale (performance under high transaction volume)

**Impact:** Smarter testing, fewer production issues, revenue protected.

---

## What Happens If We Skip This Step?

**Skipping BU Context = Testing Blind**

- You miss domain-specific edge cases (generic tests don't catch specialized risks)
- You can't prioritize risks (don't know what matters most)
- You duplicate work (unaware of existing knowledge)
- You make wrong quality trade-offs (don't understand business priorities)
- You test slower (keep asking "what does this mean?")

**Using BU Context = Testing with Insight**

- You catch domain-specific risks (know what to look for)
- You prioritize effectively (test critical paths first)
- You leverage existing knowledge (build on what's known)
- You make informed trade-offs (understand business impact)
- You test faster (domain fluency reduces friction)

---

## Next Step

Once you understand your BU's domain context, move to:

**[Step 3: Capture Deep Context Using Quality Scopes](./05-step3-quality-scopes.md)**

You'll learn how to document feature-level context that builds on BU understanding.

---

**Remember:** Quality risks come from business goals and constraints, not test cases.

Understand the "why" before deciding the "how."
