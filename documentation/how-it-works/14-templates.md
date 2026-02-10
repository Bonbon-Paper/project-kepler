# Templates - Quality Intelligence

**Quick access to all templates for consistent documentation**

---

## Template 1: QA Context Brief

**When to use:** Before grooming or sprint planning for any significant feature

**Location:** Create in Quality Scope folder

```markdown
# [Feature Name] - QA Context Brief

**Created:** [Date]
**Sprint:** [Sprint Number]
**QA Lead:** [Your Name]

## Product Vision (1-2 sentences)
[What problem does this solve? Why does it matter?]

## Design Intent (Key UX flows)
- [Happy path user journey]
- [Key interaction points]
- [Visual/accessibility requirements]

## Technical Context
- Components involved: [Backend services, frontend, integrations]
- Key dependencies: [Third-party APIs, databases, etc.]
- Performance requirements: [SLA, latency targets]

## Quality Risks (Top 3-5)
1. [Risk 1: What could go wrong?]
2. [Risk 2: What edge cases matter?]
3. [Risk 3: What assumptions are we making?]

## Initial Test Approach
- [ ] [Testing strategy: manual, automated, both?]
- [ ] [Key scenarios to validate]
- [ ] [What existing tests cover this?]

## Open Questions
- [ ] [Question for Product team]
- [ ] [Question for Design team]
- [ ] [Question for Engineering team]
```

---

## Template 2: Business Unit Context

**When to use:** When documenting a new BU or updating existing BU understanding

**Location:** `documentation/business-units/[bu]/_bu-context.md`

```markdown
# [BU Name] - Business Unit Context

**Last Updated:** [Date]
**Owner:** [BU Name] BU
**QA Lead:** [Name]

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

## Core User Journeys
1. [User journey 1]
2. [User journey 2]
3. [User journey 3]

## Technical Context
- Primary tech stack: [e.g., React, Node.js, PostgreSQL]
- Key integrations: [e.g., Stripe, SendGrid, AWS S3]
- Deployment: [e.g., Kubernetes on AWS]

## Domain-Specific Terminology
| Term | Definition | Example |
|------|------------|---------|
| [Term 1] | [Definition] | [Example usage] |

## Notes for QA Team
[Any specific context QAs should know when testing this BU's features]
```

---

## Template 3: Quality Flavour

**When to use:** Defining what "good quality" means for a specific BU

**Location:** `documentation/business-units/[bu]/_quality-flavour.md`

```markdown
# [BU Name] - Quality Flavour

**What "Good Quality" Means for [BU Name]**

## Core Quality Philosophy
[1-2 paragraphs: What makes quality different in this BU?]

## Quality Dimensions

### 1. [Dimension 1: e.g., Financial Correctness]
**What it means:** [Definition]
**Why it matters:** [Business impact]
**How we measure it:** [Metric or signal]
**Example:** [Concrete scenario]

### 2. [Dimension 2: e.g., Reliability]
**What it means:** [Definition]
**Why it matters:** [Business impact]
**How we measure it:** [Metric or signal]
**Example:** [Concrete scenario]

### 3. [Dimension 3: e.g., Compliance]
**What it means:** [Definition]
**Why it matters:** [Business impact]
**How we measure it:** [Metric or signal]
**Example:** [Concrete scenario]

## Quality Gates Specific to This BU
- [ ] [Gate 1: e.g., "All payment flows tested with test credit cards"]
- [ ] [Gate 2: e.g., "Security review passed"]

## Known Risk Areas
- [Risk area 1: e.g., "Third-party API downtime"]
- [Risk area 2: e.g., "Complex state machine edge cases"]
```

---

## Template 4: OKR Alignment

**When to use:** Quarterly, to align quality work with business objectives

**Location:** `documentation/business-units/[bu]/_okr-alignment.md`

```markdown
# [BU Name] - OKR Alignment

**Period:** [Q1 2026]
**Last Updated:** [Date]

## [BU Name] OKRs

### O1: [Objective 1]

**Key Results:**
- KR1: [Measurable key result]
- KR2: [Measurable key result]
- KR3: [Measurable key result]

**How QA Contributes:**
- [QA contribution to KR1]
- [QA contribution to KR2]

### O2: [Objective 2]

**Key Results:**
- KR1: [Measurable key result]
- KR2: [Measurable key result]

**How QA Contributes:**
- [QA contribution to KR1]
- [QA contribution to KR2]

## Quarterly Quality Priorities

**Priority 1: [e.g., Fee Calculation Accuracy]**
- Supports KR: [Which key result?]
- Target: [Specific quality target]
- Status: [On track / At risk / Blocked]

**Priority 2: [e.g., Payment Reliability]**
- Supports KR: [Which key result?]
- Target: [Specific quality target]
- Status: [On track / At risk / Blocked]
```

---

## Template 5: Risk Analysis

**When to use:** For any medium-to-high complexity feature

**Location:** `documentation/business-units/[bu]/quality-scopes/[feature]/risk-hypotheses/risk-analysis.md`

```markdown
# [Feature Name] - Risk Analysis

**Created:** [Date]
**Last Updated:** [Date]

## Risk [Number]: [Risk Title]

### Description
[What could go wrong?]

### Severity: [Critical P0 / High P1 / Medium P2 / Low P3]
- **Financial Impact:** [Revenue, cost, or monetary risk]
- **Customer Impact:** [User experience or trust impact]
- **Operational Impact:** [Team burden, manual work]

### Likelihood: [High >50% / Medium 10-50% / Low <10%]
- **Why [Likelihood]:** [Reasoning for likelihood assessment]
- **Historical Data:** [Any past occurrences of similar issues]

### Root Causes (Hypothesized)
1. **[Root Cause 1]**
   - **Scenario:** [Specific situation where this manifests]
   - **Example:** [Concrete example]
   - **Impact:** [What happens if this occurs]

### Test Scenarios Required
| Scenario | Input | Expected Output | Priority |
|----------|-------|-----------------|----------|
| [Scenario 1] | [Input conditions] | [Expected behavior] | P0/P1/P2 |

### Mitigation Strategy
**Pre-Development:**
- [ ] [Mitigation action 1]

**Development:**
- [ ] [Mitigation action 2]

**Pre-Production:**
- [ ] [Mitigation action 3]

**Production:**
- [ ] [Mitigation action 4]

### Rollback Plan
[How to revert if this risk materializes in production]
```

---

## Template 6: Learning Notes (Post-Sprint)

**When to use:** After every sprint, capture what you learned

**Location:** `documentation/business-units/[bu]/quality-scopes/[feature]/learning-notes/sprint-[number].md`

```markdown
# Learning Notes - Sprint [Number]

**Sprint:** [Sprint Number]
**Date:** [Sprint End Date]
**QA:** [Your Name]

## What We Built
[Brief: what was delivered this sprint?]

## What Went Well (Quality Perspective)
- [Thing 1 that worked well]
- [Thing 2 that worked well]

## What Surprised Us
- [Surprise 1: unexpected issue, edge case, or insight]
- [Surprise 2: ...]

## Risks That Materialized
- [Risk we predicted that actually happened]
  - How we handled it: [Solution]

## Risks That Didn't Materialize
- [Risk we worried about that turned out to be non-issue]
  - Why it didn't happen: [Explanation - was our assumption wrong?]

## What We'd Do Differently Next Time
- [Learning 1: process improvement]
- [Learning 2: testing approach change]

## Pattern Recognized
**Pattern:** [Is this learning specific to this feature, or does it apply elsewhere?]
**Applies to:** [Other features, other BUs, or company-wide?]

## Action Items for Next Sprint
- [ ] [Action 1]
- [ ] [Action 2]

## Metrics (If Applicable)
- Test scenarios executed: [Number]
- Bugs found: [Number (P0/P1/P2/P3 breakdown)]
- Test coverage: [Percentage or scope]
- Time to validate: [Hours]
```

---

## Template 7: Validation Notes (During Testing)

**When to use:** While actively testing a feature

**Location:** `documentation/business-units/[bu]/quality-scopes/[feature]/validation-notes/test-session-[date].md`

```markdown
# Validation Notes - [Date]

**Feature:** [Feature Name]
**Tester:** [Your Name]
**Environment:** [Dev / Staging / Production]
**Build:** [Version or commit hash]

## Test Scenarios Executed

### Scenario 1: [Scenario Name]
- **Status:** ✅ Pass / ❌ Fail / ⚠️ Partial
- **Steps:**
  1. [Step 1]
  2. [Step 2]
  3. [Step 3]
- **Expected Result:** [What should happen]
- **Actual Result:** [What actually happened]
- **Notes:** [Any observations, edge cases, or follow-ups]
- **Bug Filed:** [Link to bug ticket, if applicable]

### Scenario 2: [Scenario Name]
...

## Edge Cases Discovered
- [Edge case 1: Description + How to reproduce]
- [Edge case 2: Description + How to reproduce]

## Blockers / Issues
- [Blocker 1: What's preventing further testing?]

## Test Coverage Summary
- ✅ [Area 1: Fully tested]
- ⚠️ [Area 2: Partially tested - reason]
- ❌ [Area 3: Not tested - why]

## Risk Assessment
- **High Risk Areas:** [What still concerns you?]
- **Confidence Level:** [High / Medium / Low]
- **Release Recommendation:** [Approve / Approve with caveats / Block]

## Next Steps
- [ ] [Follow-up action 1]
- [ ] [Follow-up action 2]
```

---

## Template 8: Monthly Quality Summary

**When to use:** End of each month, for leadership visibility

**Location:** `quality-tracking/monthly-summaries/[YYYY-MM].md`

```markdown
# Monthly Quality Summary - [Month Year]

**Period:** [Month 1 - Month 30, Year]
**Prepared by:** QA Team

## Executive Summary
[2-3 sentences: Overall quality status, key wins, key concerns]

## Quality Metrics

| Metric | Target | Actual | Status | Trend |
|--------|--------|--------|--------|-------|
| Payment Success Rate | >95% | 94.2% | 🟡 Yellow | ⬆️ Up from 93% |
| Fee Calculation Accuracy | 100% | 99.98% | 🟡 Yellow | ⬆️ Up from 99.95% |
| Production Incidents (P0) | 0 | 1 | 🔴 Red | ➡️ Same as last month |

## Key Accomplishments
- [Accomplishment 1: e.g., "Launched dynamic fee engine with zero production issues"]
- [Accomplishment 2: e.g., "Reduced late-stage bugs by 30%"]

## Quality Risks & Challenges
- [Risk 1: e.g., "Multi-currency rounding logic needs finance validation"]
- [Risk 2: e.g., "Test automation coverage gaps in payment flows"]

## Learnings & Patterns
- [Learning 1: e.g., "Early QA involvement reduced rework by 40%"]
- [Learning 2: e.g., "AI-assisted test ideation saved 3 hours per feature"]

## Next Month Focus
- [Priority 1: e.g., "Achieve 97% payment success rate"]
- [Priority 2: e.g., "Close fee calculation accuracy gap to 100%"]

## Blockers Escalated
- [Blocker 1: e.g., "Need finance team availability for validation sessions"]
```

---

## Quick Reference: Which Template When?

| Situation | Template to Use | Where to Put It |
|-----------|-----------------|-----------------|
| Starting a new feature | QA Context Brief | Quality Scope folder |
| Documenting a new BU | BU Context | `business-units/[bu]/_bu-context.md` |
| Defining quality for BU | Quality Flavour | `business-units/[bu]/_quality-flavour.md` |
| Quarterly planning | OKR Alignment | `business-units/[bu]/_okr-alignment.md` |
| Analyzing risks | Risk Analysis | Quality Scope `risk-hypotheses/` |
| After testing | Validation Notes | Quality Scope `validation-notes/` |
| After sprint | Learning Notes | Quality Scope `learning-notes/` |
| End of month | Monthly Summary | `quality-tracking/monthly-summaries/` |

---

**Remember:** Templates are starting points, not rigid requirements. Adapt them to fit your needs.

**The goal:** Make your thinking visible and shareable, not create perfect documentation.

---

**Next:** See [15-faqs.md](./15-faqs.md) for common questions about using these templates.
