# Dynamic Transaction Fee Engine - Quality Scope

**Feature Type:** Revenue-Critical Enhancement
**Sprint:** Sprint 24-26 (Feb 2026)
**Priority:** P0 (Directly impacts revenue)
**Owner:** Revenue & Growth BU
**QA Lead:** [Name]
**Created:** 2026-02-10
**Status:** In Development

---

## Why This Exists

### The Problem
Currently, Paper.id uses a **flat transaction fee rate** (2.9% + Rp 2,000) for all merchants. This creates two issues:

1. **Revenue Leakage:** High-volume merchants should pay lower rates (economies of scale), but we can't offer tiered pricing today
2. **Competitive Disadvantage:** Competitors offer volume-based pricing, making us less attractive to large merchants

### The Business Opportunity
Implementing **dynamic fee calculation** based on merchant tier unlocks:
- **Revenue Growth:** Attract high-volume merchants (more GTV at slightly lower margin = more absolute revenue)
- **Revenue Protection:** Lock in current merchants with loyalty pricing before they churn to competitors
- **Market Expansion:** Enterprise merchants require tiered pricing to even consider us

**Estimated Impact:**
- Attract 50 high-volume merchants → +$3M GTV/month → +$75K monthly revenue (at 2.5% rate)
- Retain 20 at-risk merchants → Prevent $1M GTV churn → $29K monthly revenue saved
- **Total:** ~$100K monthly revenue upside

---

## Product Vision

**Product Brief:** [Link to product doc]

### User Story
**As a** high-volume merchant processing >$100K monthly
**I want** lower transaction fees based on my volume
**So that** I can reduce payment processing costs and increase profitability

### Success Criteria
- Merchants can see their tier and fee rate in dashboard
- Fee calculation automatically adjusts based on monthly GTV
- Tier changes take effect at start of next billing cycle
- 100% fee calculation accuracy (zero revenue leakage or overcharging)

---

## Design Intent

**Design Doc:** [Link to Figma]

### Key UX Intentions

1. **Transparency:** Merchants must understand exactly what tier they're in and why
   - Dashboard shows: "Your Tier: Gold (Rp 50M-100M monthly GTV) | Your Rate: 2.7% + Rp 1,800"
   - Clear upgrade path: "Process Rp 5M more to reach Platinum tier (2.5% rate)"

2. **No Surprises:** Fee changes communicated 30 days in advance
   - Email notification when tier changes (upgrade or downgrade)
   - Transaction receipt shows applicable fee rate clearly

3. **Grandfathered Rates:** Existing merchants keep current rate for 90 days before tier migration
   - Grace period to prevent sudden fee increases
   - Clear timeline: "Your rate will adjust to 2.9% on May 1, 2026"

### Visual Design Notes
- Tier badges (Bronze, Silver, Gold, Platinum, Diamond) with color coding
- Progress bar showing GTV toward next tier
- Historical fee savings chart (vs standard rate)

---

## Fee Tier Structure (Finalized with Finance)

| Tier | Monthly GTV Range | Transaction Fee | Target Merchants |
|------|------------------|-----------------|------------------|
| **Bronze (Default)** | Rp 0 - 10M | 2.9% + Rp 2,000 | New & small merchants |
| **Silver** | Rp 10M - 50M | 2.8% + Rp 1,900 | Growing merchants |
| **Gold** | Rp 50M - 100M | 2.7% + Rp 1,800 | Mid-size merchants |
| **Platinum** | Rp 100M - 500M | 2.5% + Rp 1,500 | High-volume merchants |
| **Diamond** | Rp 500M+ | 2.3% + Rp 1,000 | Enterprise merchants |

**Additional Business Rules:**
- GTV calculated based on **previous month's completed transactions** (not pending/failed)
- Tier assignment happens on **1st of each month** at 00:00 WIB
- Custom enterprise rates (negotiated manually) override tier system
- Refunds/chargebacks deducted from GTV for tier calculation
- Multi-currency GTV converted to IDR using end-of-month exchange rate

---

## Technical Context

### Architecture

**Components Involved:**
1. **Fee Calculation Engine (Python/FastAPI)**
   - Input: Transaction amount, payment method, merchant ID
   - Logic: Lookup merchant tier → Calculate fee → Return fee amount
   - Output: Fee in Rupiah (rounded to 2 decimal places)

2. **Merchant Tier Service (Node.js)**
   - Calculates monthly GTV per merchant
   - Assigns tier based on GTV ranges
   - Cron job runs 1st of month at 00:00 WIB

3. **Notification Service (Node.js)**
   - Sends tier change emails
   - Triggers 30 days before fee adjustment

4. **Dashboard API (Node.js)**
   - Exposes tier info to frontend
   - Historical GTV and tier progression

### Database Schema

**New Tables:**

```sql
-- Merchant Tiers
CREATE TABLE merchant_tiers (
  id UUID PRIMARY KEY,
  merchant_id UUID REFERENCES merchants(id),
  tier VARCHAR(20), -- 'bronze', 'silver', 'gold', 'platinum', 'diamond'
  effective_date DATE,
  monthly_gtv DECIMAL(15,2),
  fee_percentage DECIMAL(4,2),
  fee_fixed_amount DECIMAL(10,2),
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

-- GTV Calculation Cache
CREATE TABLE monthly_gtv (
  id UUID PRIMARY KEY,
  merchant_id UUID REFERENCES merchants(id),
  month DATE, -- First day of month (e.g., 2026-02-01)
  total_gtv DECIMAL(15,2),
  transaction_count INTEGER,
  calculated_at TIMESTAMP
);
```

**Updated Tables:**

```sql
-- Transactions table adds tier reference
ALTER TABLE transactions
  ADD COLUMN applied_tier VARCHAR(20),
  ADD COLUMN fee_percentage DECIMAL(4,2),
  ADD COLUMN fee_fixed_amount DECIMAL(10,2);
```

### API Endpoints

**New:**
- `GET /api/merchants/{id}/tier` - Get current tier info
- `GET /api/merchants/{id}/gtv-history` - Monthly GTV breakdown
- `POST /api/admin/merchants/{id}/override-tier` - Manual tier override (admin only)

**Updated:**
- `POST /api/transactions/calculate-fee` - Now includes tier-based logic

### Third-Party Dependencies
- **None (internal calculation)** - Fee tier logic is fully owned by us
- Depends on existing transaction data from payment gateways (Stripe, Midtrans, etc.)

---

## Quality Risks & Hypotheses

See detailed [risk-hypotheses/](./risk-hypotheses/) folder for full risk analysis.

### Top 5 Quality Risks

**Risk 1: Fee Calculation Errors → Revenue Leakage or Overcharging**
- **Severity:** Critical (P0)
- **Likelihood:** Medium (complex logic with edge cases)
- **Impact:** Every 0.01% error = Rp 1,000 monthly at Rp 10M GTV scale
- **Mitigation:** Comprehensive unit tests, manual validation with finance team

**Risk 2: Tier Assignment Edge Cases → Wrong Fee Applied**
- **Severity:** High (P1)
- **Likelihood:** Medium (GTV calculation has rounding, timezone, currency conversion complexity)
- **Impact:** Merchant charged wrong rate for entire month
- **Mitigation:** Test boundary conditions (exactly Rp 10M, Rp 50M, etc.), timezone handling

**Risk 3: Race Conditions → Tier Change During Active Transaction**
- **Severity:** Medium (P2)
- **Likelihood:** Low (tier changes happen at midnight, low transaction volume)
- **Impact:** Transaction could use wrong fee if processed exactly at tier change time
- **Mitigation:** Transaction records tier snapshot at payment initiation

**Risk 4: Notification Failures → Merchants Surprised by Fee Change**
- **Severity:** Medium (P2)
- **Likelihood:** Low (email service is reliable)
- **Impact:** Customer support burden, merchant complaints
- **Mitigation:** Retry logic for email delivery, dashboard banner as backup notification

**Risk 5: Refund/Chargeback Handling → Incorrect GTV Calculation**
- **Severity:** Medium (P2)
- **Likelihood:** Medium (refunds are common, chargeback timing varies)
- **Impact:** Merchant tier downgraded incorrectly if refunds deducted incorrectly
- **Mitigation:** Test refund scenarios, validate GTV recalculation logic

---

## Validation Approach

### Testing Strategy

**1. Unit Testing (Engineering + QA Review)**
- Fee calculation for all tier combinations
- Boundary value testing (GTV exactly at tier thresholds)
- Rounding logic validation
- Edge cases: Zero amount, negative amounts, very large amounts

**2. Integration Testing (QA Owned)**
- End-to-end transaction with tier-based fee
- Tier assignment cron job execution
- Notification delivery
- Dashboard API tier display

**3. Manual Testing (QA Owned)**
- User journey: Merchant views tier → Processes transaction → Sees fee applied correctly
- Admin override scenarios (custom enterprise rates)
- Cross-browser testing for dashboard tier display

**4. Performance Testing (QA + Engineering)**
- Fee calculation under load (10,000 transactions/minute)
- GTV calculation job completes within 30 minutes for all merchants
- Database query performance (tier lookup)

**5. Finance Validation (QA Facilitates)**
- Sample transactions validated by finance team
- Fee amounts match expected calculations
- Reconciliation report includes tier information

### Test Data Requirements
- **Merchants across all tiers:** Bronze (10), Silver (5), Gold (3), Platinum (2), Diamond (1)
- **Edge case merchants:** GTV exactly at tier boundary (e.g., Rp 10,000,000.00)
- **Multi-currency merchants:** GTV from transactions in USD, SGD, EUR (requires FX conversion)
- **Enterprise merchant with custom rate:** Overrides tier system

---

## Quality Gates

**Pre-Development:**
- [x] QA Context Brief created *(this document)*
- [x] Finance team validated fee tier structure
- [ ] Design approved tier display in dashboard
- [x] Rollback plan defined

**Development:**
- [ ] Unit tests: 100% coverage for fee calculation logic
- [ ] Integration tests include exact fee validation
- [ ] Edge case scenarios tested (boundaries, rounding, timezone)
- [ ] Performance tests: Fee calculation <100ms per transaction

**Pre-Production:**
- [ ] Manual testing with test merchant accounts across all tiers
- [ ] Finance team validates sample fee calculations
- [ ] Dashboard tier display tested (Chrome, Firefox, Safari, mobile)
- [ ] Notification emails reviewed by product team
- [ ] Security review: Admin override endpoint properly protected

**Production:**
- [ ] Phased rollout: 10% of merchants first week
- [ ] Daily reconciliation for first 2 weeks
- [ ] Customer support monitoring for tier-related questions
- [ ] Rollback ready for 48 hours post-launch

---

## Success Metrics

### Launch Criteria (Sprint 26 End):
- [ ] All merchants assigned to correct tier (100% accuracy validated)
- [ ] Fee calculation accuracy: 100% (zero discrepancies in reconciliation)
- [ ] Notification delivery: >99% (tier change emails sent successfully)
- [ ] Dashboard tier display: Works across all supported browsers
- [ ] Zero customer complaints about incorrect fee charges

### Post-Launch (30 Days):
- [ ] Revenue impact: +$75K monthly revenue from new high-volume merchants
- [ ] Merchant retention: 20 at-risk merchants retained (prevented churn)
- [ ] Support ticket volume: <10 tier-related questions per week
- [ ] Fee calculation error rate: <0.001% (1 error per 100,000 transactions)

---

## Related Documentation

- **Product Brief:** [Link]
- **Design Specs:** [Link to Figma]
- **Engineering Design Doc:** [Link]
- **Finance Approval:** [Link to fee tier approval email]
- **Risk Analysis:** [risk-hypotheses/](./risk-hypotheses/)
- **Existing Tests:** [existing-tests-context/](./existing-tests-context/)
- **Learning Notes:** [learning-notes/](./learning-notes/)

---

## Open Questions

**For Product:**
- [x] ~~What happens to merchants who downgrade tiers (GTV drops)? Immediate fee increase or grace period?~~
  - **Answer:** 30-day grace period before rate increases

**For Finance:**
- [ ] How do we handle partial month GTV (merchant signs up mid-month)?
  - **Status:** Pending finance input (Feb 12 meeting scheduled)

**For Engineering:**
- [ ] Can tier calculation job handle 10,000+ merchants within 30-minute window?
  - **Status:** Performance testing scheduled for Sprint 25

**For Design:**
- [x] ~~How to display tier badge on mobile (limited screen space)?~~
  - **Answer:** Use icon instead of full tier name

---

**Last Updated:** 2026-02-10 | **Next Review:** Sprint 25 Planning

See subdirectories for detailed context:
- [Design Intent](./design-intent/)
- [Risk Hypotheses](./risk-hypotheses/)
- [Existing Tests](./existing-tests-context/)
- [Validation Notes](./validation-notes/)
- [Learning Notes](./learning-notes/)
