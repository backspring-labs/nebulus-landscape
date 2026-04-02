---
id: journey.execute_interest_accrual_cycle
type: journey
name: Execute Interest Accrual Cycle
domain_ids:
  - domain.ledger_core
  - domain.accounts
  - domain.orchestration_control
capability_ids:
  - cap.interest_accrual_management
  - cap.interest_posting_and_settlement_management
  - cap.balance_computation_management
  - cap.journal_entry_construction_management
  - cap.posting_execution_management
  - cap.end_of_day_and_cycle_processing_management
  - cap.subledger_account_management
  - cap.financial_audit_trail_and_lineage_management
  - cap.suspense_and_exception_account_management
value_stream_id: vs.deposit_account_servicing
process_id: proc.interest_accrual_cycle_v1
entry_capability_id: cap.interest_accrual_management
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Execute Interest Accrual Cycle

## 1. Objective

Calculate and record interest accrued on eligible deposit accounts based on configured product economics and balance states, and — at the end of the accrual period — capitalize accrued interest into posted balance through interest posting and settlement. This journey ensures that interest is accurately computed on a scheduled basis, that accrual entries maintain accounting integrity, and that capitalization produces correct balance effects.

## 2. Primary Actor

System-initiated scheduled process (daily accrual run, period-end capitalization), or an operator triggering an ad hoc accrual or correction run.

## 3. Trigger

- Scheduled daily accrual cycle fires as part of end-of-day processing.
- Accrual period closes (e.g., monthly statement cycle, quarter-end) triggering interest capitalization and posting.
- Operator initiates an ad hoc accrual run for a specific account or portfolio segment (e.g., correction, catch-up).

## 4. Preconditions

- Eligible accounts exist with active interest-bearing product configurations.
- Interest rate schedules, day-count conventions, and accrual rules are available from product configuration (sourced from Accounts domain).
- Balance states required for accrual calculation (e.g., average daily balance, minimum balance, tiered balance) are available.
- The accrual engine and posting pipeline are operational.

## 5. Happy Path

### Stage 1 — Accrual cycle initiation and account selection
The accrual cycle is initiated and eligible accounts are identified.

**Capabilities activated:** `cap.end_of_day_and_cycle_processing_management`, `cap.interest_accrual_management`

The scheduled accrual cycle fires. Eligible accounts are selected based on product type, account status, and accrual schedule. Accounts that are closed, dormant without accrual rights, or otherwise ineligible are excluded.

**Participating neighbor domains:** `domain.accounts` (product configuration, account eligibility), `domain.orchestration_control` (cycle scheduling)

### Stage 2 — Interest calculation
Interest is calculated for each eligible account based on balance states and configured economics.

**Capabilities activated:** `cap.interest_accrual_management`, `cap.balance_computation_management`

For each account, the applicable rate, day-count convention, and balance basis are determined. Interest is calculated for the accrual period. Tiered, blended, or promotional rate logic is applied as configured.

### Stage 3 — Accrual entry recording
Accrual entries are constructed and recorded to reflect the calculated interest.

**Capabilities activated:** `cap.journal_entry_construction_management`, `cap.posting_execution_management`, `cap.subledger_account_management`

Journal entries representing accrued interest are constructed (debit interest expense / credit interest payable, or the applicable accounting treatment). Entries are posted to the accrual subledger. Running accrual totals are updated.

### Stage 4 — Period-end capitalization and interest posting
When the accrual period closes, accumulated accrued interest is capitalized and posted to customer balances.

**Capabilities activated:** `cap.interest_posting_and_settlement_management`, `cap.balance_computation_management`

Accrued interest is capitalized — credited to the customer account balance and reversed from the accrual position. Customer-visible posted balance is updated. Statement-ready interest totals are produced.

### Stage 5 — Exception handling
Accounts that fail accrual calculation or posting are routed to exception processing.

**Capabilities activated:** `cap.suspense_and_exception_account_management`

Accounts with calculation errors, missing configuration, or posting failures are flagged and routed to exception queues. Suspense entries may be created for unresolved items. Exception totals are reported for operational review.

### Stage 6 — Audit trail and cycle completion
The accrual cycle is finalized with full audit lineage.

**Capabilities activated:** `cap.financial_audit_trail_and_lineage_management`, `cap.end_of_day_and_cycle_processing_management`

Cycle completion is recorded with account counts, total interest accrued, total interest posted, exception counts, and processing timestamps. Full lineage from rate selection through calculation to posting is preserved for each account.

## 6. Alternate Paths

- **Ad hoc accrual run** — An operator triggers accrual for a single account or subset, outside the scheduled cycle, for correction or catch-up purposes.
- **Rate change mid-period** — A rate change takes effect mid-accrual period, requiring split-period calculation with the old rate through the change date and the new rate thereafter.
- **Retroactive accrual correction** — A prior-period accrual error is discovered, requiring recalculation and corrective entries.
- **Zero-interest or promotional period** — Accounts in a zero-rate promotional period are processed but produce zero accrual, with tracking maintained for promotional period expiration.

## 7. Failure / Exception Paths

- Missing or invalid interest rate configuration for an account or product.
- Balance state required for calculation is unavailable or inconsistent.
- Accrual calculation produces an unexpected result (e.g., negative interest on a standard deposit).
- Journal entry construction fails balancing validation.
- Posting execution fails for a subset of accounts.
- Capitalization produces a balance discrepancy versus running accrual totals.
- Cycle processing times out or is interrupted before completion.

## 8. Related Domains

- `domain.ledger_core` — owns the entire accrual cycle from calculation through entry recording, capitalization, posting, exception handling, and audit trail
- `domain.accounts` — provides product configuration, interest rate rules, day-count conventions, and account eligibility context
- `domain.orchestration_control` — may schedule and coordinate accrual cycle execution, retries, and batch management

## 9. Related Capabilities

- `cap.interest_accrual_management`
- `cap.interest_posting_and_settlement_management`
- `cap.balance_computation_management`
- `cap.journal_entry_construction_management`
- `cap.posting_execution_management`
- `cap.end_of_day_and_cycle_processing_management`
- `cap.subledger_account_management`
- `cap.financial_audit_trail_and_lineage_management`
- `cap.suspense_and_exception_account_management`

## 10. Value Stream Context

This journey sits inside `vs.deposit_account_servicing` as the primary interest processing cycle. Interest accrual and capitalization are among the most economically significant recurring processes in deposit banking, directly affecting customer balances, institution interest expense, and regulatory reporting. Accuracy and completeness of this cycle are fundamental to financial integrity.

## 11. Supporting Process Candidates

- `proc.interest_accrual_cycle_v1`
- `proc.daily_accrual_calculation`
- `proc.period_end_interest_capitalization`
- `proc.accrual_exception_handling`
- `proc.interest_rate_determination`
- `proc.accrual_correction_and_adjustment`

## 12. Key Risks and Controls Encountered

### Key risks
- Incorrect interest rate applied due to stale or misconfigured product economics
- Day-count convention error producing systematic over- or under-accrual
- Missing accounts in accrual run due to incorrect eligibility filtering
- Capitalization mismatch between accrued total and posted amount
- Undetected accrual failure for a subset of accounts
- Retroactive rate changes not properly reflected in prior-period accruals
- Incomplete audit trail for regulatory or finance examination

### Key controls
- Rate validation against product configuration at calculation time
- Accrual total reconciliation before and after capitalization
- Account count and total amount reconciliation at cycle completion
- Exception detection and routing for calculation or posting failures
- Dual-period calculation logic for mid-period rate changes
- Cycle completion evidence with account-level detail
- Immutable accrual lineage from rate selection through posting

## 13. Typical Systems and Providers Involved

### Typical systems
- Interest accrual engine
- Core banking ledger / posting engine
- Balance computation engine
- Product configuration service (rate schedules, day-count rules)
- Batch processing / cycle management platform
- Exception management workbench
- Financial audit trail repository

### Representative providers
- Temenos
- Thought Machine
- Mambu
- Fiserv
- Jack Henry
- FIS
- Finastra
- Oracle (Flexcube)

## 14. Open Questions

- Should daily accrual and period-end capitalization be modeled as two distinct journeys or stages within one journey? The current model treats them as stages of a single cycle.
- How should promotional or tiered rate structures affect the accrual calculation model — is this purely configuration from Accounts, or does Ledger need rate-tier logic?
- What is the tolerance threshold for accrual/capitalization rounding differences before an exception is raised?
- Should accrual correction for prior periods be a separate journey (retroactive accrual adjustment) or a variant path here?

## 15. Provenance

This journey represents the core interest processing cycle in the Ledger / Core Banking domain. It exercises the interest accrual and posting capabilities that were explicitly placed in this domain (rather than Accounts) because Ledger performs the financial calculation and posting while Accounts configures the rules. The journey is a primary consumer of end-of-day and cycle processing capabilities and is tightly coupled with the Post Deposit Transaction journey for the actual posting mechanics.
