# Revenue & Growth - Business Unit Context

**Last Updated:** 2026-02-10
**Owner:** Revenue & Growth BU
**QA Lead:** [To be assigned]

---

## What This BU Does

Revenue & Growth Business Unit is responsible for **all money-in and money-out flows** on the Paper.id platform. We own the complete transaction lifecycle from payment initiation to reconciliation, including:

- **Payment Processing** - Credit card, bank transfer, e-wallet integrations
- **Transaction Fees** - Calculation, collection, and distribution of platform fees
- **Subscription Management** - Recurring billing for SaaS and premium features
- **Invoicing & Billing** - Automated invoice generation and payment reminders
- **Payout Processing** - Merchant settlements and fund disbursements
- **Revenue Analytics** - Transaction reporting and financial dashboards

**Business Model:**
Paper.id charges a percentage-based transaction fee on every payment processed through the platform. Our revenue scales with transaction volume and value.

**Current Fee Structure:**
- Standard transactions: 2.9% + Rp 2,000 per transaction
- High-volume merchants: 2.5% + Rp 1,500 per transaction
- Cross-border transactions: 3.4% + Rp 3,000 per transaction
- Subscription products: 3.5% monthly recurring

---

## Key Stakeholders

- **Product Lead:** [Name] - Owns revenue product roadmap
- **Engineering Lead:** [Name] - Payment infrastructure and APIs
- **Finance Lead:** [Name] - Revenue recognition and compliance
- **Design Lead:** [Name] - Payment UX and conversion optimization
- **QA Lead:** [Name] - Quality assurance for financial flows
- **Compliance Officer:** [Name] - Regulatory adherence (PCI-DSS, local regulations)

---

## Main Products/Features Owned

### 1. Payment Gateway Integration
Multi-provider payment processing (Stripe, Midtrans, Xendit, Doku)
- Credit card processing
- Bank transfer (Virtual Account)
- E-wallets (GoPay, OVO, Dana, ShopeePay)
- Buy Now Pay Later (Kredivo, Akulaku)

### 2. Transaction Fee Engine
Platform fee calculation and collection system
- Percentage-based fee calculation
- Minimum and maximum fee thresholds
- Dynamic fee adjustment based on merchant tier
- Multi-currency fee calculation
- Promo and discount application logic

### 3. Subscription Billing
Recurring payment management for SaaS products
- Monthly/annual subscription plans
- Mid-cycle plan changes and proration
- Failed payment retry logic
- Subscription lifecycle (trial, active, paused, cancelled, expired)
- Dunning management for failed payments

### 4. Invoice & Receipt System
Automated invoicing and financial documentation
- Invoice generation with tax calculations
- Payment receipt issuance
- Credit note and refund documentation
- Multi-currency invoicing
- Compliance with local tax regulations

### 5. Merchant Payout System
Settlement of funds to merchants
- Payout scheduling (daily, weekly, bi-weekly, monthly)
- Fee deduction before payout
- Hold periods for risk management
- Bank transfer integration for payouts
- Payout reconciliation and reporting

### 6. Revenue Dashboard & Analytics
Financial reporting and insights
- Real-time transaction monitoring
- Revenue forecasting and trends
- Fee revenue breakdown
- Merchant performance analytics
- Chargeback and dispute tracking

---

## Core User Journeys

### Customer-Facing Flows:
1. **Customer makes a purchase**
   - Selects payment method → Enters payment details → Confirms payment → Receives confirmation

2. **Customer subscribes to a service**
   - Chooses subscription plan → Enters billing info → First charge processed → Recurring billing starts

3. **Customer requests a refund**
   - Initiates refund request → Merchant approves → Refund processed → Funds returned

### Merchant-Facing Flows:
4. **Merchant receives a payout**
   - Settlement period ends → Fees deducted → Payout initiated → Funds deposited to bank

5. **Merchant upgrades payment methods**
   - Reviews available payment options → Enables new methods → Tests in sandbox → Goes live

6. **Merchant reviews transaction reports**
   - Accesses revenue dashboard → Filters by date/method → Exports report → Reconciles with bank

---

## Technical Context

### Primary Tech Stack:
- **Backend:** Node.js (Express), Python (FastAPI for fee calculation engine)
- **Database:** PostgreSQL (transactional data), Redis (caching for fee lookups)
- **Message Queue:** RabbitMQ (async payment processing)
- **Payment SDKs:** Stripe SDK, Midtrans Snap, Xendit API

### Key Integrations:
- **Payment Gateways:** Stripe, Midtrans, Xendit, Doku
- **Banking:** Virtual Account providers (BCA, Mandiri, BNI, Permata)
- **E-Wallets:** GoPay, OVO, Dana, ShopeePay APIs
- **Accounting:** Integration with internal finance system
- **Compliance:** PCI-DSS scanning, fraud detection (Sift Science)

### Deployment:
- **Infrastructure:** Kubernetes on AWS (EKS)
- **Database:** RDS PostgreSQL (Multi-AZ for high availability)
- **Caching:** ElastiCache Redis
- **Monitoring:** Datadog (APM, transaction monitoring)
- **Logging:** CloudWatch, centralized logging with ELK stack

### Critical Dependencies:
- **Payment provider uptime** - We depend on external gateway availability
- **Bank network stability** - Virtual accounts require bank API connectivity
- **Currency exchange rates** - Real-time rates for cross-border transactions
- **Tax calculation service** - Accurate tax computation for invoicing

---

## Business Metrics & OKRs Alignment

### Key Metrics We Track:
- **Gross Transaction Volume (GTV):** Total value of payments processed
- **Revenue:** Fee income generated (target: 2.9% average effective rate)
- **Transaction Success Rate:** % of payment attempts that succeed (target: >95%)
- **Payout Accuracy:** % of payouts processed correctly (target: 100%)
- **Chargebacks:** % of transactions disputed (target: <0.5%)
- **Payment Method Adoption:** Distribution across payment methods

### Current OKRs (Q1 2026):
- **O1:** Increase platform revenue by 30% QoQ
  - KR1: Achieve $10M in Gross Transaction Volume
  - KR2: Maintain 2.9% effective transaction fee rate
  - KR3: Launch 2 new payment methods (BNPL expansion)

- **O2:** Improve transaction reliability and user experience
  - KR1: Achieve 97% payment success rate (up from 94%)
  - KR2: Reduce payment friction (3-click checkout)
  - KR3: Support 5 new currencies

- **O3:** Ensure financial compliance and security
  - KR1: Maintain PCI-DSS Level 1 certification
  - KR2: Zero financial discrepancies in audits
  - KR3: Implement real-time fraud detection

---

## Known Challenges & Pain Points

### 1. Payment Gateway Downtime
- **Problem:** External providers occasionally have outages (Stripe, Midtrans)
- **Impact:** Failed transactions = lost revenue + poor UX
- **Mitigation:** Multi-provider failover logic, status page monitoring

### 2. Complex Fee Calculation Logic
- **Problem:** Dynamic fees based on merchant tier, promo codes, currency, payment method
- **Impact:** Fee calculation bugs directly impact revenue (over/under-charging)
- **Mitigation:** Extensive test coverage, fee simulation tools, manual validation

### 3. Subscription Edge Cases
- **Problem:** Mid-cycle plan changes, proration, failed payment retries have many edge cases
- **Impact:** Revenue leakage or customer overbilling complaints
- **Mitigation:** Clear state machine, comprehensive scenario testing

### 4. Multi-Currency Complexity
- **Problem:** Exchange rate fluctuations, rounding differences, currency conversion fees
- **Impact:** Revenue variance, reconciliation mismatches
- **Mitigation:** Real-time rate snapshots, rounding rules documentation

### 5. Reconciliation Gaps
- **Problem:** Timing differences between payment gateway reports and internal records
- **Impact:** Finance team manual reconciliation work, delayed reporting
- **Mitigation:** Automated reconciliation jobs, alerting for discrepancies

---

## Domain-Specific Terminology

| Term | Definition | Example |
|------|------------|---------|
| **GTV (Gross Transaction Volume)** | Total payment value processed, before fees | Rp 100,000,000 in transactions |
| **MDR (Merchant Discount Rate)** | Fee charged to merchant per transaction | 2.9% MDR on Rp 100,000 = Rp 2,900 |
| **Settlement** | Process of transferring funds to merchant | Daily settlement at 6 PM WIB |
| **Chargeback** | Customer dispute reversal | Customer contests charge, funds reversed |
| **Proration** | Partial billing for mid-cycle changes | Upgrade plan mid-month, charged proportionally |
| **Virtual Account (VA)** | Bank-issued account for bank transfers | BCA VA: 1234567890 |
| **3DS (3D Secure)** | Additional authentication for card payments | OTP verification for cards |
| **Webhook** | Real-time payment status notification | Stripe sends webhook on payment success |
| **Dunning** | Automated retry for failed subscription payments | Retry failed charge 3 times over 7 days |

---

## Quality Implications

**Why quality matters differently for Revenue BU:**

1. **Financial Correctness is Binary**
   - Either the fee calculation is right (100% correct) or it's wrong
   - No "mostly works" - every Rupiah matters at scale
   - Bugs in fee logic = revenue loss or overcharging (compliance risk)

2. **Revenue Impact is Direct**
   - Payment failures = immediate revenue loss
   - Fee undercalculation = permanent revenue leakage
   - Fee overcalculation = customer churn + refund costs

3. **Compliance is Non-Negotiable**
   - PCI-DSS violations can shut down the platform
   - Tax calculation errors have legal consequences
   - Data breach impacts customer trust irrecoverably

4. **User Trust is Fragile**
   - One incorrect charge destroys trust
   - Payment UX friction directly impacts conversion
   - Refund delays create support burden

5. **Scale Amplifies Impact**
   - 0.01% fee calculation error across $10M GTV = $1,000 revenue loss/gain
   - Small bugs become large financial impacts at volume
   - Performance issues cause real revenue loss (abandoned carts)

---

## How This BU Differs from Others

| Aspect | Revenue BU | Other BUs (Network, Pivot) |
|--------|-----------|---------------------------|
| **Error Tolerance** | Zero tolerance for financial errors | Some bugs can be fixed post-release |
| **Testing Rigor** | Every edge case must be tested | Prioritized testing based on risk |
| **Rollback Risk** | Difficult (money already moved) | Easier to rollback features |
| **Compliance** | Heavily regulated (PCI-DSS, tax laws) | Less regulatory constraints |
| **Audit Trail** | Every transaction logged immutably | Logging for debugging, not audit |
| **Deployment** | Blue-green with immediate rollback capability | Canary deploys acceptable |

---

## Notes for QA Team

**When testing Revenue BU features, always consider:**

✅ **Financial Correctness:**
- Are fee calculations accurate to the last Rupiah?
- Do rounding rules match finance expectations?
- Are multi-currency conversions using correct rates?

✅ **Edge Cases:**
- What happens if payment gateway times out?
- How are failed payments retried?
- What if user changes plan mid-cycle?

✅ **Compliance:**
- Is PCI-DSS data handling followed (no card data in logs)?
- Are tax calculations correct for all regions?
- Is audit trail complete and tamper-proof?

✅ **Idempotency:**
- Can the same payment be accidentally processed twice?
- Are webhook retries handled safely?
- Is payout processing idempotent?

✅ **Performance:**
- Can the system handle peak transaction volume (e.g., payday)?
- Are payment flows completing within SLA (< 3 seconds)?
- Is rate limiting in place to prevent abuse?

---

**This BU context should be referenced before testing ANY Revenue & Growth feature.**

See [_quality-flavour.md](./_quality-flavour.md) for what "good quality" specifically means in this BU.
