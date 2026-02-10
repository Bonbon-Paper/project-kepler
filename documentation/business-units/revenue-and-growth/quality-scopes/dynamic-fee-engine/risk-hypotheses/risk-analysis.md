# Dynamic Fee Engine - Risk Analysis

**Created:** 2026-02-10
**QA Lead:** [Name]
**Last Updated:** 2026-02-10

---

## Risk Assessment Framework

For Revenue BU, we assess risks using:

**Severity:** Critical (P0) | High (P1) | Medium (P2) | Low (P3)
**Likelihood:** High (>50%) | Medium (10-50%) | Low (<10%)
**Priority:** Severity × Likelihood

---

## Risk 1: Fee Calculation Errors

### Description
Dynamic fee calculation logic incorrectly computes transaction fees based on merchant tier, leading to revenue leakage (undercharging) or customer complaints (overcharging).

### Severity: **Critical (P0)**
- **Financial Impact:** At Rp 10B monthly GTV scale, 0.01% error = Rp 1M monthly revenue impact
- **Customer Impact:** Incorrect charges erode trust, cause churn
- **Operational Impact:** Manual corrections don't scale, create audit complexity

### Likelihood: **Medium (20%)**
- **Why Medium:** Complex logic with multiple inputs (tier, payment method, currency, promo codes)
- **Historical Data:** We've had 2 fee calculation bugs in past 12 months (0.03% effective rate variance)
- **Mitigating Factors:** Unit tests will cover most paths; finance validation provides second check

### Root Causes (Hypothesized)

**1. Rounding Errors**
- **Scenario:** Percentage-based fee calculated to many decimal places, then rounded incorrectly
- **Example:** 2.7% of Rp 123,456 = Rp 3,333.312 → Should round to Rp 3,333.31 (not Rp 3,333.32)
- **Impact:** Compounds across thousands of transactions per day

**2. Boundary Condition Bugs**
- **Scenario:** GTV exactly at tier threshold (Rp 10,000,000.00) might assign wrong tier
- **Example:** Is Rp 10M the start of Silver tier or end of Bronze tier?
- **Impact:** Entire month of transactions charged at wrong rate

**3. Tier Snapshot Timing Issues**
- **Scenario:** Transaction starts in Bronze tier, completes after tier changes to Silver
- **Example:** Transaction initiated 11:59 PM (Bronze rate), processed 12:01 AM (tier now Silver)
- **Impact:** Fee calculated using wrong tier (race condition)

**4. Multi-Currency Conversion Errors**
- **Scenario:** GTV from USD/SGD transactions converted to IDR for tier assignment using stale FX rates
- **Example:** $1,000 USD transaction → Uses yesterday's rate instead of transaction date rate
- **Impact:** GTV miscalculated, wrong tier assigned

**5. Promo Code Interaction**
- **Scenario:** Promo code discount applied after tier fee, or tier fee not adjusted for promo
- **Example:** 10% off promo + 2.7% tier fee → Does discount apply to original amount or after fee?
- **Impact:** Unclear which takes precedence, inconsistent fee application

### Test Scenarios Required

| Scenario | Input | Expected Output | Priority |
|----------|-------|-----------------|----------|
| Standard Silver tier transaction | Amount: Rp 100,000, Tier: Silver (2.8%) | Fee: Rp 2,800 + Rp 1,900 = Rp 4,700 | P0 |
| Rounding edge case | Amount: Rp 123,456, Tier: Gold (2.7%) | Fee: Rp 3,333.31 + Rp 1,800 = Rp 5,133.31 | P0 |
| Boundary at tier threshold | GTV: Rp 10,000,000.00 exactly | Tier: Silver (not Bronze) | P0 |
| Tier change during transaction | Transaction starts 23:59, completes 00:01 (tier changed) | Uses tier at payment initiation | P1 |
| Multi-currency GTV | $1,000 USD + Rp 5M IDR | GTV converted to IDR using correct rates | P1 |
| Promo code + tier fee | Amount: Rp 100K, Tier: Silver, Promo: 10% off | Fee calculated correctly (define order) | P2 |

### Mitigation Strategy

**Pre-Development:**
- [x] Finance team validates fee calculation formula
- [x] Define rounding rules explicitly (round to 2 decimals, banker's rounding)
- [ ] Clarify boundary conditions (inclusive vs exclusive thresholds)
- [ ] Define promo code precedence (apply before or after tier fee?)

**Development:**
- [ ] Unit tests for all tier × amount combinations (100+ scenarios)
- [ ] Boundary value tests for tier thresholds
- [ ] Timezone handling tests (tier changes at midnight WIB)
- [ ] Multi-currency FX rate snapshot tests

**Pre-Production:**
- [ ] Manual validation: 50 sample transactions across all tiers
- [ ] Finance team sign-off on calculated fees
- [ ] Reconciliation test: Sum of fees matches expected total

**Production:**
- [ ] Daily reconciliation for first 2 weeks
- [ ] Alerting for fee variance >0.01% from expected rate
- [ ] Manual review of first 1,000 dynamic fee transactions

### Rollback Plan
- Database migration reversible (tier columns can be NULLed)
- Feature flag: `dynamic_fee_enabled` (can disable instantly)
- Fallback to Bronze tier rate (2.9%) if tier lookup fails

---

## Risk 2: Tier Assignment Edge Cases

### Description
Monthly GTV calculation incorrectly assigns merchants to wrong tiers due to edge cases in transaction inclusion/exclusion logic.

### Severity: **High (P1)**
- **Financial Impact:** Wrong tier = wrong fee rate for entire month (e.g., Diamond merchant charged Bronze rate = significant revenue loss)
- **Customer Impact:** Unexpected tier changes cause merchant confusion, support tickets

### Likelihood: **Medium (25%)**
- **Why Medium:** GTV calculation involves refunds, chargebacks, multi-currency, timezone handling
- **Historical Data:** No prior GTV calculation (new feature), but similar aggregation queries have had bugs

### Root Causes (Hypothesized)

**1. Refund/Chargeback Deduction Timing**
- **Scenario:** Refund issued in month N+1 for transaction in month N
- **Question:** Is refund deducted from month N GTV (transaction month) or month N+1 GTV (refund month)?
- **Impact:** GTV miscalculated if deduction logic incorrect

**2. Partial Month Handling**
- **Scenario:** New merchant signs up mid-month (Feb 15), processes Rp 3M by Feb 28
- **Question:** Is GTV prorated (Rp 3M over 14 days = Rp 6M projected monthly), or literal (Rp 3M)?
- **Impact:** New merchants might get wrong tier in first month

**3. Timezone Issues**
- **Scenario:** Tier assignment cron job runs at 00:00 WIB, but some transactions timestamped in UTC
- **Question:** Is a transaction at Feb 28 11:59 PM UTC counted in Feb or Mar GTV?
- **Impact:** Off-by-one day errors in GTV calculation

**4. Multi-Currency GTV Conversion**
- **Scenario:** Merchant processes $10K USD (≈ Rp 152M at rate 15,200) + Rp 50M IDR
- **Question:** Which FX rate used for conversion? Real-time, end-of-day, or end-of-month?
- **Impact:** GTV total varies based on FX rate snapshot time

**5. Failed/Pending Transactions**
- **Scenario:** Transaction pending at month-end (initiated Feb 28, completed Mar 2)
- **Question:** Counted in Feb GTV or Mar GTV?
- **Impact:** GTV total could shift between months

### Test Scenarios Required

| Scenario | Input | Expected Behavior | Priority |
|----------|-------|-------------------|----------|
| Refund in following month | Feb transaction Rp 10M, refund in Mar Rp 2M | Feb GTV = Rp 10M, Mar GTV = Rp 0 (deduct from original month) | P0 |
| New merchant mid-month | Signed up Feb 15, Rp 3M GTV by Feb 28 | Tier based on literal Rp 3M (Bronze tier) | P1 |
| Transaction at month boundary | Feb 28 11:59 PM WIB transaction | Counted in Feb GTV | P0 |
| Multi-currency GTV | $5K USD + Rp 50M IDR | USD converted using end-of-month rate, summed | P1 |
| Pending transaction at month-end | Initiated Feb 28, completed Mar 2 | Counted in Mar GTV (completion date) | P2 |

### Mitigation Strategy

**Pre-Development:**
- [ ] Define GTV calculation rules with finance team (refund handling, partial month, FX rates)
- [ ] Document timezone handling explicitly (all timestamps in WIB)
- [ ] Clarify transaction inclusion criteria (completed only, not pending)

**Development:**
- [ ] Unit tests for GTV calculation with edge cases
- [ ] Timezone conversion tests (UTC → WIB)
- [ ] Boundary date tests (last second of month)

**Pre-Production:**
- [ ] Manual GTV calculation for 10 test merchants, compare with automated calculation
- [ ] Finance team validates GTV logic matches accounting standards

**Production:**
- [ ] Run GTV calculation job in dry-run mode for 1 month before enforcement (log results, don't apply)
- [ ] Manual review of tier assignments for first 100 merchants
- [ ] Grace period: Tier changes don't affect fees for first month (validation period)

### Rollback Plan
- If tier assignments look incorrect, pause fee application
- Fall back to Bronze tier for all merchants until GTV logic fixed

---

## Risk 3: Race Conditions - Tier Change During Transaction

### Description
Transaction initiated in one tier, completes after tier changes (midnight tier update), causing fee mismatch.

### Severity: **Medium (P2)**
- **Financial Impact:** Isolated to transactions crossing midnight tier boundary (low volume)
- **Customer Impact:** Merchant confused why receipt shows different rate than expected

### Likelihood: **Low (5%)**
- **Why Low:** Tier changes happen at midnight WIB (low transaction volume); payment processing typically <10 seconds
- **Historical Data:** No historical data (new scenario)

### Root Causes (Hypothesized)

**1. Tier Lookup at Wrong Time**
- **Scenario:** Fee calculated at payment completion (00:01 AM), but tier changed at 00:00 AM
- **Example:** Transaction initiated 11:58 PM (Bronze tier), tier changes to Silver at midnight, fee calculated at 00:01 AM
- **Impact:** Fee uses new tier (Silver rate) instead of tier at initiation (Bronze rate)

**2. Database Tier Update During Payment Processing**
- **Scenario:** Tier assignment cron job updates `merchant_tiers` table while payment in flight
- **Example:** Payment starts → Tier update job runs → Payment reads new tier → Wrong fee calculated
- **Impact:** Race condition between tier update and fee lookup

### Test Scenarios Required

| Scenario | Setup | Expected Behavior | Priority |
|----------|-------|-------------------|----------|
| Transaction crosses midnight | Initiated 11:59 PM Bronze, completed 00:01 AM (now Silver) | Fee uses Bronze rate (tier at initiation) | P1 |
| Concurrent tier update & payment | Tier update job running, payment initiated simultaneously | Transaction locks tier row, waits for update to complete | P2 |

### Mitigation Strategy

**Design Decision:**
- **Tier Snapshot at Payment Initiation:** Transaction records `applied_tier`, `fee_percentage`, `fee_fixed_amount` at the moment payment starts
- **Immutable Fee:** Once tier snapshotted, fee doesn't change even if tier updates mid-transaction

**Implementation:**
- [ ] Payment initiation API saves tier snapshot to `transactions` table
- [ ] Fee calculation reads snapshotted tier (not real-time tier lookup)
- [ ] Tier assignment cron job uses database transaction isolation to prevent read conflicts

**Testing:**
- [ ] Simulate transaction initiated 23:59:59, completed 00:00:01 (crosses midnight)
- [ ] Verify snapshotted tier used for fee calculation
- [ ] Load test: 1,000 transactions initiated during tier change window (23:55-00:05)

### Rollback Plan
- If race condition detected, tier assignment job can be paused
- Transactions will continue using existing tier until job resumes

---

## Risk 4: Notification Failures

### Description
Email notifications for tier changes fail to deliver, causing merchants to be surprised by fee changes.

### Severity: **Medium (P2)**
- **Financial Impact:** Indirect (support burden, potential churn if merchants feel blindsided)
- **Customer Impact:** Poor UX if merchant discovers tier change via invoice instead of proactive notification

### Likelihood: **Low (10%)**
- **Why Low:** Email service (SendGrid) is reliable (>99% delivery rate)
- **Historical Data:** Email delivery failures <1% in past 6 months

### Mitigation Strategy

**Primary Notification: Email (30 days before tier change)**
- [ ] Retry logic: 3 attempts over 24 hours if initial send fails
- [ ] Monitoring: Alert if email delivery rate <95%

**Backup Notification: Dashboard Banner**
- [ ] In-app banner: "Your tier will change to Silver on Mar 1, 2026"
- [ ] Visible for 30 days before tier change
- [ ] Dismissable but reappears on next login until tier change date

**Fallback Notification: Invoice/Receipt**
- [ ] Every transaction receipt shows: "Fee rate: 2.8% (Silver tier)"
- [ ] Tier info visible even if email notification missed

### Testing
- [ ] Email delivery testing (test SendGrid integration)
- [ ] Dashboard banner display testing (across browsers)
- [ ] Receipt tier display testing

---

## Risk 5: Refund/Chargeback Impact on GTV

### Description
Refunds and chargebacks incorrectly deducted from (or not deducted from) GTV, causing wrong tier assignments.

### Severity: **Medium (P2)**
- **Financial Impact:** Merchant downgraded incorrectly (loses lower fee rate) or upgraded incorrectly (revenue leakage)
- **Customer Impact:** Unexpected tier changes

### Likelihood: **Medium (20%)**
- **Why Medium:** Refund timing varies; chargebacks can occur months after original transaction

### Test Scenarios Required

| Scenario | Input | Expected Behavior | Priority |
|----------|-------|-------------------|----------|
| Same-month refund | Feb transaction Rp 10M, refund Rp 2M (both in Feb) | Feb GTV = Rp 8M (net) | P0 |
| Cross-month refund | Feb transaction Rp 10M, refund in Mar Rp 2M | Feb GTV = Rp 8M (adjust retroactively) | P1 |
| Chargeback 3 months later | Feb transaction Rp 5M, chargeback in May | Adjust May GTV (not Feb), tier recalculated | P2 |

### Mitigation Strategy

**Business Rule (Defined with Finance):**
- **Same-month refund:** Deduct from current month GTV
- **Cross-month refund:** Deduct from original transaction month GTV (adjust retroactively)
- **Chargeback:** Deduct from chargeback month GTV (can't change past tiers)

**Implementation:**
- [ ] GTV calculation query excludes refunded amounts
- [ ] Chargeback handling updates current month GTV only
- [ ] Historical tier assignments never retroactively changed (prevents billing confusion)

---

## Summary: Risk Prioritization

| Risk | Severity | Likelihood | Priority | Mitigation Effort |
|------|----------|------------|----------|-------------------|
| 1. Fee Calculation Errors | P0 Critical | Medium | **Highest** | High (extensive testing) |
| 2. Tier Assignment Edge Cases | P1 High | Medium | **High** | High (define rules, test boundaries) |
| 3. Race Conditions (Tier Change) | P2 Medium | Low | **Medium** | Low (tier snapshot design) |
| 4. Notification Failures | P2 Medium | Low | **Low** | Low (retry logic + fallback) |
| 5. Refund/Chargeback GTV Impact | P2 Medium | Medium | **Medium** | Medium (define business rules) |

---

## Risk Mitigation Timeline

**Sprint 24 (Current):**
- [x] Risk analysis completed
- [ ] Business rules clarified with finance (refunds, boundaries, FX rates)
- [ ] Unit test scenarios defined

**Sprint 25:**
- [ ] Unit tests implemented (100% coverage)
- [ ] Integration tests for tier assignment
- [ ] Manual testing for top 3 risks

**Sprint 26:**
- [ ] Finance validation
- [ ] Production dry-run (GTV calculation without fee enforcement)
- [ ] Phased rollout (10% merchants)

---

**Next:** See [validation-notes/](../validation-notes/) for detailed test execution results.
