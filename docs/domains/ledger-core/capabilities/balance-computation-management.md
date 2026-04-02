---
id: cap.balance_computation_management
type: capability
name: Balance Computation Management
domain_id: domain.ledger_core
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Balance Computation Management

## 1. Definition

Balance Computation Management is the enduring business ability to compute authoritative balances from posted entries, including current/posted, available, collected, and accrued balance states, ensuring that all balance types are derived from a single source of posted truth.

## 2. Business Outcome

Ensure that the institution can produce accurate, consistent, and timely balance figures across all required balance types, enabling reliable customer-facing balance presentation, correct interest and fee calculations, accurate regulatory reporting, and trustworthy internal financial management.

## 3. Scope

### Included activities
- Deriving posted/current balances from the sum of committed journal entries
- Computing available balances by applying holds, pending items, and float rules to posted balances
- Calculating collected balances reflecting cleared versus uncollected funds
- Including accrued but unposted components (interest, fees) in appropriate balance views
- Recomputing balances after corrections, reversals, or backdated adjustments
- Maintaining governed balance type definitions with explicit derivation rules
- Preserving balance-to-entry lineage for audit and reconciliation

### Excluded activities
- Journal entry construction and posting execution (`cap.journal_entry_construction_management`, `cap.posting_execution_management`)
- Hold placement and release management (`cap.memo_and_pending_financial_position_management`)
- Interest accrual calculation (`cap.interest_accrual_management`)
- Balance presentation and formatting for customer channels (channel domain)
- Statement generation and cycle management (`domain.accounts`)

## 4. Upstream Dependencies
- Posted journal entries from `cap.posting_execution_management` as the authoritative source of financial truth
- Hold and pending position data from `cap.memo_and_pending_financial_position_management` for available balance computation
- Accrual records from `cap.interest_accrual_management` for accrued balance inclusion
- Account configuration from `domain.accounts` for balance type applicability and derivation rules

## 5. Downstream Dependencies
- Customer-facing balance presentation in digital and branch channels
- Interest accrual and fee assessment capabilities that consume authoritative balances as computation inputs
- Statement and reporting capabilities that present balance snapshots at specific points in time
- Overdraft and protection capabilities that evaluate available balance for decisioning
- Reconciliation capabilities that compare computed balances against independent sources

## 6. Related Value Streams
- `vs.balance_derivation`
- `vs.financial_truth_creation`
- `vs.customer_financial_visibility`

## 7. Related Journeys
- `journey.compute_customer_balance`
- `journey.post_deposit_transaction`
- `journey.process_end_of_day_close`

## 8. Likely Processes
- Posted balance derivation from committed journal entries
- Available balance calculation applying holds, pending items, and float schedules
- Collected balance computation based on clearing status
- Accrued component inclusion in composite balance views
- Balance recomputation triggered by corrections or backdated adjustments
- Balance type definition maintenance and governance
- Balance snapshot capture for statement and reporting purposes

## 9. Risks
- Balance divergence from ledger truth where computed balances do not match the sum of posted entries
- Inconsistent available balance logic where different channels or systems apply different hold and float rules
- Stale balance after correction where a reversal or adjustment is posted but balances are not recomputed
- Incorrect accrued component inclusion where unposted accruals are included in balance types that should reflect only posted activity
- Performance degradation in balance computation under high transaction volumes
- Balance type definition drift where the meaning of a balance type changes without updating all consumers

## 10. Controls
- Governed balance definitions with explicit derivation rules for each balance type
- Repeatable derivation from posted truth ensuring balances can be independently verified from journal entries
- Mandatory recomputation after correction events to prevent stale balances
- Balance-to-entry lineage preserving the ability to decompose any balance into its constituent posted entries
- Balance type consistency checks ensuring all consumers apply the same derivation logic
- Periodic balance integrity validation comparing computed balances against independent recalculation

## 11. Typical Systems/Providers

### Typical systems
- Balance computation engines
- Balance state stores and caches
- Available balance services
- Balance recomputation services

### Typical providers
- Temenos
- Thought Machine
- Mambu
- Fiserv
- In-house balance engines

## 12. Open Questions
- Should balance computation be synchronous (computed inline with each posting) or asynchronous (derived from an entry stream with eventual consistency)?
- How should the system handle balance computation for multi-currency accounts where exchange rates may change between computation requests?
- What is the appropriate caching strategy for frequently accessed balances, and how should cache invalidation be managed after corrections?
- Should balance recomputation after backdated adjustments be immediate or deferred to end-of-day processing?
- How should balance computation handle the transition period when balance type definitions are updated?

## 13. Provenance

This capability reflects the foundational banking requirement to derive trustworthy balance figures from the posted ledger. In banking, balance is not a single number but a family of related computations -- posted, available, collected, accrued -- each serving different business purposes. The accuracy of these computations directly affects customer trust, regulatory compliance, and the correctness of downstream interest and fee calculations. Extracting balance computation as a distinct capability allows the institution to independently govern how balances are derived, ensure consistency across all consumers, and maintain the lineage between computed balances and the posted entries from which they originate.
