---
id: journey.close_financial_period
type: journey
name: Close Financial Period
domain_ids:
  - domain.ledger_core
  - domain.accounts
  - domain.orchestration_control
capability_ids:
  - cap.period_close_support_management
  - cap.end_of_day_and_cycle_processing_management
  - cap.financial_reconciliation_management
  - cap.interest_accrual_management
  - cap.interest_posting_and_settlement_management
  - cap.fee_assessment_management
  - cap.fee_posting_and_adjustment_management
  - cap.balance_computation_management
  - cap.subledger_account_management
  - cap.general_ledger_interface_management
  - cap.suspense_and_exception_account_management
  - cap.financial_audit_trail_and_lineage_management
value_stream_id: vs.financial_integrity_and_control
process_id: proc.financial_period_close_v1
entry_capability_id: cap.period_close_support_management
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Close Financial Period

## 1. Objective

Complete the controlled close of a reporting period (month-end, quarter-end, or year-end) by ensuring all outstanding postings are finalized, accruals and fee assessments are completed, reconciliation controls are satisfied, suspense positions are reviewed, period-close evidence is produced, balances are rolled forward, and the closed period is locked against further posting. This journey establishes the authoritative financial record for the period and provides the foundation for downstream reporting, regulatory production, and finance operations.

## 2. Primary Actor

Finance operations team or period-close coordinator, supported by automated cycle processing and reconciliation engines. Senior finance or accounting officers provide close signoff.

## 3. Trigger

- Scheduled period-end date is reached (month-end, quarter-end, year-end).
- An operator manually initiates a close cycle (e.g., for a special reporting period or correction close).

## 4. Preconditions

- All business-day processing for the period is complete (end-of-day cycles have run through the last business day).
- Accrual cycles, fee runs, and settlement processing for the period are substantially complete.
- Reconciliation processes have run and break status is known.
- Period-close policies, signoff requirements, and lock rules are defined.
- Operators with close authority are available for signoff.

## 5. Happy Path

### Stage 1 — Pre-close assessment and outstanding item review
The period-close process is initiated and outstanding items are assessed.

**Capabilities activated:** `cap.period_close_support_management`, `cap.end_of_day_and_cycle_processing_management`

A pre-close checklist is generated identifying: pending postings, incomplete accrual runs, unresolved suspense items, open reconciliation breaks, and any late-arriving transactions. The close coordinator reviews and confirms readiness or initiates remediation.

**Participating neighbor domains:** `domain.orchestration_control` (close workflow coordination)

### Stage 2 — Final accrual and fee completion
Any remaining accruals and fee assessments for the period are finalized.

**Capabilities activated:** `cap.interest_accrual_management`, `cap.interest_posting_and_settlement_management`, `cap.fee_assessment_management`, `cap.fee_posting_and_adjustment_management`

Final interest accrual through period-end is confirmed or run. Period-end fee assessments are completed. Any accrual or fee corrections are posted. Accrual and fee totals are reconciled against expectations.

**Participating neighbor domains:** `domain.accounts` (product configuration for accrual and fee rules)

### Stage 3 — Suspense and exception review
Suspense positions and exception items are reviewed for period-end treatment.

**Capabilities activated:** `cap.suspense_and_exception_account_management`

Suspense balances are reviewed. Items that can be resolved before close are cleared. Remaining suspense items are documented with aging status and expected resolution dates. Exception items requiring period-end treatment (e.g., write-off, reclassification) are processed.

### Stage 4 — Reconciliation confirmation
Reconciliation status is confirmed across all required position pairs.

**Capabilities activated:** `cap.financial_reconciliation_management`, `cap.subledger_account_management`, `cap.general_ledger_interface_management`

Subledger-to-GL reconciliation is confirmed. Operational source-to-subledger reconciliation is confirmed. Settlement position reconciliation is confirmed. Any remaining breaks are documented with materiality assessment and remediation plans.

### Stage 5 — Balance finalization and GL position lock
Period-end balances are finalized and GL positions are locked.

**Capabilities activated:** `cap.balance_computation_management`, `cap.general_ledger_interface_management`, `cap.period_close_support_management`

Final period-end balances are computed and confirmed. GL positions are finalized and reported. The GL interface produces period-end summarization. Positions are prepared for roll-forward.

### Stage 6 — Period lock and roll-forward
The period is locked against further posting and balances are rolled forward.

**Capabilities activated:** `cap.period_close_support_management`, `cap.end_of_day_and_cycle_processing_management`

The closed period is locked — no further postings are permitted without explicit reopen authorization. Opening balances for the new period are established by rolling forward closing balances. The new period is opened for posting.

### Stage 7 — Close evidence and signoff
Period-close evidence is produced and signoff is obtained.

**Capabilities activated:** `cap.financial_audit_trail_and_lineage_management`, `cap.period_close_support_management`

Close evidence package is generated: reconciliation confirmations, suspense position report, break status summary, accrual and fee completion confirmation, balance roll-forward verification, and exception documentation. Authorized signoff is obtained from finance operations and, where required, senior accounting officers.

## 6. Alternate Paths

- **Soft close** — A preliminary close is performed for management reporting, with the period remaining open for a defined adjustment window before hard close and lock.
- **Reopen closed period** — A material error is discovered after close, requiring authorized reopening, correction posting, and re-close with updated evidence.
- **Accelerated close** — Period-close is performed on an accelerated timeline (e.g., for regulatory reporting deadlines), with some reconciliation and evidence steps compressed.
- **Year-end close** — Additional year-end procedures are executed, including annual accrual true-ups, year-end adjustments, and annual regulatory reporting preparation.

## 7. Failure / Exception Paths

- Outstanding postings cannot be completed before the close deadline.
- Accrual or fee runs fail or produce errors that cannot be corrected in time.
- Material reconciliation breaks remain unresolved at close time.
- Suspense balances exceed acceptable thresholds.
- GL interface fails to produce period-end positions.
- Balance roll-forward produces discrepancies.
- Required signoff authority is unavailable.
- Period lock fails due to system error, leaving the period open to inadvertent posting.

## 8. Related Domains

- `domain.ledger_core` — owns the entire period-close cycle from pre-close assessment through accrual completion, reconciliation confirmation, balance finalization, period lock, roll-forward, and close evidence production
- `domain.accounts` — provides product configuration context for accrual and fee finalization
- `domain.orchestration_control` — may coordinate the close workflow, checklist progression, and escalation

## 9. Related Capabilities

- `cap.period_close_support_management`
- `cap.end_of_day_and_cycle_processing_management`
- `cap.financial_reconciliation_management`
- `cap.interest_accrual_management`
- `cap.interest_posting_and_settlement_management`
- `cap.fee_assessment_management`
- `cap.fee_posting_and_adjustment_management`
- `cap.balance_computation_management`
- `cap.subledger_account_management`
- `cap.general_ledger_interface_management`
- `cap.suspense_and_exception_account_management`
- `cap.financial_audit_trail_and_lineage_management`

## 10. Value Stream Context

This journey sits inside `vs.financial_integrity_and_control` as the highest-level financial control event. Period close establishes the authoritative financial record for a reporting period. It is the point at which all posting, accrual, reconciliation, and control activities converge into a signed-off, locked financial position. Downstream regulatory reporting, financial statements, and management information all depend on the integrity of the closed period.

## 11. Supporting Process Candidates

- `proc.financial_period_close_v1`
- `proc.pre_close_readiness_assessment`
- `proc.final_accrual_and_fee_completion`
- `proc.period_end_reconciliation_confirmation`
- `proc.suspense_period_end_review`
- `proc.balance_finalization_and_roll_forward`
- `proc.period_lock_execution`
- `proc.close_evidence_generation`

## 12. Key Risks and Controls Encountered

### Key risks
- Period closed with material unresolved reconciliation breaks
- Incomplete accruals or fee assessments distort period financials
- Posting to a closed period without proper authorization
- Balance roll-forward error creates opening balance discrepancy in new period
- Suspense balances carried forward without adequate review
- Close evidence is incomplete or does not support regulatory examination
- Period lock fails, allowing inadvertent posting to the closed period

### Key controls
- Pre-close checklist and readiness gate
- Accrual and fee completion confirmation before close
- Reconciliation confirmation with break documentation and materiality assessment
- Suspense review and aging threshold enforcement
- Period lock enforcement preventing unauthorized posting
- Balance roll-forward verification (closing balance = opening balance of next period)
- Close evidence package with required signoff
- Reopen controls requiring senior authorization and re-close procedures

## 13. Typical Systems and Providers Involved

### Typical systems
- Core banking ledger / posting engine
- Period-close management platform
- Reconciliation engine / matching platform
- Interest accrual engine
- Fee assessment engine
- General ledger interface / GL bridge
- Suspense management workbench
- Close evidence and signoff workflow
- Regulatory reporting preparation platform

### Representative providers
- Temenos
- Fiserv
- Jack Henry
- FIS
- Finastra
- Oracle (Flexcube)
- SAP (GL and finance close)
- BlackLine (close management)
- Trintech (close management)

## 14. Open Questions

- Should soft close and hard close be modeled as distinct journeys or stages within one journey? The current model treats them as a variant path.
- What is the boundary between Ledger-domain period close and enterprise finance close? The recommended model is that Ledger closes the banking books; enterprise finance may have a broader close process.
- How should period reopen be governed — as a variant path within this journey or a separate controlled journey?
- What level of reconciliation break materiality is acceptable at close, and who defines the threshold?
- Should year-end close be a separate journey given its additional regulatory and reporting requirements?

## 15. Provenance

This journey represents the highest-level financial control event in the Ledger / Core Banking domain. It exercises nearly every capability in the domain, converging accrual, posting, reconciliation, suspense management, and audit trail capabilities into a controlled close cycle. The journey is operator- and finance-facing rather than customer-facing, reflecting its role as an institutional financial control process. Confidence is medium because period-close practices vary significantly by institution size, regulatory environment, and core banking platform capabilities.
