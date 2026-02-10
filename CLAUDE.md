# Project Kepler - Quality Intelligence
## Operating Manual for General Manager, Quality

**Last Updated:** 2026-02-10
**Project Domain:** Quality Assurance Transformation
**Strategic Initiative:** Quality Intelligence Operating Model
**Role:** This document defines how GM Quality operates and maintains this system

---

## Executive Context

Project Kepler implements **Quality Intelligence** - a QA operating model that translates product vision, design intent, and engineering signals into early quality decisions.

### The Vision
QA evolves from reactive testing to proactive quality enablement. We build a system where QA becomes a cross-functional quality hub - guiding quality decisions before code is written, not just finding bugs after.

### Why This Matters Now
- Product complexity growing → Late-stage QA insufficient
- Quality issues discovered mid-sprint → Rework, inefficiency, poor UX
- Knowledge fragmented across individuals → Lost when people leave
- Reactive approach doesn't scale → Need preventive and predictive capabilities

### The Transformation Path

**Level 0 - Reactive Quality (Current State for Most Teams):**
- QA tests after development
- Knowledge in individual heads
- Manual processes, no pattern recognition

**Level 1 - Preventive Quality (Month 1 - Starting Point):**
- QA joins grooming actively (≥80% attendance)
- Context briefs before development
- Early risk discussions
- Simple quality gates
- Basic knowledge documentation

**Level 2 - Predictive Quality (Month 2):**
- Quality signals tracked (hotfixes, rework, late issues)
- Quality Learning Board maintained
- Manual AI thinking (ChatGPT/Claude for analysis)
- 15-30 min QA sharing per sprint
- Pattern recognition

**Level 3 - Assisted Quality (Month 3):**
- Consistent AI usage (risk analysis, test ideation, documentation)
- Internal prompting guidelines
- Human review of all AI outputs
- Productivity improvements visible

**Level 4 - Autonomous Quality (Future - Not Current Focus):**
- AI integrated into workflows
- Automated quality signals
- Risk recommendation systems

---

## GM Quality Role: Strategic Priorities

### Priority 1: Quality as Early Intelligence, Not Late Validation

**Old Model:** QA validates after development → Finds issues too late
**New Model:** QA informs before development → Prevents issues early

**Your Actions:**
- Ensure QA Context Briefs created before grooming (100% of P0/P1 features)
- Review risk hypotheses - are they specific and actionable?
- Monitor grooming attendance (target: ≥80%)
- Celebrate early risk identification (not just bug finding)

**Success Metric:** Rework due to late quality issues decreasing (track monthly)

---

### Priority 2: Knowledge as Competitive Advantage

**Problem:** Quality knowledge lives in individual heads → Lost when people leave
**Solution:** Shared Quality Intelligence repository → Institutional memory

**Your Actions:**
- Review Quality Scopes quarterly - are they capturing thinking, not just test cases?
- Identify cross-BU patterns - what learnings apply broadly?
- Ensure learning notes written after every sprint (minimum 1 per sprint)
- Make knowledge visible to stakeholders (monthly quality summaries)

**Success Metric:** New QA onboarding time reduced by 50% (they reference existing context)

---

### Priority 3: AI as Productivity Multiplier, Not Replacement

**Principle:** AI enhances QA judgment, never replaces it

**Your Role:**
- Set guardrails: "AI suggests, QA decides" is non-negotiable
- Review AI-assisted work - is human judgment applied?
- Create prompting guidelines for each BU (what context must AI receive?)
- Ensure team doesn't copy-paste AI outputs blindly

**Success Metric:** Productivity improvement (time from feature brief to risk assessment decreasing) while maintaining quality judgment

---

### Priority 4: Cross-Functional Quality Culture

**Vision:** QA as quality hub connecting Product, Design, and Engineering

**Your Actions:**
- Partner with Product - when do they brief QA? (target: before PRD finalized)
- Partner with Design - is design intent documented for QA? (target: every major feature)
- Partner with Engineering - are quality needs discussed during architecture? (target: every high-risk change)
- Reduce Program Triage dependency - are issues prevented vs triaged?

**Success Metric:** Stakeholder feedback - "QA helps us make better decisions early"

---

## Repository Structure (Your Operating Environment)

This is your quality brain. Everything lives here.

```
project-kepler/
├── documentation/
│   ├── business-units/                # Domain-level quality context
│   │   ├── revenue-and-growth/        # Example: Payment processing BU
│   │   │   ├── _bu-context.md         # What this BU does, tech stack, terminology
│   │   │   ├── _quality-flavour.md    # What "good quality" means here
│   │   │   ├── _okr-alignment.md      # How quality supports business goals
│   │   │   └── quality-scopes/        # Feature-level deep context
│   │   │       └── dynamic-fee-engine/
│   │   │           ├── README.md      # Feature overview, vision, risks
│   │   │           ├── risk-hypotheses/
│   │   │           ├── existing-tests-context/
│   │   │           ├── design-intent/
│   │   │           ├── validation-notes/
│   │   │           └── learning-notes/
│   │   ├── network/                   # (Ready to scaffold)
│   │   └── pivot/                     # (Ready to scaffold)
│   └── how-it-works/                  # Implementation guides (7-step workflow)
├── quality-tracking/
│   ├── cross-bu-patterns/             # Patterns across multiple BUs
│   └── monthly-summaries/             # High-level quality summaries
├── benchmarks/
│   └── quality-standards/             # Shared quality benchmarks
├── checklists/
│   └── checklist-before-run.md        # Pre-testing/analysis checklist
├── automation-sources/                # References to automation projects
├── boards/                            # Quality Learning Board (visualization)
├── CLAUDE.md                          # This file (your operating manual)
├── README.md                          # Project overview
├── STATUS.md                          # Implementation tracking
└── PRD - Quality Intelligence.pdf     # Complete product requirements
```

---

## Operating Principles (Your Standards)

### 1. Consistency Over Completeness

**Rule:** Better to document 3 insights every sprint than perfect documentation once and burn out.

**Your Job:**
- Enforce: Minimum 1 learning note per sprint per QA
- Reject: Perfectionism (2-hour documentation sessions)
- Celebrate: Consistent small contributions

**Example:**
- ✅ Good: "3 bullet points about rounding errors, captured in 10 minutes"
- ❌ Bad: "Waited until I had 'complete' analysis, never documented it"

---

### 2. Context Before Execution

**Rule:** Never test or use AI without clear context. Checklist is mandatory.

**Your Job:**
- Enforce: [Checklist Before Run](./checklists/checklist-before-run.md) used before testing P0/P1 features
- Monitor: Are QAs testing on autopilot or with context?
- Coach: When bugs missed, ask "Did you have full context?"

**Red Flag:** QA says "I don't know what this feature is for" while testing → Context gap

---

### 3. Make Thinking Visible

**Rule:** Repository captures WHY (thinking), not just WHAT (test cases).

**Your Job:**
- Review: Do Quality Scopes explain reasoning, not just results?
- Example: "Tested fee calculation because 0.01% error = Rp 1M monthly loss" (not just "Tested fee calculation")
- Push back: "Why did you test this?" should have documented answer

---

### 4. Learn Forward, Not Backward

**Rule:** Capture learning to prevent future issues, not to blame past decisions.

**Your Job:**
- Frame: "What did we learn?" not "Who messed up?"
- Quality Learning Board shows patterns (not individual bug reports)
- Retrospectives feed into cross-BU patterns

---

### 5. Human Judgment is Non-Negotiable

**Rule:** AI assists, QA decides. Always.

**Your Job:**
- Enforce: All AI outputs reviewed by human before use
- Coach: "What did AI suggest? What did you validate? What did you decide differently?"
- Reject: Copy-paste AI test cases without QA review

---

## Success Indicators (How You Measure Progress)

### Leading Indicators (Process Health)
**Track these monthly:**

| Metric | Target | Why It Matters |
|--------|--------|----------------|
| QA grooming attendance | ≥80% | Early involvement prevents late rework |
| Context briefs created | 100% of P0/P1 features | Context before execution |
| Quality signals updated | Weekly | Pattern recognition requires data |
| Learning insights documented | ≥3 per sprint | Knowledge compounds |
| AI usage with human review | 100% reviewed | Judgment applied |

### Lagging Indicators (Business Impact)
**Track these monthly:**

| Metric | Target | Why It Matters |
|--------|--------|----------------|
| Feedback loop speed | Decreasing | Faster QA feedback = less rework |
| Late-stage issues | Decreasing | Early prevention working |
| Rework from misalignment | Decreasing | Context clarity improving |
| Sprint delivery stability | Increasing | Quality predictability |
| Program Triage dependency | Decreasing | Prevention > triage |

---

## Risk Management (Your Mitigation Strategies)

### Risk 1: Documentation Becomes Bureaucracy
**Symptom:** QAs see this as "more work" not "better work"

**Your Mitigation:**
- Time targets: 15-30 min per sprint (if more, simplify)
- Templates provided (copy-paste-adapt, not blank page)
- Quick wins: Prove value fast (catch 1 major bug early → celebrate)

---

### Risk 2: Adoption Inconsistency
**Symptom:** Only senior QAs use it, juniors ignore it

**Your Mitigation:**
- Step-by-step guides remove ambiguity (7-step workflow is mandatory)
- Pair programming: Senior + junior document together
- Team workshop (Week 2-3): Hands-on training

---

### Risk 3: Examples Become Dogma
**Symptom:** "Revenue BU did it this way, so we must too"

**Your Mitigation:**
- Explicitly state: "Revenue BU is example, not mandate"
- Each BU defines its own quality flavour
- Templates flexible (adapt to context)

---

### Risk 4: AI Becomes Crutch
**Symptom:** QAs copy-paste AI output without review

**Your Mitigation:**
- Human review mandatory (enforce this)
- Prompting guidelines include business context AI doesn't know
- Spot-check: "Explain why you chose this test scenario" (should be QA reasoning, not "AI said so")

---

### Risk 5: Knowledge Becomes Stale
**Symptom:** BU contexts outdated, Quality Scopes reference old features

**Your Mitigation:**
- Quarterly BU context review (update OKRs, tech stack, known risks)
- Archive old Quality Scopes (move to `/archive/` after 6 months)
- Monthly summary forces reflection (what's changed?)

---

## Role Standard: How to Operate This System

### Daily Operations

**Morning:**
- Review: Which features entering testing today? Do they have Quality Scopes?
- Check: Are QAs blocked? (Missing context, unclear requirements, stakeholder unavailable)

**During Sprint:**
- Monitor: Grooming attendance, context brief completion
- Coach: When QAs ask questions, point to relevant documentation first
- Escalate: Quality blockers to Product/Engineering (document in Quality Scope)

**End of Sprint:**
- Review: Learning notes captured? (Minimum 1 per QA)
- Identify: Cross-BU patterns? (Add to quality-tracking/)
- Celebrate: Wins (bug prevented early, knowledge reused, fast onboarding)

---

### Weekly Operations

**Monday:**
- Review sprint goals - which features need Quality Scopes?
- Assign: Each QA owns 1-2 Quality Scopes this sprint

**Wednesday:**
- Mid-sprint check: Quality Scopes being updated during testing?
- Coaching: Pair with QAs struggling with documentation

**Friday:**
- Sprint retro: What quality learnings? (Feed into learning notes)
- Next sprint prep: Context briefs for upcoming features

---

### Monthly Operations

**Week 1:**
- Collect quality signals (hotfixes, late issues, rework, incidents)
- Aggregate learning notes from all Quality Scopes

**Week 2:**
- Write monthly quality summary (trends, insights, progress)
- Update Quality Learning Board visualization

**Week 3:**
- Share monthly summary with stakeholders (email + board link)
- Gather feedback: What's working? What's confusing?

**Week 4:**
- Review BU contexts (any updates needed?)
- Plan next month focus (which quality priority?)

---

### Quarterly Operations

**BU Context Review:**
- Update OKR alignment (new quarter = new objectives)
- Tech stack changes? (New integrations, migrations)
- Quality flavour still accurate? (New quality dimensions discovered?)

**Knowledge Pruning:**
- Archive Quality Scopes >6 months old (move to `/archive/`)
- Identify evergreen patterns (move to cross-BU patterns)
- Update benchmarks based on learnings

**Team Retrospective:**
- What's working well? (Process, tools, collaboration)
- What's not working? (Documentation burden, ambiguity, adoption gaps)
- What to change next quarter?

---

## How to Update This System

### When BU Context Changes

**Trigger:** New BU created, major tech stack change, OKRs shift

**Action:**
1. Navigate to `documentation/business-units/[bu]/`
2. Update `_bu-context.md` (tech stack, products, terminology)
3. Update `_quality-flavour.md` (if quality dimensions changed)
4. Update `_okr-alignment.md` (quarterly)
5. Document change in file: "Updated [date]: [what changed] - [your name]"

---

### When Quality Scope Created

**Trigger:** New P0/P1 feature starting development

**Action:**
1. QA creates folder: `documentation/business-units/[bu]/quality-scopes/[feature-name]/`
2. QA writes README.md (why exists, product vision, design intent, technical context, risks)
3. QA documents risk-hypotheses/ (detailed risk analysis)
4. QA notes existing-tests-context/ (what tests exist, what's needed)
5. You review: Is context sufficient? Are risks specific?

---

### When Sprint Ends

**Trigger:** Sprint retrospective

**Action:**
1. Each QA writes learning notes in their Quality Scope: `learning-notes/sprint-[number].md`
2. You review: Any cross-BU patterns? (Add to `quality-tracking/cross-bu-patterns/`)
3. Update monthly progress tracking

---

### When Pattern Discovered

**Trigger:** Same issue/learning appears across multiple features or BUs

**Action:**
1. Create/update: `quality-tracking/cross-bu-patterns/[pattern-name].md`
2. Document: What's the pattern? Where observed? Root cause? Solution? Which BUs affected?
3. Share with teams: Link from BU contexts to relevant patterns
4. Update quality standards if pattern becomes best practice

---

### When AI Usage Evolves (Level 2-3)

**Trigger:** Team starts using AI consistently

**Action:**
1. Create prompting guidelines: `documentation/business-units/[bu]/prompting-guidelines.md`
2. Document: What context must AI receive? What review checklist? What examples of good vs bad prompts?
3. Enforce human review: Spot-check AI outputs, coach QAs
4. Update monthly: How is AI being used? What productivity gains? What risks observed?

---

## Stakeholder Communication (Your Storytelling)

### To QA Team
**Frequency:** Weekly (team meetings), Daily (Slack/Teams)

**Message:**
- Celebrate: Quality wins (bug prevented early, knowledge reused)
- Coach: How to document better, how to use AI effectively
- Remove blockers: Missing context, stakeholder unavailability

---

### To Product/Design/Engineering
**Frequency:** Monthly (cross-functional sync)

**Message:**
- Share: Quality insights from Quality Learning Board
- Request: Earlier involvement (context briefs before development starts)
- Collaborate: Quality needs during architecture discussions

---

### To Leadership
**Frequency:** Monthly (quality summary), Quarterly (strategic review)

**Message:**
- Trends: Quality metrics (success rates, late issues, rework)
- Impact: Revenue protected, delivery improved, efficiency gained
- Investment: What's needed? (Time, tools, headcount)

**Format:** 1-page summary + link to Quality Learning Board

---

## Decision Framework (When You Need to Decide)

### Question: Should we create a Quality Scope for this feature?

**Create if:**
- P0 or P1 priority
- Touches money, auth, or compliance
- Multi-sprint epic
- Refactor changing behavior
- Identified as high-risk

**Skip if:**
- Trivial (typo, small UI tweak)
- Internal tool (low impact)
- Short experiment (<1 week)

---

### Question: Is this a cross-BU pattern?

**Yes if:**
- Observed in 2+ different features or BUs
- Same root cause or solution
- Learning applies broadly

**No if:**
- Specific to one feature's unique context
- One-off edge case

**Document as pattern when:** 2nd occurrence (first time = coincidence, second = pattern)

---

### Question: Should we use AI for this task?

**Use AI for:**
- Context analysis (summarize long docs)
- Risk brainstorming (generate edge case ideas)
- Test ideation (suggest scenarios)
- Documentation speed (draft learning notes)

**Don't use AI for:**
- Final quality decisions (ship/block)
- Risk prioritization (requires business context)
- Stakeholder communication (human touch needed)

**Always:** Human reviews AI output before use

---

### Question: How much time should QAs spend documenting?

**Target:** 15-30 minutes per sprint per QA

**If more:** Simplify (templates too complex? Expectations too high?)

**If less:** Check quality (are insights meaningful or superficial?)

**Balance:** Enough to capture learning, not so much it feels like overhead

---

## Next Steps for Launching Quality Intelligence

### Week 1: Foundation
- [ ] Share README.md with QA team
- [ ] Walk through Revenue BU example (team meeting)
- [ ] Assign: Each QA picks one feature to document this sprint
- [ ] Tool setup: ChatGPT/Claude access (for Level 2)

### Week 2-3: Adoption
- [ ] Team workshop: "How to use Quality Intelligence" (hands-on)
- [ ] Pair programming: Senior + junior document together
- [ ] Monitor: Are QAs using step-by-step guides? (03-09)

### Week 4: Feedback
- [ ] Retrospective: What's working? What's confusing?
- [ ] Adjust: Simplify if documentation feels heavy
- [ ] Celebrate: First quality win (bug prevented early, knowledge reused)

### Month 2: Expansion
- [ ] Document 2 more BUs (Network, Pivot)
- [ ] Quality signals tracked (hotfixes, rework, late issues)
- [ ] Quality Learning Board visualization started

### Month 3: AI Integration
- [ ] Prompting guidelines created per BU
- [ ] AI usage consistent across team
- [ ] Productivity improvements visible

### Month 4: Standardization
- [ ] QA Playbook published
- [ ] Quality benchmarks defined
- [ ] Monthly quality summaries routine

---

## Final Reminder: This Is Your Role Standard

**CLAUDE.md is not a history book. It's your operating manual.**

When you (or another GM Quality) operates this system:
- Read this file quarterly (refresh on standards)
- Update when operating model changes (new level, new priority)
- Keep it practical (what to do, not what was discussed)

**This document defines:**
- ✅ What GM Quality does (strategic priorities, daily/weekly/monthly operations)
- ✅ How to operate the system (update BU contexts, create Quality Scopes, track metrics)
- ✅ Standards and principles (consistency > completeness, context before execution)
- ✅ Decision frameworks (when to create Quality Scope, when to use AI)

**This document does NOT:**
- ❌ Document every conversation or decision history (that's STATUS.md)
- ❌ Explain how we built this (that's STATUS.md collaborative insights section)
- ❌ Provide step-by-step guides (that's documentation/how-it-works/ 03-09)

**Use this as your handbook. Update it as the role evolves. Keep it comprehensive and pragmatic.**

---

**Role:** General Manager, Quality
**Responsibility:** Operate and maintain Quality Intelligence system
**Standard:** This document defines how you do it

**Let's pioneer quality that drives business results.** 💰🚀

---

**Reference Documents:**
- [README.md](./README.md) - Project overview for all audiences
- [STATUS.md](./STATUS.md) - Implementation tracking and historical context
- [PRD - Quality Intelligence.pdf](./PRD%20-%20Quality%20Intelligence.pdf) - Complete vision and requirements
- [How It Works](./documentation/how-it-works/) - Step-by-step workflow guides (03-09)
- [Templates](./documentation/how-it-works/14-templates.md) - All documentation templates
