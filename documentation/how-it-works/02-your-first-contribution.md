# Your First Contribution - Quality Intelligence

**Goal:** Make your first meaningful contribution to the Quality Intelligence repository in the next 15 minutes.

No pressure. No perfection required. Just one small step.

---

## Choose Your Path

Pick the option that fits your situation **right now**:

### Path A: "I'm working on a feature this sprint"
→ [Create a Quality Scope](#path-a-create-your-first-quality-scope)

### Path B: "I want to understand my Business Unit better"
→ [Add BU Context](#path-b-add-bu-context)

### Path C: "I just finished a sprint and learned something"
→ [Capture a Learning Note](#path-c-capture-a-learning-note)

### Path D: "I'm not sure what to do"
→ [Start with the Smallest Action](#path-d-start-with-the-smallest-action)

---

## Path A: Create Your First Quality Scope

**When:** You're starting work on a feature, epic, or significant change

**Time:** 10-15 minutes

**Steps:**

### 1. Navigate to Your BU's Quality Scopes

```bash
cd documentation/business-units/[your-bu]/quality-scopes/
```

Example: `documentation/business-units/network/quality-scopes/`

### 2. Create a Folder for Your Feature

```bash
mkdir my-feature-name
cd my-feature-name
```

Name it something descriptive:
- ✅ `user-authentication-sso`
- ✅ `payment-gateway-integration`
- ✅ `dashboard-performance-refactor`
- ❌ `feature-123` (too generic)
- ❌ `new-stuff` (not descriptive)

### 3. Create a Quick Context File

Create `README.md` inside your feature folder:

```markdown
# [Feature Name] - Quality Scope

## Why This Exists
[1-2 sentences: What problem does this feature solve?]

## Product Vision
[Link to product brief or copy key points]

## Design Intent
[Link to design docs or note key UX intentions]

## Quality Risks I'm Thinking About
- [ ] Risk 1: [What could go wrong?]
- [ ] Risk 2: [What assumptions are we making?]
- [ ] Risk 3: [What edge cases matter?]

## Validation Approach
[How will you validate quality? Manual testing? Automation? Both?]

## Notes
[Any other context useful for future you or other QAs]
```

**Example:**

```markdown
# SSO Authentication - Quality Scope

## Why This Exists
Users need to log in using their company's Single Sign-On instead of creating separate accounts.

## Product Vision
See: [Link to product brief]
Success = 80% of enterprise users adopt SSO within 30 days of rollout.

## Design Intent
See: [Link to Figma]
Key UX intention: Seamless transition from old login to SSO with zero confusion.

## Quality Risks I'm Thinking About
- [ ] Risk 1: Session handling across SSO providers (Google, Okta, Azure AD)
- [ ] Risk 2: Fallback behavior if SSO provider is down
- [ ] Risk 3: Data migration for existing users switching to SSO
- [ ] Risk 4: Security: token validation and expiration handling

## Validation Approach
- Manual testing with test accounts from each SSO provider
- Automated tests for token validation logic
- Security review checklist for auth flows

## Notes
- Engineering mentioned using OAuth 2.0 - need to understand token refresh flow
- Design wants branded SSO buttons per provider - visual testing needed
```

### 4. Commit and Move On

That's it. You've created your first Quality Scope.

**Next sprint**, you'll add learning notes to this folder. Future you (or another QA) will thank you for the context.

---

## Path B: Add BU Context

**When:** You understand your Business Unit well and want to document it

**Time:** 15-20 minutes

**Steps:**

### 1. Navigate to Your BU Folder

```bash
cd documentation/business-units/[your-bu]/
```

### 2. Create or Update `_bu-context.md`

```markdown
# [BU Name] - Business Unit Context

## What This BU Does
[What is the core responsibility of this Business Unit?]

## Key Stakeholders
- Product Lead: [Name]
- Engineering Lead: [Name]
- Design Lead: [Name]
- QA: [Name(s)]

## Main Products/Features Owned
- [Product/Feature 1]: [Brief description]
- [Product/Feature 2]: [Brief description]
- [Product/Feature 3]: [Brief description]

## Core User Journeys
1. [User journey 1 - e.g., "User signs up for service"]
2. [User journey 2 - e.g., "User makes first transaction"]
3. [User journey 3 - e.g., "User invites team members"]

## Technical Context
- Primary tech stack: [e.g., React, Node.js, PostgreSQL]
- Key integrations: [e.g., Stripe, SendGrid, AWS S3]
- Deployment: [e.g., Kubernetes on AWS]

## Notes
[Anything else useful for understanding this BU]
```

### 3. Create or Update `_quality-flavour.md`

```markdown
# [BU Name] - Quality Flavour

## What "Good Quality" Means Here

For [BU Name], quality means:

### 1. [Quality Dimension 1]
**Why it matters:** [Explanation]
**How we measure it:** [Metric or signal]
**Example:** [Concrete example]

### 2. [Quality Dimension 2]
**Why it matters:** [Explanation]
**How we measure it:** [Metric or signal]
**Example:** [Concrete example]

### 3. [Quality Dimension 3]
**Why it matters:** [Explanation]
**How we measure it:** [Metric or signal]
**Example:** [Concrete example]

## Quality Gates Specific to This BU
- [ ] [Gate 1: e.g., "All payment flows tested with test credit cards"]
- [ ] [Gate 2: e.g., "Accessibility audit passed for all new UI"]
- [ ] [Gate 3: e.g., "Performance budget met: <2s page load"]

## Known Risk Areas
- [Risk area 1: e.g., "Third-party payment gateway downtime"]
- [Risk area 2: e.g., "Edge cases in subscription billing logic"]
- [Risk area 3: e.g., "Mobile browser compatibility for payment forms"]
```

**Example for Revenue BU:**

```markdown
# Revenue BU - Quality Flavour

## What "Good Quality" Means Here

For Revenue BU, quality means:

### 1. Financial Correctness
**Why it matters:** Incorrect billing loses customer trust and revenue
**How we measure it:** Zero financial discrepancies in production
**Example:** Subscription renewal amount matches user's plan exactly

### 2. Regulatory Compliance
**Why it matters:** Non-compliance risks legal penalties and service shutdown
**How we measure it:** 100% compliance with PCI-DSS, tax regulations
**Example:** Credit card data never logged or stored outside PCI-compliant vault

### 3. Transaction Reliability
**Why it matters:** Failed payments directly impact revenue and user experience
**How we measure it:** <0.1% payment failure rate (excluding user errors)
**Example:** Retry logic handles transient gateway errors gracefully

## Quality Gates Specific to This BU
- [ ] All payment flows tested with test cards (success, failure, edge cases)
- [ ] Tax calculation verified for multi-region scenarios
- [ ] Subscription state machine validated (active, paused, cancelled, expired)
- [ ] Refund flows tested end-to-end

## Known Risk Areas
- Third-party payment gateway downtime (Stripe, PayPal)
- Currency conversion edge cases
- Proration logic for mid-cycle plan changes
- Invoice generation timing and accuracy
```

### 4. Done!

You've documented domain knowledge that helps everyone understand what quality means in your BU.

---

## Path C: Capture a Learning Note

**When:** You just finished a sprint and learned something valuable

**Time:** 5-10 minutes

**Steps:**

### 1. Go to the Quality Scope You Worked On

```bash
cd documentation/business-units/[your-bu]/quality-scopes/[feature-name]/
```

### 2. Create or Update `learning-notes/`

Create `learning-notes/sprint-[number].md`:

```markdown
# Learning Notes - Sprint [Number]

## What We Built
[Brief: what was delivered this sprint?]

## What Went Well (Quality Perspective)
- [Thing 1]
- [Thing 2]
- [Thing 3]

## What Surprised Us
- [Surprise 1: unexpected issue, edge case, or insight]
- [Surprise 2: ...]

## Risks That Materialized
- [Risk we predicted that actually happened]
- [How we handled it]

## Risks That Didn't Materialize
- [Risk we worried about that turned out to be non-issue]
- [Why it didn't happen - was our assumption wrong?]

## What We'd Do Differently Next Time
- [Learning 1]
- [Learning 2]

## Pattern Recognized
[Is this learning specific to this feature, or does it apply to other features/BUs too?]

## Action Items for Next Sprint
- [ ] [Action 1]
- [ ] [Action 2]
```

**Example:**

```markdown
# Learning Notes - Sprint 23

## What We Built
SSO authentication MVP: Google and Okta providers, user account migration flow

## What Went Well
- Early collaboration with Engineering on token validation logic
- Design provided test accounts for each SSO provider
- Automated tests caught session expiration bug before manual testing

## What Surprised Us
- Azure AD integration more complex than expected (enterprise-specific configs)
- Users expect "Remember me" option even with SSO (UX assumption mismatch)
- Logout behavior differs across SSO providers (some redirect, some don't)

## Risks That Materialized
- Risk: "Session handling across SSO providers" → YES
  - We found inconsistent token refresh behavior between Google and Okta
  - Fixed by standardizing refresh logic in our backend

## Risks That Didn't Materialize
- Risk: "Fallback behavior if SSO provider is down" → NO
  - We tested this extensively but SSO providers were stable during sprint
  - Fallback logic exists but not battle-tested in production yet

## What We'd Do Differently Next Time
- Start with one SSO provider (Google) and validate thoroughly before adding others
- Request design mocks for error states earlier (we improvised on logout errors)
- Include performance testing for token validation (we assumed it'd be fast, didn't measure)

## Pattern Recognized
**Pattern:** Third-party integrations always have more edge cases than docs suggest.
**Applies to:** Any feature integrating external services (payment gateways, email providers, etc.)

## Action Items for Next Sprint
- [ ] Add Azure AD to Quality Scope and test enterprise SSO configs
- [ ] Document SSO provider differences in knowledge base
- [ ] Add "Remember me" UX discussion to backlog (design input needed)
```

### 3. Update the Quality Learning Board

*(Optional for now - we'll cover this in [09-step7-storytelling.md](./09-step7-storytelling.md))*

Add a summary of your learning to `boards/data.js` so it appears on the Quality Learning Board.

---

## Path D: Start with the Smallest Action

**When:** You're not sure what to do, or feeling overwhelmed

**Time:** 5 minutes

**Steps:**

### 1. Read One BU Context File

Pick any Business Unit (even if it's not yours):

```bash
cat documentation/business-units/network/_bu-context.md
```

Just read it. That's all.

### 2. Tomorrow: Pick Another File

Maybe `_quality-flavour.md` from the same BU.

### 3. Day 3: Create a Learning Note

Write down one thing you learned this week about quality.

Doesn't matter if it's profound. Could be:
- "Edge case X slipped through because we didn't test Y scenario"
- "Design intent wasn't clear, caused rework—next time ask earlier"
- "AI-generated test ideas were helpful for brainstorming risks"

---

## What If I Make a Mistake?

**You won't break anything.** This is a Git repository. Mistakes can be undone.

**Common "mistakes" that are actually fine:**
- ✅ Putting a note in the "wrong" folder → We'll move it later if needed
- ✅ Writing too little → Better than writing nothing
- ✅ Writing too much → Future you can edit it down
- ✅ Not sure if something is a "pattern" → Document it anyway, we'll decide together

**The only real mistake:**
- ❌ Not contributing at all because you're waiting for perfection

---

## Cheat Sheet: Quick Actions

| Situation | Action | Time |
|-----------|--------|------|
| Starting a new feature | Create Quality Scope folder + README | 10 min |
| Finished a sprint | Add learning notes to Quality Scope | 5-10 min |
| Want to document BU knowledge | Create/update `_bu-context.md` | 15 min |
| Learned something valuable | Write it down in `learning-notes/` | 5 min |
| Not sure what to do | Read one existing context file | 5 min |
| Found a cross-BU pattern | Add to `quality-tracking/cross-bu-patterns/` | 10 min |

---

## After Your First Contribution

**Congratulations!** You've contributed to Quality Intelligence.

**What happens next:**
1. Your contribution is now visible to the team
2. Others can learn from your thinking
3. Future you will thank you for the context
4. You've built the habit (the hardest part)

**Next sprint:**
- Add to what you created
- Reference what others created
- Notice how the knowledge compounds

**Next month:**
- You'll naturally know where things go
- You'll reference the repo when making quality decisions
- You'll see patterns across sprints

---

## Need Help?

**If you're stuck:**
1. Look at existing examples in other BUs
2. Ask in QA team channel: "Where should I put X?"
3. Make your best guess and note: "Not sure if this is the right place"

**If something doesn't make sense:**
- Open an issue or suggest changes to these docs
- The system evolves based on what works for us

---

## Next Step

Once you've made your first contribution, explore the step-by-step workflow:

- [03-step1-foundation.md](./03-step1-foundation.md) - Everyone uses the same repository
- [04-step2-bu-context.md](./04-step2-bu-context.md) - Start with Business Unit context
- [05-step3-quality-scopes.md](./05-step3-quality-scopes.md) - Capture deep context

---

**Remember:** Consistency > Completeness.

One small contribution per sprint beats perfect documentation once.

You've got this. 🚀
