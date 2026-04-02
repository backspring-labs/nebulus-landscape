---
id: cap.interest_accrual_management
type: capability
name: Interest Accrual Management
domain_id: domain.ledger_core
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Interest Accrual Management

## 1. Definition

Interest Accrual Management is the enduring business ability to calculate interest accruals based on configured product economics and authoritative balance states over time, producing accrual records that represent earned but unposted interest.

## 2. Business Outcome

Ensure that the institution accurately calculates interest accruals in accordance with product terms, regulatory requirements, and accounting standards, enabling correct interest posting, accurate financial reporting, and trustworthy customer-facing interest disclosures.

## 3. Scope

### Included activities
- Performing daily interest accrual calculations based on governed accrual methods
- Selecting the correct balance basis (posted, collected, available) for each accrual computation
- Applying configured interest rates, tiers, and day-count conventions
- Recomputing accruals after corrections, backdated adjustments, or rate changes
- Maintaining accrual records with full lineage to balance basis and rate inputs
- Supporting multiple accrual methods (simple, compound, tiered, blended) per product configuration
- Producing accrual outputs suitable for downstream posting and reporting consumption

### Excluded activities
- Interest posting, capitalization, and settlement (`cap.interest_posting_and_settlement_management`)
- Interest rate administration and product pricing (product domain)
- Balance computation and derivation (`cap.balance_computation_management`)
- Customer interest disclosure and statement rendering (channel and account domain)
- Tax withholding on interest (tax domain)

## 4. Upstream Dependencies
- Authoritative balance states from `cap.balance_computation_management` as the basis for accrual calculation
- Interest rate configurations and product economics from account and product domain
- Day-count convention and accrual method definitions from product configuration
- Correction and adjustment events that trigger accrual recomputation

## 5. Downstream Dependencies
- `cap.interest_posting_and_settlement_management` for converting accrued amounts to posted entries
- `cap.balance_computation_management` for including accrued amounts in composite balance views
- Financial reporting capabilities that require accrued interest figures for period-end statements
- Customer-facing interest disclosure and projection capabilities
- Regulatory reporting that requires accrual-basis interest figures

## 6. Related Value Streams
- `vs.interest_economics`
- `vs.accounting_event_processing`
- `vs.financial_truth_creation`

## 7. Related Journeys
- `journey.execute_interest_accrual_cycle`
- `journey.process_end_of_day_close`

## 8. Likely Processes
- Daily interest accrual cycle execution across all eligible accounts
- Balance basis selection and validation for each accrual computation
- Rate and tier application based on product configuration and balance levels
- Day-count convention application for accrual period calculation
- Accrual recomputation triggered by corrections or backdated adjustments
- Accrual correction processing when prior accruals are found to be incorrect
- Accrual record generation and storage for downstream consumption

## 9. Risks
- Wrong balance basis used for accrual, causing interest to be calculated on an incorrect amount
- Wrong day-count treatment applying an incorrect convention, producing systematically incorrect accrual amounts
- Accrual rule drift from configuration where the running accrual logic diverges from the current product terms
- Failure to recompute after correction where a backdated adjustment is posted but accruals are not recalculated
- Rate application errors where the wrong interest rate or tier is applied to an account
- Accrual timing errors where accruals are calculated for incorrect periods

## 10. Controls
- Governed accrual methods with explicit configuration linking each product to its accrual calculation rules
- Explicit balance basis selection ensuring each accrual computation documents which balance type was used
- Date basis control enforcing correct day-count conventions per product and regulatory jurisdiction
- Mandatory accrual recomputation after correction events to prevent stale accrual figures
- Accrual-to-posting lineage ensuring every accrual record can be traced forward to its eventual posting
- Periodic accrual validation comparing computed accruals against independent recalculation

## 11. Typical Systems/Providers

### Typical systems
- Interest accrual engines and batch processors
- Accrual record stores
- Rate application services
- Balance basis services providing authoritative balances for accrual input

### Typical providers
- Temenos
- Thought Machine
- Mambu
- Fiserv
- In-house interest calculation engines

## 12. Open Questions
- Should accrual calculations be performed in real-time (upon each posting) or as a batch process at end-of-day?
- How should the accrual engine handle accounts with multiple interest tiers where the balance crosses tier boundaries during the accrual period?
- What is the correct behavior when a rate change occurs mid-accrual period -- should the period be split or should the new rate apply to the entire period?
- How should accrual recomputation handle situations where the original balance basis is no longer available due to archival?
- Should accrual records be immutable (with corrections creating new offsetting records) or mutable (with corrections updating existing records)?

## 13. Provenance

This capability reflects the fundamental accounting requirement to recognize interest income and expense as it is earned or incurred, rather than only when it is paid or received. In banking, interest accrual is a daily process that depends on accurate balance states, correct rate application, and proper day-count conventions. The accuracy of accrual calculations directly affects financial statements, customer interest payments, and regulatory compliance. Extracting interest accrual as a distinct capability allows the institution to independently govern how accruals are computed, ensure consistency between accrual and posting, and maintain the lineage between accrual inputs (balances, rates, periods) and accrual outputs.
