# Repository Structure - Quality Intelligence

## The Big Picture

The `quality-intelligence-repo` (Project Kepler) is our **shared quality brain**.

- It stores **context**, not execution
- It captures **why** we test, not only **what** we test
- It turns **individual QA experience** into **shared knowledge**

**If something is not written here, it is easy to forget, repeat, or relearn the hard way.**

---

## Folder Structure

```
project-kepler/
├── documentation/
│   ├── business-units/                # BU-level quality context
│   │   ├── network/
│   │   │   ├── _bu-context.md        # What this BU does
│   │   │   ├── _okr-alignment.md     # What success means
│   │   │   ├── _quality-flavour.md   # What "good quality" looks like here
│   │   │   └── quality-scopes/        # Feature/epic-level context
│   │   │       ├── backend-microservices/
│   │   │       ├── design-intent/
│   │   │       ├── existing-tests-context/
│   │   │       ├── risk-hypotheses/
│   │   │       ├── validation-notes/
│   │   │       └── learning-notes/
│   │   ├── revenue-and-growth/
│   │   │   ├── _bu-context.md
│   │   │   ├── _okr-alignment.md
│   │   │   ├── _quality-flavour.md
│   │   │   └── quality-scopes/
│   │   └── pivot/
│   │       ├── _bu-context.md
│   │       ├── _okr-alignment.md
│   │       ├── _quality-flavour.md
│   │       └── quality-scopes/
│   └── how-it-works/                  # Implementation guides (you are here!)
├── quality-tracking/
│   ├── cross-bu-patterns/             # Patterns across multiple BUs
│   │   └── cross-bu-patterns.md
│   └── monthly-summaries/             # High-level quality summaries
│       └── monthly-summaries.md
├── benchmarks/
│   └── quality-standards/             # Shared quality benchmarks
│       └── quality-standards.md
├── checklists/
│   └── checklist-before-run.md        # Pre-analysis/testing checklist
├── automation-sources/                # References to automation projects
│   └── [CI, Cypress, Flutter, Playwright]
├── boards/
│   ├── index.html                     # Quality Learning Board (visualization)
│   └── data.js                        # Board data
├── CLAUDE.md                          # Project memory (GM Quality perspective)
├── README.md                          # Project overview
└── PRD - Quality Intelligence.pdf     # Complete product requirements
```

---

## What Goes Where

### 📁 `documentation/business-units/`

**Purpose:** Domain-level quality context for each Business Unit

**What to put here:**
- `_bu-context.md` - What this BU does, its role, responsibilities
- `_okr-alignment.md` - OKRs, success criteria, business goals
- `_quality-flavour.md` - What "good quality" means for this BU specifically

**Why it matters:**
Quality risks come from business goals and constraints, not just test cases. Understanding the BU context helps QA make better quality decisions.

**Example:**
- Network BU: Quality might focus on reliability, uptime, data consistency
- Revenue BU: Quality might focus on transaction accuracy, compliance, financial correctness
- Pivot BU: Quality might focus on experimentation speed, rollback capability, isolated risk

---

### 📁 `documentation/business-units/[BU]/quality-scopes/`

**Purpose:** Change-level deep context (nested under each BU)

**What to put here:**
Quality Scopes represent anything that needs deep context:
- Feature development
- Epic implementation
- Refactoring initiatives
- Risky changes
- Long-running projects

**Inside each Quality Scope folder:**
- `backend-microservices/` - Technical architecture context
- `design-intent/` - UI/UX design intentions and rationale
- `existing-tests-context/` - What tests already exist and why
- `risk-hypotheses/` - Predicted risks and assumptions
- `validation-notes/` - QA validation approach and results
- `learning-notes/` - What we learned (feeds next sprint)

**Why it matters:**
Test cases already exist—but their **intent** is often lost. Quality Scopes capture the *why* behind testing decisions.

**When to create a Quality Scope:**
- Starting a new feature with complexity
- Epic that spans multiple sprints
- Refactor that changes existing behavior
- Change identified as high-risk
- Anything you'll need to remember context for later

---

### 📁 `quality-tracking/`

**Purpose:** Cross-BU patterns and leadership summaries

**What to put here:**
- `cross-bu-patterns/` - Quality patterns that appear across multiple BUs
  - Repeated risk types
  - Common testing approaches
  - Shared pain points or learnings

- `monthly-summaries/` - High-level quality insights for stakeholders
  - Quality trends
  - Key learnings
  - Progress indicators
  - Areas needing attention

**Why it matters:**
Individual BU learning is valuable; cross-BU patterns are strategic. Monthly summaries make progress visible to leadership.

---

### 📁 `benchmarks/`

**Purpose:** Shared quality standards and benchmarks

**What to put here:**
- Quality gates that apply across all BUs
- Acceptance criteria templates
- Definition of "done" from quality perspective
- Quality metrics thresholds
- Best practices consensus

**Why it matters:**
Without benchmarks, "quality" is subjective. Shared standards enable consistent quality decisions.

---

### 📁 `checklists/`

**Purpose:** Pre-flight checks before testing or analysis

**What to put here:**
- `checklist-before-run.md` - Questions to answer before running tests or using AI

**Why it matters:**
Testing or AI analysis without context creates noise. The checklist ensures context is clear before execution.

**Example checklist items:**
- [ ] Do I understand the product vision for this feature?
- [ ] Have I read the design intent?
- [ ] Do I know what quality risks matter most here?
- [ ] Are my assumptions documented?
- [ ] Have I reviewed existing test context?

---

### 📁 `automation-sources/`

**Purpose:** References to automation projects (not execution)

**What to put here:**
- Links to CI/CD pipelines
- References to Cypress, Flutter, Playwright projects
- Documentation pointers

**What NOT to put here:**
- Actual test execution code (that lives in automation repos)
- Test run results (that lives in test management tools)

**Why it matters:**
Quality Intelligence is about **context and decisions**, not test execution. We reference automation, not duplicate it.

---

### 📁 `boards/`

**Purpose:** Quality Learning Board for visualization and storytelling

**What to put here:**
- `index.html` - Interactive board showing quality insights
- `data.js` - Data backing the visualizations

**What NOT to put here:**
- Raw test data
- Detailed bug reports (those live in issue trackers)

**Why it matters:**
The board is for **trends, insights, and communication**—not raw data. It helps tell the quality story to stakeholders.

---

### 📄 Root Files

**CLAUDE.md:**
- Full project memory
- GM Quality perspective
- Strategic context
- Progress tracking
- Risk management

**README.md:**
- Quick project overview
- Getting started guide
- FAQ for team members

**PRD - Quality Intelligence.pdf:**
- Complete product requirements
- Background and vision
- Implementation details

---

## Navigation Tips

### As a QA Team Member:

**Starting your day:**
1. Go to `documentation/business-units/[your-BU]/`
2. Read `_bu-context.md` if you haven't recently
3. Check `quality-scopes/` for your current features

**Before grooming:**
1. Review product vision and design intent
2. Create or update Quality Scope for the feature
3. Note initial risk hypotheses

**After sprint:**
1. Add learning notes to your Quality Scope
2. Update `quality-tracking/cross-bu-patterns/` if you found a pattern
3. Contribute to `boards/` data for monthly summary

### As a Stakeholder (Product/Design/Engineering):

**Want to understand quality context for a feature?**
→ Check `documentation/business-units/[BU]/quality-scopes/[feature]/`

**Want to see overall quality trends?**
→ Check `boards/index.html` or `quality-tracking/monthly-summaries/`

**Want to know quality standards?**
→ Check `benchmarks/quality-standards/`

---

## Key Principles

### 1. Hierarchical Context
- Business Unit level = domain context (changes slowly)
- Quality Scope level = change context (per feature/epic)
- This prevents duplication and makes updates manageable

### 2. Separation of Concerns
- **Context** lives in `documentation/`
- **Learning** lives in `quality-tracking/`
- **Standards** live in `benchmarks/`
- **Execution** lives elsewhere (automation repos, test tools)

### 3. Write for Future You
When you document, imagine yourself 3 months from now trying to understand why you made certain quality decisions. What would you want to know?

---

## What If I'm Still Confused?

**Start here:**
1. Go to `documentation/business-units/`
2. Pick the BU you work with most
3. Read the three context files (`_bu-context`, `_okr-alignment`, `_quality-flavour`)
4. That's it for today

**Tomorrow:**
1. Create a new folder under that BU's `quality-scopes/` for a feature you're working on
2. Write 3 bullet points about what quality risks you're thinking about
3. That's it

**By Week 2:**
You'll naturally know where things go because you've been using the structure.

---

## Next Step

Read [02-your-first-contribution.md](./02-your-first-contribution.md) to make your first entry in the repository.

---

**Remember:** This structure serves us—if it doesn't make sense, we adapt it. The goal is shared quality knowledge, not perfect folder organization.
