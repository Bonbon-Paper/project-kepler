# Revenue & Growth - OKR Alignment

**Period:** Q1 2026 (Jan - Mar 2026)
**Last Updated:** 2026-02-10

---

## Company-Level OKRs (Paper.id)

**Company Vision:** Become Southeast Asia's leading payment infrastructure platform

**Q1 2026 Company OKRs:**

### O1: Scale Transaction Volume to $50M GTV
- KR1: Achieve $15M monthly GTV by end of Q1 (currently $7.5M)
- KR2: Onboard 500 new merchants (currently 1,200 total)
- KR3: Launch in 2 new countries (Malaysia, Philippines)

### O2: Improve Platform Reliability to Enterprise Grade
- KR1: 99.9% uptime SLA for payment processing
- KR2: <2 second average payment completion time
- KR3: Zero critical production incidents

### O3: Achieve Profitability Milestone
- KR1: Reach $500K monthly revenue (currently $217K)
- KR2: Reduce CAC (Customer Acquisition Cost) by 30%
- KR3: Increase LTV:CAC ratio to 3:1

---

## Revenue BU OKRs (Derived from Company Goals)

### O1: Increase Platform Revenue by 30% QoQ

**Why this matters:**
Company profitability depends on our ability to maximize revenue per transaction while maintaining competitive pricing.

**Key Results:**

**KR1: Achieve $10M in Gross Transaction Volume (GTV)**
- **Current:** $7.5M monthly GTV (Dec 2025)
- **Target:** $10M monthly GTV (Mar 2026)
- **Strategy:** Enable more payment methods (reduce friction) + support high-volume merchants
- **Quality Impact:** More transactions = more edge cases. Quality must scale with volume.

**KR2: Maintain 2.9% Effective Transaction Fee Rate**
- **Current:** 2.87% effective rate (some revenue leakage from fee calculation bugs)
- **Target:** 2.90% effective rate (close the 0.03% gap = ~$2,250 monthly revenue recovery)
- **Strategy:** Fix fee calculation edge cases + implement dynamic pricing by merchant tier
- **Quality Impact:** Fee accuracy is non-negotiable. Every 0.01% = $1,000 monthly revenue.

**KR3: Launch 2 New Payment Methods (BNPL Expansion)**
- **Current:** Credit card, bank transfer, 4 e-wallets
- **Target:** Add Kredivo (BNPL) and Akulaku (BNPL)
- **Strategy:** Partner integrations + merchant adoption campaigns
- **Quality Impact:** New integrations = new failure modes. Must test extensively.

---

### O2: Improve Transaction Reliability and User Experience

**Why this matters:**
Payment friction = abandoned carts = lost revenue. Improving success rates directly impacts GTV.

**Key Results:**

**KR1: Achieve 97% Payment Success Rate (up from 94%)**
- **Current:** 94.2% success rate (5.8% failures costing ~$435K monthly in lost GTV)
- **Target:** 97% success rate (reduce failures to 3%)
- **Root causes of failures:**
  - 2.3% - Payment gateway timeouts
  - 1.5% - User errors (insufficient funds, expired cards)
  - 1.2% - Integration bugs (webhook handling, 3DS flow issues)
  - 0.8% - Other (network issues, browser compatibility)
- **Strategy:** Implement failover logic, improve error messaging, optimize 3DS flow
- **Quality Impact:** Need robust timeout handling, retry logic, idempotency testing

**KR2: Reduce Payment Friction (3-Click Checkout)**
- **Current:** Average 7 clicks from "Checkout" to "Payment Confirmed"
- **Target:** Reduce to 3 clicks (one-click for saved payment methods)
- **Strategy:** Saved payment methods, pre-filled forms, guest checkout optimization
- **Quality Impact:** Fast checkout = less testing time. Must balance speed vs thoroughness.

**KR3: Support 5 New Currencies**
- **Current:** IDR, USD, SGD
- **Target:** Add MYR, PHP, THB, VND, EUR
- **Strategy:** Multi-currency payment processing + dynamic FX rates
- **Quality Impact:** Currency conversion testing, rounding rules, FX rate accuracy

---

### O3: Ensure Financial Compliance and Security

**Why this matters:**
Non-compliance shuts down the platform. Security breaches destroy trust irreversibly.

**Key Results:**

**KR1: Maintain PCI-DSS Level 1 Certification**
- **Current:** Certified (last audit: Q4 2025, next: Q2 2026)
- **Target:** Pass Q2 audit with zero critical findings
- **Strategy:** Quarterly internal audits, automated compliance scanning, team training
- **Quality Impact:** Every payment flow must be PCI-DSS compliant. No shortcuts.

**KR2: Zero Financial Discrepancies in Audits**
- **Current:** 99.8% reconciliation match (0.2% gap = $15K monthly discrepancy under investigation)
- **Target:** 100% reconciliation match
- **Strategy:** Fix root cause of rounding errors, improve audit trail logging
- **Quality Impact:** Reconciliation tests required for every fee calculation change

**KR3: Implement Real-Time Fraud Detection**
- **Current:** Manual fraud review (post-transaction)
- **Target:** Automated fraud scoring for 100% of transactions (block suspicious activity)
- **Strategy:** Integrate Sift Science, ML-based fraud detection
- **Quality Impact:** False positives hurt UX. Must balance security vs friction.

---

## How QA Contributes to Each OKR

### Contributing to O1 (Increase Revenue by 30%):

**KR1: Achieve $10M GTV**
- **QA Role:** Ensure new payment methods work flawlessly (reduce friction = more transactions)
- **Quality Focus:** Integration testing for BNPL providers (Kredivo, Akulaku)
- **Success Metric:** <1% failure rate for new payment methods

**KR2: Maintain 2.9% Fee Rate**
- **QA Role:** Catch fee calculation bugs before they cause revenue leakage
- **Quality Focus:** Regression testing for dynamic pricing, promo codes, multi-currency fees
- **Success Metric:** Zero fee calculation bugs in production

**KR3: Launch 2 New Payment Methods**
- **QA Role:** Validate end-to-end flows for new payment methods
- **Quality Focus:** User journeys (select BNPL → approve loan → payment confirmed)
- **Success Metric:** New methods pass all quality gates before launch

---

### Contributing to O2 (Improve Reliability & UX):

**KR1: Achieve 97% Success Rate**
- **QA Role:** Identify and test fixes for timeout scenarios, retry logic, webhook failures
- **Quality Focus:** Load testing, timeout simulation, idempotency validation
- **Success Metric:** Payment failure rate <3% in production

**KR2: Reduce to 3-Click Checkout**
- **QA Role:** Ensure fast checkout doesn't compromise security or correctness
- **Quality Focus:** Saved payment method flows, guest checkout, autofill accuracy
- **Success Metric:** Zero payment errors from optimized checkout

**KR3: Support 5 New Currencies**
- **QA Role:** Test currency conversion accuracy, rounding, and display formatting
- **Quality Focus:** FX rate correctness, multi-currency invoicing, payout conversions
- **Success Metric:** 100% accuracy in currency calculations

---

### Contributing to O3 (Compliance & Security):

**KR1: Maintain PCI-DSS Certification**
- **QA Role:** Validate every payment flow against PCI-DSS checklist
- **Quality Focus:** Tokenization, TLS, no card data in logs, secure data transmission
- **Success Metric:** Zero PCI-DSS violations found in internal audits

**KR2: Zero Financial Discrepancies**
- **QA Role:** Test reconciliation logic, validate rounding rules, ensure audit trail completeness
- **Quality Focus:** Fee accuracy, transaction logging, reconciliation report generation
- **Success Metric:** 100% reconciliation match in QA environment before release

**KR3: Implement Fraud Detection**
- **QA Role:** Test fraud scoring accuracy, validate block logic, ensure no false positives hurt UX
- **Quality Focus:** Fraud rule testing, edge case scenarios (legit high-value transactions)
- **Success Metric:** <0.1% false positive rate (legit transactions blocked)

---

## Quarterly Quality Priorities Aligned to OKRs

### Q1 2026 QA Focus Areas:

**Priority 1: Fee Calculation Accuracy (supports KR: 2.9% fee rate)**
- Fix known fee calculation bugs (promo code + multi-currency edge case)
- Expand regression test suite for dynamic pricing
- Validate rounding rules with finance team
- Target: Zero fee bugs, 100% reconciliation match

**Priority 2: Payment Method Integration Testing (supports KR: launch 2 new methods)**
- End-to-end testing for Kredivo and Akulaku BNPL flows
- Timeout and retry scenarios for new providers
- Idempotency validation for webhooks
- Target: <1% failure rate for new methods at launch

**Priority 3: Transaction Reliability (supports KR: 97% success rate)**
- Load testing for peak volume (10x current traffic)
- Timeout simulation and failover testing
- Webhook replay and retry logic validation
- Target: Payment flows resilient to gateway downtime

**Priority 4: Multi-Currency Support (supports KR: 5 new currencies)**
- Currency conversion accuracy testing
- Rounding rule validation across currencies
- Invoice formatting for regional tax requirements
- Target: 100% accuracy for FX calculations

**Priority 5: PCI-DSS Compliance (supports KR: maintain certification)**
- Automated scanning for PCI violations (card data in logs)
- Security testing for tokenization flows
- Quarterly internal compliance audit
- Target: Zero critical findings in Q2 audit

---

## Trade-Offs and Quality Decisions

### Example Trade-Off: Speed vs Thoroughness

**Scenario:** Product wants to launch new payment method in 2 weeks to hit KR deadline.

**Quality Perspective:**
- **Risk:** Insufficient testing time = higher failure rate at launch
- **Impact:** Failed transactions hurt user trust + revenue loss
- **Decision Framework:**
  1. Can we test critical paths in 2 weeks? (Yes/No)
  2. Can we launch to 10% of users first? (Phased rollout)
  3. Can we rollback quickly if issues found? (Rollback plan required)
  4. What's the cost of delaying 1 week? (Missed KR vs quality risk)

**QA Recommendation:**
- Launch to 10% of users (limited blast radius)
- Monitor closely for 48 hours before full rollout
- Accept 1-week delay if critical issues found in testing

**Alignment with OKRs:**
- Supports KR: Launch 2 new payment methods (on-time)
- Doesn't compromise KR: 97% success rate (phased rollout reduces risk)

---

## Success Metrics Dashboard

| OKR | Key Result | QA Contribution | Target | Current | Status |
|-----|------------|-----------------|--------|---------|--------|
| O1 | $10M GTV | New payment method quality | <1% failure rate | TBD | 🟡 In Progress |
| O1 | 2.9% fee rate | Fee calculation accuracy | 100% accuracy | 99.97% | 🟡 Gap exists |
| O2 | 97% success rate | Reliability testing | <3% failures | 5.8% | 🔴 Needs work |
| O2 | 5 new currencies | Currency conversion accuracy | 100% accuracy | N/A | ⚪ Not started |
| O3 | PCI-DSS cert | Compliance validation | Zero violations | 0 critical | ✅ On track |
| O3 | Zero discrepancies | Reconciliation testing | 100% match | 99.8% | 🟡 Gap exists |

---

## Monthly Check-In: Quality Progress vs OKRs

**End of each month, QA reports:**

1. **Fee Calculation Accuracy:** [Percentage] - Target: 100%
2. **Payment Success Rate:** [Percentage] - Target: 97%
3. **New Payment Methods Status:** [On track / At risk / Delayed]
4. **PCI-DSS Compliance:** [Pass / Issues found]
5. **Blockers Impacting OKRs:** [List any quality issues blocking KR achievement]

**Example Monthly Report (Feb 2026):**
- ✅ Fee accuracy: 99.98% (gap narrowed from 0.03% to 0.02%)
- 🟡 Success rate: 95.1% (improved from 94.2%, still below 97% target)
- ✅ BNPL integration: Kredivo testing complete, Akulaku in progress
- ✅ PCI-DSS: No violations found in internal audit
- 🔴 Blocker: Multi-currency rounding logic needs finance validation (delaying currency expansion)

---

## Final Alignment: Quality as a Business Enabler

**Quality in Revenue BU is not a checkbox—it's a revenue driver.**

- Better fee accuracy = more revenue captured
- Higher payment success rate = more GTV = more revenue
- Faster compliance = faster market expansion
- Fewer production issues = better merchant retention

**Every quality decision should ask:** *"Does this help us achieve our OKRs, or does it protect us from risks that would prevent achieving them?"*

Both are valid. Both matter.

---

**Next:** See [quality-scopes/](./quality-scopes/) for feature-level context aligned to these OKRs.
