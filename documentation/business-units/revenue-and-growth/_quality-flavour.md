# Revenue & Growth - Quality Flavour

**What "Good Quality" Means for Revenue BU**

---

## Core Quality Philosophy

For Revenue & Growth, quality is not subjective. **Good quality means zero financial errors, period.**

Unlike other BUs where some bugs can be tolerated post-release, Revenue BU operates under a different standard:
- **Every Rupiah counts** - At $10M GTV, even 0.01% error = $1,000 revenue impact
- **User trust is binary** - One wrong charge destroys confidence
- **Compliance is non-negotiable** - PCI-DSS violations shut down the platform

**Our quality mantra:** *"Would I trust this with my own money?"*

---

## The Three Dimensions of Revenue Quality

### 1. Financial Correctness

**What it means:**
Every fee calculation, every transaction amount, every payout must be mathematically correct to the last Rupiah. No rounding errors. No off-by-one bugs. No "close enough."

**Why it matters:**
- Fee undercalculation = permanent revenue leakage (can't retroactively charge customers)
- Fee overcalculation = customer churn + refund processing costs + reputation damage
- At scale, tiny errors become massive: 0.01% of Rp 100B = Rp 10M loss/gain

**How we measure it:**
- ✅ 100% accuracy in fee calculation tests (zero tolerance for "mostly works")
- ✅ Reconciliation match rate: 100% (every Rupiah in our system matches gateway reports)
- ✅ Zero financial discrepancies in production audits

**Example scenarios that MUST work:**
- Fee calculation for Rp 99,999 at 2.9% + Rp 2,000 = exactly Rp 4,899.97 (not Rp 4,900)
- Multi-currency conversion: $100 USD → IDR at rate 15,234.50 = Rp 1,523,450 (not rounded)
- Proration: Mid-month plan upgrade from Rp 100K to Rp 200K (15 days remaining) = Rp 50,000 charge
- Discount application: 10% off Rp 275,000 = Rp 247,500 (then apply 2.9% fee = Rp 7,177.50)

**Test requirements:**
- Every fee calculation scenario has unit tests with exact expected values
- Integration tests verify end-to-end transaction amounts match expected totals
- Edge case tests for rounding, min/max fees, negative scenarios
- Regression tests for every historical fee calculation bug

---

### 2. Transaction Reliability

**What it means:**
Money movement must be predictable, consistent, and never "lost in flight." Users must trust that when they pay, it works. When we payout, funds arrive.

**Why it matters:**
- Failed payments = abandoned transactions = lost revenue + poor UX
- Duplicate charges = customer complaints + refund overhead + trust erosion
- Missing payouts = merchant churn (our customers depend on timely settlements)

**How we measure it:**
- ✅ Payment success rate: >95% (excluding user errors like insufficient funds)
- ✅ Payout accuracy: 100% (every scheduled payout processes correctly)
- ✅ Idempotency: 0 duplicate charges or payouts due to retry logic
- ✅ Zero "lost transactions" (every initiated payment reaches a terminal state)

**Example scenarios that MUST work:**
- Payment gateway timeout → Retry with same idempotency key → No duplicate charge
- User clicks "Pay Now" twice rapidly → Only one charge processes
- Webhook received multiple times (Stripe retry) → Payment status updates once
- Payout job fails mid-run → Resumes safely without duplicate payouts
- Network interruption during payment → User sees clear status, can retry safely

**Test requirements:**
- Load testing for peak transaction volume (payday, flash sales)
- Timeout and retry scenario testing
- Idempotency key validation (same key = same result)
- Webhook replay testing (multiple deliveries handled safely)
- Failure recovery testing (job restart mid-process)

---

### 3. Regulatory Compliance & Security

**What it means:**
Every payment flow must adhere to PCI-DSS, tax regulations, and data privacy laws. Credit card data must never be logged, stored, or transmitted insecurely. Tax calculations must match local requirements.

**Why it matters:**
- PCI-DSS violations = platform shutdown + massive fines
- Tax errors = legal penalties + finance audit failures
- Data breach = loss of payment processor partnerships + customer trust destroyed
- Non-compliance blocks international expansion

**How we measure it:**
- ✅ PCI-DSS Level 1 certification maintained (annual audit pass)
- ✅ Zero credit card data in logs, databases, or error messages
- ✅ 100% tax calculation accuracy (validated against finance team requirements)
- ✅ Audit trail completeness: every transaction traceable with immutable logs

**Example compliance requirements:**
- **PCI-DSS:**
  - Card numbers masked in UI (show only last 4 digits)
  - CVV never stored (even temporarily)
  - Payment forms use tokenization (Stripe Elements, not raw card fields)
  - TLS 1.2+ for all payment API communication

- **Tax Compliance:**
  - VAT/PPN calculated correctly per Indonesian tax law (11% as of 2026)
  - Invoice formatting matches tax authority requirements
  - Tax-exempt transactions handled correctly (exports, certain services)
  - Quarterly tax report generation matches revenue records

- **Data Privacy:**
  - Customer payment data encrypted at rest (AES-256)
  - GDPR/PDPA-compliant data retention (delete after 7 years)
  - User consent for storing payment methods
  - Right to data deletion honored (anonymize transaction history)

**Test requirements:**
- Security testing: pen-testing payment flows quarterly
- Compliance checklist validated before every release
- Log review: automated scanning for PCI-DSS violations (regex for card patterns)
- Tax calculation tests for all supported regions
- Data retention policy enforcement tests

---

## Quality Gates Specific to Revenue BU

Before any Revenue feature can be released to production, it MUST pass:

### Pre-Development Gates:
- [ ] **QA Context Brief created** - Product vision, design intent, and quality risks documented
- [ ] **Fee calculation logic reviewed** - Finance team validates expected fee amounts
- [ ] **Compliance checklist completed** - PCI-DSS, tax, data privacy requirements identified
- [ ] **Rollback plan defined** - How to revert if financial discrepancies found post-release

### Development Gates:
- [ ] **Unit tests for financial logic: 100% coverage** - Every fee calculation path tested
- [ ] **Integration tests include exact amount validation** - End-to-end flows verify totals
- [ ] **Edge case scenarios tested** - Rounding, min/max fees, negative amounts, zero amounts
- [ ] **Idempotency tests pass** - Retries don't create duplicates

### Pre-Production Gates:
- [ ] **Manual testing with real payment scenarios** - Test credit cards, bank transfers, e-wallets
- [ ] **Cross-browser payment form testing** - Chrome, Firefox, Safari, mobile browsers
- [ ] **Performance testing under load** - Can handle 10x current peak transaction volume
- [ ] **Security review passed** - No PCI-DSS violations, secure data handling verified
- [ ] **Finance team sign-off** - Revenue calculations validated against business requirements

### Post-Production Gates:
- [ ] **Reconciliation report validated** - First 24 hours of production transactions reconciled
- [ ] **Error rate monitoring** - <1% payment failures (excluding user errors)
- [ ] **No customer complaints about incorrect charges** - Support tickets monitored
- [ ] **Rollback readiness maintained for 48 hours** - Can revert immediately if issues detected

---

## Risk Areas We Watch Closely

### High-Risk Zones (Extra Testing Required):

1. **Fee Calculation Engine**
   - Any change to fee percentages, min/max thresholds, or tier logic
   - Multi-currency fee calculation
   - Promo code and discount application
   - **Why risky:** Direct revenue impact, errors propagate to all transactions

2. **Subscription Proration Logic**
   - Mid-cycle plan changes (upgrade/downgrade)
   - Trial period handling
   - Failed payment retry timing
   - **Why risky:** Complex state machine, many edge cases, recurring revenue at stake

3. **Payment Gateway Integration Changes**
   - Adding new payment providers (Stripe, Midtrans, Xendit)
   - Webhook signature validation
   - Timeout and retry handling
   - **Why risky:** Third-party dependency, idempotency critical, real money in flight

4. **Payout Processing**
   - Fee deduction before payout
   - Payout scheduling and batching
   - Bank transfer initiation
   - **Why risky:** Large amounts, merchant trust, difficult to reverse

5. **Refund Flows**
   - Partial refunds (fee refund calculation)
   - Chargeback handling
   - Multi-payment method refunds (split payment scenarios)
   - **Why risky:** Money leaving the system, finance audit trail

---

## Quality Anti-Patterns (What NOT to Do)

❌ **"It's just a small fee change, we don't need full regression"**
→ Even small changes can have large-scale revenue impact. Always test comprehensively.

❌ **"The payment gateway will handle that edge case"**
→ Never assume third-party behavior. Test every scenario in our control.

❌ **"We can fix financial errors with manual adjustments"**
→ Manual fixes don't scale and create audit complexity. Fix the root cause.

❌ **"Let's deploy this late Friday, low traffic time"**
→ Revenue bugs discovered on weekends cause delayed response. Deploy Monday-Thursday only.

❌ **"Close enough on rounding, finance can reconcile the difference"**
→ Every Rupiah must match. Reconciliation discrepancies are unacceptable.

---

## Quality Patterns (What TO Do)

✅ **"Test with exact expected amounts, not approximations"**
→ Assert fee = Rp 4,899.97, not fee ≈ Rp 4,900

✅ **"Include finance team in test scenario design"**
→ Finance understands edge cases we might miss (tax exempt, multi-currency rounding)

✅ **"Validate in production sandbox before real payment testing"**
→ Use Stripe test mode, Midtrans sandbox - catch integration issues early

✅ **"Monitor reconciliation reports daily for first week post-release"**
→ Catch discrepancies fast, before they compound

✅ **"Document every fee calculation assumption"**
→ Future QAs need to understand why 2.9% + Rp 2,000, not 2.9% rounded up

---

## Success Metrics for Quality

| Metric | Target | Current | Status |
|--------|--------|---------|--------|
| Fee calculation accuracy | 100% | 100% | ✅ Green |
| Payment success rate | >95% | 94.2% | 🟡 Yellow (improvement needed) |
| Payout accuracy | 100% | 100% | ✅ Green |
| Reconciliation match | 100% | 99.8% | 🟡 Yellow (investigate 0.2% gap) |
| PCI-DSS audit | Pass | Pass (last: Q4 2025) | ✅ Green |
| Production incidents (financial) | 0 per month | 1 in Jan 2026 | 🔴 Red (RCA completed) |
| Customer complaints (incorrect charges) | <5 per month | 3 | ✅ Green |

---

## Communication: How We Talk About Revenue Quality

**To Product Team:**
- "This feature impacts revenue calculation - we need finance validation before release"
- "Fee logic change requires regression testing all merchant tiers - plan 2 extra days"

**To Engineering Team:**
- "Payment flows need idempotency keys - let's design retry logic together"
- "Can we add a circuit breaker for payment gateway timeouts?"

**To Finance Team:**
- "New proration logic - can you validate these test scenarios match accounting expectations?"
- "Reconciliation gap detected - need your help to trace the 0.2% difference"

**To Leadership:**
- "Revenue quality is at 99.8% match rate (target: 100%) - investigating root cause"
- "New payment method launch delayed 1 week to ensure financial correctness"

---

## Final Thought: The Revenue Quality Mindset

**Every line of code that touches money is a contract with our users and merchants.**

When we calculate fees, we're promising accuracy.
When we process payments, we're promising reliability.
When we handle refunds, we're promising fairness.

**Good quality in Revenue BU means keeping every promise, every time, at every scale.**

If you wouldn't trust it with your own money, it's not ready for production.

---

**Next:** See [_okr-alignment.md](./_okr-alignment.md) for how quality connects to business goals.
