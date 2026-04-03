---
id: proc.interest_accrual_cycle_execution
type: process
name: Interest Accrual Cycle Execution
journey_id: journey.execute_interest_accrual_cycle
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
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Interest Accrual Cycle Execution

## 1. Objective

Operationally execute the scheduled interest accrual cycle across eligible deposit accounts by calculating interest based on configured product economics and balance states, recording accrual entries to the book of record, capitalizing accrued interest at period end through interest posting and settlement, routing exceptions for accounts that fail calculation or posting, and completing the cycle with full audit lineage including account counts, totals, and processing evidence.

## 2. Scope

This process covers:
- accrual cycle initiation and eligible account identification
- interest calculation using applicable rates, day-count conventions, and balance bases
- accrual entry construction and recording (debit interest expense / credit interest payable)
- period-end interest capitalization and customer balance posting
- exception handling for accounts with calculation or posting failures
- cycle completion with reconciliation totals and audit trail

This process does **not** own:
- interest rate configuration or product economics definition (`domain.accounts`)
- account status management or dormancy determination (`domain.accounts`)
- batch scheduling and orchestration (`domain.orchestration_control`)
- customer notification of interest credited (`domain.channels_experience`)
- transaction posting mechanics beyond the interest-specific posting (deposit transaction posting process within `domain.ledger_core`)

## 3. Trigger

A scheduled daily accrual cycle fires as part of end-of-day processing, an accrual period closes (monthly statement cycle, quarter-end) triggering interest capitalization and posting, or an operator initiates an ad hoc accrual run for a specific account or portfolio segment.

## 4. Entry Conditions

- Eligible accounts exist with active interest-bearing product configurations.
- Interest rate schedules, day-count conventions, and accrual rules are available from product configuration.
- Balance states required for accrual calculation (average daily balance, minimum balance, tiered balance) are current and available.
- The accrual engine and posting pipeline are operational.
- End-of-day processing prerequisites are satisfied (for scheduled cycles).

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.interest_accrual.cycle_initiation_and_account_selection` | Cycle Initiation and Account Selection | `domain.ledger_core` with `domain.accounts` and `domain.orchestration_control` participation |
| `stage.interest_accrual.interest_calculation` | Interest Calculation | `domain.ledger_core` |
| `stage.interest_accrual.accrual_entry_recording` | Accrual Entry Recording | `domain.ledger_core` |
| `stage.interest_accrual.period_end_capitalization_and_posting` | Period-End Capitalization and Posting | `domain.ledger_core` |
| `stage.interest_accrual.exception_handling` | Exception Handling | `domain.ledger_core` |
| `stage.interest_accrual.audit_trail_and_cycle_completion` | Audit Trail and Cycle Completion | `domain.ledger_core` |

## 6. Stage-by-Stage Flow

### Stage: `stage.interest_accrual.cycle_initiation_and_account_selection`

**Description:** The accrual cycle is initiated either by the scheduled end-of-day trigger or by an operator-initiated ad hoc run. Eligible accounts are identified based on product type, account status, and accrual schedule. Accounts that are closed, dormant without accrual rights, or otherwise ineligible are excluded from the run. The cycle is assigned a unique run identifier and processing parameters are established.

**Capabilities activated:** `cap.end_of_day_and_cycle_processing_management`, `cap.interest_accrual_management`

**Systems involved:** `sys.core_banking_platform`

**Participating domains:** `domain.accounts`, `domain.orchestration_control`

**Decision points:**
- Is this a scheduled cycle or an ad hoc run?
- Which accounts are eligible based on product type, status, and accrual schedule?
- Should dormant accounts with accrual rights be included?
- What is the accrual period being processed (daily, monthly, quarterly)?
- Are there accounts requiring special handling (new accounts, recently closed, rate change mid-period)?

**Risks:** `risk.missing_accounts_in_accrual_run`, `risk.stale_product_economics_configuration`, `risk.incorrect_accrual_period_determination`

**Controls:** `ctrl.account_eligibility_filter_validation`, `ctrl.cycle_run_identifier_assignment`, `ctrl.product_configuration_currency_check`

**Evidence generated:** Cycle run identifier, initiation timestamp, trigger type (scheduled or ad hoc), eligible account count, excluded account count with exclusion reasons, processing parameters

**Handoff:** Eligible accounts identified; moves to interest calculation.

### Stage: `stage.interest_accrual.interest_calculation`

**Description:** For each eligible account, interest is calculated for the accrual period. The applicable rate is determined from product configuration. The day-count convention (actual/360, actual/365, 30/360, etc.) is applied. The balance basis (average daily balance, minimum balance, tiered balance) is selected and the corresponding balance is retrieved. Tiered, blended, or promotional rate logic is applied as configured. The calculated interest amount for each account is produced.

**Capabilities activated:** `cap.interest_accrual_management`, `cap.balance_computation_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- What interest rate applies to this account (base rate, tiered rate, promotional rate)?
- What day-count convention applies?
- What balance basis is used for calculation (average daily, minimum, tiered)?
- Has a rate change occurred mid-period requiring split-period calculation?
- Is the account in a promotional period with special rate treatment?
- Does the calculated amount pass reasonableness checks?

**Risks:** `risk.incorrect_interest_rate_applied`, `risk.day_count_convention_error`, `risk.stale_product_economics_configuration`, `risk.balance_basis_mismatch`

**Controls:** `ctrl.rate_validation_against_product_configuration`, `ctrl.day_count_convention_verification`, `ctrl.calculation_reasonableness_check`, `ctrl.dual_period_calculation_for_rate_changes`

**Evidence generated:** Per-account calculation record including rate applied, day-count convention, balance basis and amount, accrual period dates, calculated interest amount, promotional or tiered rate detail if applicable

**Handoff:** Interest amounts calculated; moves to accrual entry recording.

### Stage: `stage.interest_accrual.accrual_entry_recording`

**Description:** Journal entries representing accrued interest are constructed for each account. The standard accrual accounting treatment is applied (debit interest expense / credit interest payable, or the institution's configured accounting treatment). Entries are posted to the accrual subledger. Running accrual totals are updated per account and in aggregate.

**Capabilities activated:** `cap.journal_entry_construction_management`, `cap.posting_execution_management`, `cap.subledger_account_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- What is the correct accounting treatment for this accrual entry?
- Are accrual entries posted individually per account or as batch aggregates?
- Do running accrual totals reconcile with the sum of individual accrual entries?
- Are there prior-period accrual corrections that need to be incorporated?

**Risks:** `risk.accrual_entry_accounting_error`, `risk.running_total_drift`, `risk.batch_posting_partial_failure`

**Controls:** `ctrl.accrual_entry_balancing_validation`, `ctrl.running_total_reconciliation`, `ctrl.batch_integrity_check`

**Evidence generated:** Accrual journal entries per account, accrual subledger position updates, running accrual totals (per account and aggregate), entry construction and posting timestamps

**Handoff:** Accrual entries recorded; for daily runs the cycle may proceed to completion. For period-end runs, moves to capitalization.

### Stage: `stage.interest_accrual.period_end_capitalization_and_posting`

**Description:** When the accrual period closes, accumulated accrued interest is capitalized — credited to the customer account balance and reversed from the accrual position. Customer-visible posted balance is updated. Statement-ready interest totals are produced. This stage effectively converts accrued (earned but unposted) interest into posted balance that the customer can see and access.

**Capabilities activated:** `cap.interest_posting_and_settlement_management`, `cap.balance_computation_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Is the accrual period closed and ready for capitalization?
- Does the accumulated accrual total match the expected capitalization amount?
- Are there rounding differences between accrual and capitalization that need treatment?
- Should withholding or tax treatment be applied at capitalization?
- Are statement-ready interest totals correctly produced?

**Risks:** `risk.capitalization_mismatch`, `risk.rounding_difference_accumulation`, `risk.tax_withholding_error`, `risk.statement_interest_total_discrepancy`

**Controls:** `ctrl.accrual_total_reconciliation`, `ctrl.capitalization_amount_verification`, `ctrl.rounding_treatment_rules`, `ctrl.statement_total_cross_check`

**Evidence generated:** Capitalization entries per account, accrual position reversal entries, post-capitalization customer balance, statement-ready interest totals, capitalization reconciliation (accrued vs posted), rounding treatment detail if applicable

**Handoff:** Interest capitalized and posted; moves to exception handling review and cycle completion.

### Stage: `stage.interest_accrual.exception_handling`

**Description:** Accounts that fail accrual calculation or posting are routed to exception processing. Common exceptions include missing rate configuration, unavailable balance data, calculation producing unexpected results (e.g., negative interest on standard deposits), and posting failures. Suspense entries may be created for unresolved items. Exception totals are reported for operational review.

**Capabilities activated:** `cap.suspense_and_exception_account_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Is the exception recoverable through automatic retry?
- Should a suspense entry be created for the unresolved amount?
- Does the exception require manual investigation and correction?
- Are exception totals material enough to require escalation?
- Can the cycle complete with exceptions, or must exceptions be resolved first?

**Risks:** `risk.undetected_accrual_failure_for_subset`, `risk.suspense_entry_aging`, `risk.material_exception_not_escalated`

**Controls:** `ctrl.exception_detection_and_routing`, `ctrl.suspense_entry_creation_and_tracking`, `ctrl.exception_materiality_threshold`, `ctrl.exception_escalation_trigger`

**Evidence generated:** Exception records per account with exception type and cause, suspense entries if created, retry attempts and outcomes, exception totals and materiality assessment, escalation records if applicable

**Handoff:** Exceptions routed and documented; moves to audit trail and cycle completion.

### Stage: `stage.interest_accrual.audit_trail_and_cycle_completion`

**Description:** The accrual cycle is finalized with full audit lineage. Cycle completion is recorded with account counts, total interest accrued, total interest posted (if capitalization occurred), exception counts, and processing timestamps. Full lineage from rate selection through calculation to posting is preserved for each account. Reconciliation totals are confirmed.

**Capabilities activated:** `cap.financial_audit_trail_and_lineage_management`, `cap.end_of_day_and_cycle_processing_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Are all account counts and totals reconciled (accounts processed + exceptions = eligible accounts)?
- Does the total interest accrued reconcile with the sum of per-account calculations?
- If capitalization occurred, does the capitalization total reconcile with accrual totals?
- Is the cycle complete or are there unfinished segments requiring rerun?

**Risks:** `risk.incomplete_accrual_audit_trail`, `risk.cycle_completion_without_reconciliation`, `risk.unfinished_cycle_segment`

**Controls:** `ctrl.account_count_and_amount_reconciliation`, `ctrl.cycle_completion_evidence`, `ctrl.immutable_accrual_lineage`

**Evidence generated:** Cycle completion record, total accounts processed, total interest accrued, total interest posted, exception count and totals, processing duration, reconciliation confirmation, per-account lineage references

**Handoff:** Cycle complete. Results available for downstream reporting, statement generation, and period-close processing.

## 7. Decision Points

Key decisions across the process:
- Account eligibility for accrual (product type, status, accrual rights)
- Rate selection (base, tiered, promotional, blended)
- Day-count convention application
- Balance basis selection (average daily, minimum, tiered)
- Split-period calculation for mid-period rate changes
- Daily accrual recording vs period-end capitalization timing
- Rounding treatment at capitalization
- Exception handling: retry, suspense, or manual resolution
- Cycle completeness gate

## 8. Alternate Outcomes

- Ad hoc accrual run for a single account or subset, outside the scheduled cycle
- Rate change mid-period requiring split-period calculation with old and new rates
- Retroactive accrual correction for a prior-period error
- Zero-interest or promotional period producing zero accrual with tracking maintained
- Partial cycle completion with exceptions queued for resolution
- Capitalization deferred due to unresolved exceptions exceeding materiality threshold

## 9. Risks (process-level)

- Incorrect interest rate applied due to stale or misconfigured product economics
- Day-count convention error producing systematic over- or under-accrual
- Missing accounts in accrual run due to incorrect eligibility filtering
- Capitalization mismatch between accrued total and posted amount
- Undetected accrual failure for a subset of accounts
- Retroactive rate changes not properly reflected in prior-period accruals
- Incomplete audit trail for regulatory or finance examination

## 10. Controls (process-level)

- Rate validation against product configuration at calculation time
- Accrual total reconciliation before and after capitalization
- Account count and total amount reconciliation at cycle completion
- Exception detection and routing for calculation or posting failures
- Dual-period calculation logic for mid-period rate changes
- Cycle completion evidence with account-level detail
- Immutable accrual lineage from rate selection through posting

## 11. Evidence / Audit Considerations

This process generates a complete audit trail:
- cycle initiation record with trigger type and processing parameters
- eligible account selection and exclusion reasons
- per-account calculation detail (rate, day-count, balance basis, amount)
- accrual journal entries and subledger position updates
- capitalization entries and accrual position reversals (when period closes)
- exception records with type, cause, and resolution
- cycle completion record with reconciliation totals
- per-account lineage from rate selection through posting

## 12. Systems Involved (summary)

- core banking platform (interest accrual engine, posting engine, balance computation)
- product configuration service (rate schedules, day-count rules)
- batch processing / cycle management platform
- exception management workbench
- general ledger interface
- financial audit trail repository

## 13. Provider Participation

Representative providers often adjacent to this process:
- Temenos, Thought Machine, Mambu, Fiserv, Jack Henry, FIS, Finastra, Oracle (Flexcube)

## 14. Open Questions

- Should daily accrual and period-end capitalization be modeled as two distinct processes or stages within one process? The current model treats them as stages of a single cycle.
- How should promotional or tiered rate structures affect the accrual calculation model — is this purely configuration from Accounts, or does Ledger need rate-tier logic?
- What is the tolerance threshold for accrual/capitalization rounding differences before an exception is raised?
- Should accrual correction for prior periods be a separate process or a variant path here?
- How should tax withholding at capitalization be coordinated — is it an inline step or a separate downstream process?

## 15. Provenance

This process is medium-confidence because while the core accrual and capitalization mechanics are well understood across core banking platforms, the specifics of rate determination logic, day-count convention application, tiered/promotional rate handling, and the boundary between daily accrual and period-end capitalization vary significantly by institution and platform. It exercises the interest accrual and posting capabilities that were placed in Ledger / Core Banking (rather than Accounts) because Ledger performs the financial calculation and posting while Accounts configures the rules. The process is a primary consumer of end-of-day and cycle processing capabilities and is tightly coupled with the deposit transaction posting process for the actual posting mechanics.
