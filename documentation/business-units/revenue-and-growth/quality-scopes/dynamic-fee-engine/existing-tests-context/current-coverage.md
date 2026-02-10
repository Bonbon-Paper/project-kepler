# Dynamic Fee Engine - Existing Test Coverage

**Last Updated:** 2026-02-10
**Test Suite Location:** `payment-service/tests/`

---

## Current State: Fee Calculation Tests (Before Dynamic Fees)

### Existing Unit Tests

**File:** `fee-calculator.test.js`

**Coverage:** Basic flat-rate fee calculation

```javascript
describe('FeeCalculator', () => {
  it('calculates standard fee (2.9% + Rp 2,000)', () => {
    const amount = 100000; // Rp 100,000
    const fee = calculateFee(amount);
    expect(fee).toBe(4900); // 2.9% of 100,000 = 2,900 + 2,000 = 4,900
  });

  it('handles minimum transaction amount (Rp 1,000)', () => {
    const amount = 1000;
    const fee = calculateFee(amount);
    expect(fee).toBe(2029); // 2.9% of 1,000 = 29 + 2,000 = 2,029
  });

  it('handles large amounts (Rp 10,000,000)', () => {
    const amount = 10000000;
    const fee = calculateFee(amount);
    expect(fee).toBe(292000); // 2.9% of 10M = 290,000 + 2,000 = 292,000
  });
});
```

**What's Missing for Dynamic Fees:**
- ❌ No tier-based fee calculation
- ❌ No boundary value tests (tier thresholds)
- ❌ No rounding edge case tests
- ❌ No multi-currency tests
- ❌ No promo code interaction tests

---

### Existing Integration Tests

**File:** `payment-flow.test.js`

**Coverage:** End-to-end payment processing

```javascript
describe('Payment Flow', () => {
  it('completes credit card payment with fee', async () => {
    const payment = await initiatePayment({
      amount: 100000,
      paymentMethod: 'credit_card',
      merchantId: 'test-merchant-1'
    });

    expect(payment.status).toBe('completed');
    expect(payment.totalAmount).toBe(104900); // Amount + fee
    expect(payment.fee).toBe(4900);
  });
});
```

**What's Missing for Dynamic Fees:**
- ❌ No merchant tier setup in test fixtures
- ❌ No tier-based fee assertions
- ❌ No tier change scenario tests

---

### Existing Database Tests

**File:** `transaction-model.test.js`

**Coverage:** Transaction data persistence

```javascript
describe('Transaction Model', () => {
  it('saves transaction with fee', async () => {
    const transaction = await Transaction.create({
      amount: 100000,
      fee: 4900,
      merchantId: 'merchant-123'
    });

    expect(transaction.amount).toBe(100000);
    expect(transaction.fee).toBe(4900);
  });
});
```

**What's Missing for Dynamic Fees:**
- ❌ No `applied_tier`, `fee_percentage`, `fee_fixed_amount` columns in test schema
- ❌ No tier snapshot validation

---

## What Needs to Be Added

### 1. Tier-Based Fee Calculation Unit Tests

**New File:** `fee-calculator-dynamic.test.js`

**Required Test Cases:**

```javascript
describe('Dynamic Fee Calculator', () => {
  describe('Bronze Tier (2.9% + Rp 2,000)', () => {
    it('calculates fee for Rp 100,000', () => {
      const fee = calculateDynamicFee(100000, 'bronze');
      expect(fee).toBe(4900);
    });
  });

  describe('Silver Tier (2.8% + Rp 1,900)', () => {
    it('calculates fee for Rp 100,000', () => {
      const fee = calculateDynamicFee(100000, 'silver');
      expect(fee).toBe(4700);
    });
  });

  describe('Gold Tier (2.7% + Rp 1,800)', () => {
    it('calculates fee for Rp 100,000', () => {
      const fee = calculateDynamicFee(100000, 'gold');
      expect(fee).toBe(4500);
    });
  });

  describe('Platinum Tier (2.5% + Rp 1,500)', () => {
    it('calculates fee for Rp 500,000', () => {
      const fee = calculateDynamicFee(500000, 'platinum');
      expect(fee).toBe(14000);
    });
  });

  describe('Diamond Tier (2.3% + Rp 1,000)', () => {
    it('calculates fee for Rp 1,000,000', () => {
      const fee = calculateDynamicFee(1000000, 'diamond');
      expect(fee).toBe(24000);
    });
  });

  describe('Rounding Edge Cases', () => {
    it('rounds 2.7% of Rp 123,456 correctly', () => {
      const fee = calculateDynamicFee(123456, 'gold');
      // 2.7% of 123,456 = 3,333.312 → rounds to 3,333.31
      expect(fee).toBe(5133.31); // 3,333.31 + 1,800 = 5,133.31
    });

    it('handles amounts that produce exact decimal fee', () => {
      const fee = calculateDynamicFee(100000, 'silver');
      // 2.8% of 100,000 = 2,800.00 (exact)
      expect(fee).toBe(4700.00);
    });
  });
});
```

**Status:** Not started (Sprint 25)

---

### 2. GTV Calculation Unit Tests

**New File:** `gtv-calculator.test.js`

**Required Test Cases:**

```javascript
describe('GTV Calculator', () => {
  it('calculates monthly GTV from completed transactions', () => {
    const transactions = [
      { amount: 100000, status: 'completed', date: '2026-02-05' },
      { amount: 200000, status: 'completed', date: '2026-02-10' },
      { amount: 50000, status: 'pending', date: '2026-02-15' } // Should be excluded
    ];

    const gtv = calculateMonthlyGTV(transactions, '2026-02');
    expect(gtv).toBe(300000); // 100K + 200K (pending excluded)
  });

  it('excludes refunded amounts from GTV', () => {
    const transactions = [
      { amount: 100000, status: 'completed', date: '2026-02-05' },
      { amount: -30000, status: 'refunded', date: '2026-02-08' }
    ];

    const gtv = calculateMonthlyGTV(transactions, '2026-02');
    expect(gtv).toBe(70000); // 100K - 30K
  });

  it('converts multi-currency transactions to IDR', () => {
    const transactions = [
      { amount: 100, currency: 'USD', status: 'completed', date: '2026-02-05' }, // $100
      { amount: 50000, currency: 'IDR', status: 'completed', date: '2026-02-05' }
    ];

    const exchangeRate = { USD: 15200 }; // $1 = Rp 15,200
    const gtv = calculateMonthlyGTV(transactions, '2026-02', exchangeRate);
    expect(gtv).toBe(1570000); // ($100 * 15,200) + 50,000 = 1,570,000
  });

  it('handles timezone correctly (month boundary)', () => {
    const transactions = [
      { amount: 100000, status: 'completed', date: '2026-02-28T23:59:00+07:00' }, // Feb 28 WIB
      { amount: 200000, status: 'completed', date: '2026-03-01T00:01:00+07:00' }  // Mar 1 WIB
    ];

    const febGTV = calculateMonthlyGTV(transactions, '2026-02');
    expect(febGTV).toBe(100000); // Only Feb 28 transaction

    const marGTV = calculateMonthlyGTV(transactions, '2026-03');
    expect(marGTV).toBe(200000); // Only Mar 1 transaction
  });
});
```

**Status:** Not started (Sprint 25)

---

### 3. Tier Assignment Logic Tests

**New File:** `tier-assignment.test.js`

**Required Test Cases:**

```javascript
describe('Tier Assignment', () => {
  it('assigns Bronze tier for GTV < Rp 10M', () => {
    const tier = assignTier(5000000); // Rp 5M
    expect(tier).toBe('bronze');
  });

  it('assigns Silver tier for GTV Rp 10M - 50M', () => {
    const tier = assignTier(30000000); // Rp 30M
    expect(tier).toBe('silver');
  });

  it('assigns Gold tier for GTV Rp 50M - 100M', () => {
    const tier = assignTier(75000000); // Rp 75M
    expect(tier).toBe('gold');
  });

  it('assigns Platinum tier for GTV Rp 100M - 500M', () => {
    const tier = assignTier(250000000); // Rp 250M
    expect(tier).toBe('platinum');
  });

  it('assigns Diamond tier for GTV >= Rp 500M', () => {
    const tier = assignTier(600000000); // Rp 600M
    expect(tier).toBe('diamond');
  });

  describe('Boundary Conditions', () => {
    it('assigns Silver tier for GTV exactly Rp 10M (inclusive)', () => {
      const tier = assignTier(10000000); // Exactly Rp 10M
      expect(tier).toBe('silver'); // Silver starts at Rp 10M
    });

    it('assigns Gold tier for GTV exactly Rp 50M (inclusive)', () => {
      const tier = assignTier(50000000); // Exactly Rp 50M
      expect(tier).toBe('gold'); // Gold starts at Rp 50M
    });

    it('assigns Bronze tier for GTV Rp 9,999,999 (just below Silver)', () => {
      const tier = assignTier(9999999); // Rp 9,999,999
      expect(tier).toBe('bronze'); // Still Bronze
    });
  });
});
```

**Status:** Not started (Sprint 25)

---

### 4. Integration Tests (End-to-End with Tiers)

**Update File:** `payment-flow.test.js`

**New Test Cases to Add:**

```javascript
describe('Payment Flow with Dynamic Fees', () => {
  it('applies Silver tier fee for Silver merchant', async () => {
    // Setup: Create test merchant with Silver tier
    const merchant = await createTestMerchant({ tier: 'silver' });

    const payment = await initiatePayment({
      amount: 100000,
      paymentMethod: 'credit_card',
      merchantId: merchant.id
    });

    expect(payment.status).toBe('completed');
    expect(payment.fee).toBe(4700); // 2.8% + Rp 1,900
    expect(payment.appliedTier).toBe('silver');
    expect(payment.feePercentage).toBe(2.8);
  });

  it('snapshots tier at payment initiation (not completion)', async () => {
    const merchant = await createTestMerchant({ tier: 'bronze' });

    // Initiate payment (tier = Bronze)
    const paymentId = await initiatePayment({
      amount: 100000,
      merchantId: merchant.id
    });

    // Simulate tier change mid-payment
    await updateMerchantTier(merchant.id, 'silver');

    // Complete payment
    const payment = await completePayment(paymentId);

    // Should use Bronze tier (from initiation), not Silver
    expect(payment.appliedTier).toBe('bronze');
    expect(payment.fee).toBe(4900); // Bronze rate: 2.9% + Rp 2,000
  });
});
```

**Status:** Not started (Sprint 25)

---

## Test Coverage Gaps Summary

| Test Category | Current Coverage | Needed for Dynamic Fees | Gap | Priority |
|---------------|------------------|-------------------------|-----|----------|
| Fee Calculation Unit Tests | 3 tests (flat rate only) | 50+ tests (all tiers, rounding, edge cases) | **High** | P0 |
| GTV Calculation Tests | 0 tests | 20+ tests (refunds, multi-currency, timezone) | **Critical** | P0 |
| Tier Assignment Tests | 0 tests | 15+ tests (boundaries, edge cases) | **High** | P0 |
| Integration Tests (E2E) | 5 tests (no tier awareness) | 15+ tests (tier application, snapshot) | **Medium** | P1 |
| Performance Tests | 0 tests | Load tests for fee calculation & GTV job | **Medium** | P2 |

---

## Test Data Requirements

### Merchant Test Fixtures

**Needed:** 21 test merchants (across all tiers + edge cases)

```javascript
const testMerchants = [
  // Standard tier representatives
  { id: 'bronze-1', tier: 'bronze', monthlyGTV: 5000000 },
  { id: 'silver-1', tier: 'silver', monthlyGTV: 30000000 },
  { id: 'gold-1', tier: 'gold', monthlyGTV: 75000000 },
  { id: 'platinum-1', tier: 'platinum', monthlyGTV: 250000000 },
  { id: 'diamond-1', tier: 'diamond', monthlyGTV: 600000000 },

  // Boundary edge cases
  { id: 'boundary-9.9M', tier: 'bronze', monthlyGTV: 9999999 }, // Just below Silver
  { id: 'boundary-10M', tier: 'silver', monthlyGTV: 10000000 }, // Exactly at Silver threshold
  { id: 'boundary-10.1M', tier: 'silver', monthlyGTV: 10000001 }, // Just above Silver
  { id: 'boundary-50M', tier: 'gold', monthlyGTV: 50000000 }, // Exactly at Gold threshold
  { id: 'boundary-100M', tier: 'platinum', monthlyGTV: 100000000 }, // Exactly at Platinum
  { id: 'boundary-500M', tier: 'diamond', monthlyGTV: 500000000 }, // Exactly at Diamond

  // Multi-currency merchants
  { id: 'multi-currency-1', tier: 'silver', transactions: [
      { amount: 1000, currency: 'USD' },
      { amount: 500, currency: 'SGD' },
      { amount: 10000000, currency: 'IDR' }
    ]
  },

  // Edge cases
  { id: 'new-merchant-mid-month', tier: 'bronze', signupDate: '2026-02-15', monthlyGTV: 3000000 },
  { id: 'custom-rate-merchant', tier: 'custom', feePercentage: 2.0, feeFixed: 1000 } // Enterprise negotiated rate
];
```

**Status:** Test fixtures not created yet (Sprint 25 task)

---

## Automation Strategy

### Unit Tests (Fast, Isolated)
- **Target:** 200+ unit tests for fee calculation, GTV, tier assignment
- **Execution Time:** <10 seconds
- **Run Frequency:** Every commit (CI pipeline)

### Integration Tests (End-to-End)
- **Target:** 30+ integration tests for payment flows with tiers
- **Execution Time:** ~5 minutes
- **Run Frequency:** Every PR (before merge)

### Performance Tests (Load & Scale)
- **Target:** 5+ load tests for fee calculation & GTV batch job
- **Execution Time:** ~30 minutes
- **Run Frequency:** Weekly + before release

---

## Next Steps

**Sprint 25 (Next Sprint):**
1. Create test fixtures for all merchant tiers
2. Write unit tests for fee calculation (all tiers + edge cases)
3. Write unit tests for GTV calculation
4. Write unit tests for tier assignment logic
5. Update integration tests to include tier awareness

**Sprint 26 (Pre-Launch):**
1. Manual testing with finance team validation
2. Performance testing for GTV batch job
3. Production dry-run (GTV calculation without fee enforcement)

---

**See:** [validation-notes/](../validation-notes/) for test execution results after Sprint 25.
