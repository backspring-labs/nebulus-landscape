---
id: proc.ledger_period_close
type: process
name: Ledger Period Close
journey_id: journey.close_financial_period
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
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Ledger Period Close

## 1. Objective

Operationally execute the controlled close of a financial reporting period (month-end, quarter-end, or year-end) by assessing close readiness, finalizing outstanding accruals and fee assessments, reviewing suspense positions, confirming reconciliation status across all required position pairs, finalizing period-end balances, locking the closed period against further posting, rolling forward opening balances for the new period, and producing close evidence with authorized signoff to establish the authoritative financial record.

## 2. Scope

This process covers:
- pre-close assessment and outstanding item identification
- final accrual completion and fee assessment finalization
- suspense position and exception item period-end review
- reconciliation status confirmation across all required position pairs
- period-end balance finalization and GL position lock
- period lock preventing further posting to the closed period
- balance roll-forward establishing opening balances for the new period
- close evidence package generation and authorized signoff

This process does **not** own:
- interest rate or fee rule configuration (`domain.accounts`)
- batch scheduling and orchestration of close workflow steps (`domain.orchestration_control`)
- regulatory report production from closed-period data (`domain.reporting_analytics`)
- customer-facing statement generation (`domain.channels_experience`)
- enterprise finance close beyond the banking books (`domain.finance_operations`)

## 3. Trigger

A scheduled period-end date is reached (month-end, quarter-end, year-end), or an operator manually initiates a close cycle for a special reporting period or correction close.

## 4. Entry Conditions

- All business-day processing for the period is complete (end-of-day cycles have run through the last business day).
- Accrual cycles, fee runs, and settlement processing for the period are substantially complete.
- Reconciliation processes have run and break status is known.
- Period-close policies, signoff requirements, and lock rules are defined.
- Operators with close authority are available for signoff.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.period_close.pre_close_assessment_and_outstanding_item_review` | Pre-Close Assessment and Outstanding Item Review | `domain.ledger_core` with `domain.orchestration_control` participation |
| `stage.period_close.final_accrual_and_fee_completion` | Final Accrual and Fee Completion | `domain.ledger_core` with `domain.accounts` participation |
| `stage.period_close.suspense_and_exception_review` | Suspense and Exception Review | `domain.ledger_core` |
| `stage.period_close.reconciliation_confirmation` | Reconciliation Confirmation | `domain.ledger_core` |
| `stage.period_close.balance_finalization_and_gl_position_lock` | Balance Finalization and GL Position Lock | `domain.ledger_core` |
| `stage.period_close.period_lock_and_roll_forward` | Period Lock and Roll-Forward | `domain.ledger_core` |
| `stage.period_close.close_evidence_and_signoff` | Close Evidence and Signoff | `domain.ledger_core` |

## 6. Stage-by-Stage Flow

### Stage: `stage.period_close.pre_close_assessment_and_outstanding_item_review`

**Description:** The period-close process is initiated and a comprehensive pre-close assessment is performed. A checklist is generated identifying outstanding items: pending postings, incomplete accrual runs, unresolved suspense items, open reconciliation breaks, and late-arriving transactions. The close coordinator reviews the checklist and confirms readiness to proceed or initiates remediation for blocking items.

**Capabilities activated:** `cap.period_close_support_management`, `cap.end_of_day_and_cycle_processing_management`

**Systems involved:** `sys.core_banking_platform`

**Participating domains:** `domain.orchestration_control`

**Decision points:**
- Are all end-of-day cycles for the period complete?
- Are there pending postings that must be finalized before close?
- Are there incomplete accrual or fee runs?
- What is the current reconciliation break status?
- Are there late-arriving transactions that need disposition?
- Is the close readiness gate satisfied, or are there blocking items?

**Risks:** `risk.period_closed_with_outstanding_postings`, `risk.incomplete_cycle_processing`, `risk.late_arriving_transactions_missed`

**Controls:** `ctrl.pre_close_checklist_and_readiness_gate`, `ctrl.end_of_day_cycle_completion_confirmation`, `ctrl.late_transaction_cutoff_enforcement`

**Evidence generated:** Pre-close checklist with item status, outstanding posting inventory, incomplete cycle list, reconciliation break summary, late-arriving transaction disposition, readiness assessment outcome

**Handoff:** Readiness confirmed; moves to final accrual and fee completion.

### Stage: `stage.period_close.final_accrual_and_fee_completion`

**Description:** Any remaining accruals and fee assessments for the period are finalized. Final interest accrual through period-end is confirmed or run. Period-end fee assessments are completed. Any accrual or fee corrections are posted. Accrual and fee totals are reconciled against expectations and prior-period comparisons.

**Capabilities activated:** `cap.interest_accrual_management`, `cap.interest_posting_and_settlement_management`, `cap.fee_assessment_management`, `cap.fee_posting_and_adjustment_management`

**Systems involved:** `sys.core_banking_platform`

**Participating domains:** `domain.accounts`

**Decision points:**
- Has the final accrual cycle for the period been run and completed successfully?
- Are there accrual corrections or catch-up entries needed?
- Have all period-end fee assessments been executed?
- Are fee waiver or reversal decisions pending?
- Do accrual and fee totals reconcile with expectations?
- Are there material variances from prior-period comparisons?

**Risks:** `risk.incomplete_accruals_distort_period_financials`, `risk.fee_assessment_incomplete`, `risk.accrual_correction_missed`

**Controls:** `ctrl.accrual_and_fee_completion_confirmation`, `ctrl.accrual_total_reconciliation_to_expectation`, `ctrl.prior_period_comparison_check`, `ctrl.fee_completion_confirmation`

**Evidence generated:** Final accrual run completion record, fee assessment completion record, accrual and fee totals, reconciliation to expectations, prior-period comparison, correction entries if applicable

**Handoff:** Accruals and fees finalized; moves to suspense and exception review.

### Stage: `stage.period_close.suspense_and_exception_review`

**Description:** Suspense positions and exception items are reviewed for period-end treatment. Items that can be resolved before close are cleared. Remaining suspense items are documented with aging status and expected resolution dates. Exception items requiring period-end treatment (write-off, reclassification) are processed with appropriate approvals.

**Capabilities activated:** `cap.suspense_and_exception_account_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- What is the current suspense balance and item count?
- Which suspense items can be resolved before close?
- Do remaining suspense balances exceed acceptable thresholds?
- Are there exception items requiring write-off or reclassification?
- Do write-off or reclassification decisions have appropriate approval?
- What is the aging profile of remaining suspense items?

**Risks:** `risk.suspense_balances_carried_without_review`, `risk.material_suspense_not_escalated`, `risk.write_off_without_proper_approval`

**Controls:** `ctrl.suspense_review_and_aging_threshold`, `ctrl.suspense_clearance_before_close`, `ctrl.write_off_approval_requirement`, `ctrl.exception_item_disposition_documentation`

**Evidence generated:** Suspense position report with aging detail, items cleared before close, remaining suspense with expected resolution dates, write-off and reclassification records with approvals, exception item disposition summary

**Handoff:** Suspense reviewed and documented; moves to reconciliation confirmation.

### Stage: `stage.period_close.reconciliation_confirmation`

**Description:** Reconciliation status is confirmed across all required position pairs. Subledger-to-GL reconciliation is confirmed. Operational source-to-subledger reconciliation is confirmed. Settlement position reconciliation is confirmed. Any remaining breaks are documented with materiality assessment and remediation plans. The close coordinator confirms reconciliation is satisfactory for close.

**Capabilities activated:** `cap.financial_reconciliation_management`, `cap.subledger_account_management`, `cap.general_ledger_interface_management`

**Systems involved:** `sys.core_banking_platform`, `sys.general_ledger`, `sys.batch_reconciliation_system`

**Decision points:**
- Are all required reconciliation runs complete for the period?
- Do subledger and GL positions agree within tolerance?
- Are there outstanding breaks that are material?
- For remaining breaks, is a materiality assessment and remediation plan documented?
- Is the reconciliation status acceptable for close?
- Should close proceed with known breaks, or must breaks be resolved first?

**Risks:** `risk.period_closed_with_unresolved_breaks`, `risk.material_break_not_disclosed`, `risk.reconciliation_run_incomplete`

**Controls:** `ctrl.reconciliation_confirmation_with_break_documentation`, `ctrl.materiality_assessment_for_remaining_breaks`, `ctrl.reconciliation_completeness_check`

**Evidence generated:** Reconciliation confirmation records for each position pair, break summary with materiality assessments, remediation plans for outstanding breaks, close coordinator reconciliation sign-off

**Handoff:** Reconciliation confirmed; moves to balance finalization.

### Stage: `stage.period_close.balance_finalization_and_gl_position_lock`

**Description:** Period-end balances are finalized and GL positions are locked. Final period-end balances are computed and confirmed across all accounts. GL positions are finalized and reported. The GL interface produces period-end summarization. Positions are prepared for roll-forward to the new period.

**Capabilities activated:** `cap.balance_computation_management`, `cap.general_ledger_interface_management`, `cap.period_close_support_management`

**Systems involved:** `sys.core_banking_platform`, `sys.general_ledger`

**Decision points:**
- Are all account balances finalized and consistent with posted activity?
- Do GL positions match subledger totals?
- Is the GL period-end summarization complete and accurate?
- Are there any balance anomalies requiring investigation before lock?

**Risks:** `risk.balance_finalization_error`, `risk.gl_subledger_mismatch_at_close`, `risk.balance_anomaly_not_investigated`

**Controls:** `ctrl.balance_finalization_verification`, `ctrl.gl_subledger_agreement_confirmation`, `ctrl.balance_anomaly_detection_check`

**Evidence generated:** Period-end balance report, GL position finalization record, GL-to-subledger agreement confirmation, GL period-end summarization, balance anomaly review if applicable

**Handoff:** Balances finalized; moves to period lock and roll-forward.

### Stage: `stage.period_close.period_lock_and_roll_forward`

**Description:** The closed period is locked — no further postings are permitted without explicit reopen authorization. Opening balances for the new period are established by rolling forward closing balances. The new period is opened for posting. The lock mechanism is tested to confirm it is effective.

**Capabilities activated:** `cap.period_close_support_management`, `cap.end_of_day_and_cycle_processing_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Are all prerequisite stages complete and confirmed?
- Is the period lock mechanism ready and tested?
- Do roll-forward opening balances exactly equal closing balances?
- Is the new period ready to accept postings?
- What is the reopen authorization policy if a post-close correction is needed?

**Risks:** `risk.period_lock_failure`, `risk.balance_roll_forward_error`, `risk.new_period_not_opened`, `risk.posting_to_closed_period_without_authorization`

**Controls:** `ctrl.period_lock_enforcement`, `ctrl.balance_roll_forward_verification`, `ctrl.lock_effectiveness_test`, `ctrl.reopen_controls_requiring_senior_authorization`

**Evidence generated:** Period lock confirmation, lock effective timestamp, roll-forward record (closing balance = opening balance of new period), new period open confirmation, lock effectiveness test result

**Handoff:** Period locked and new period opened; moves to close evidence and signoff.

### Stage: `stage.period_close.close_evidence_and_signoff`

**Description:** A close evidence package is generated consolidating all close-related evidence: reconciliation confirmations, suspense position report, break status summary, accrual and fee completion confirmation, balance roll-forward verification, and exception documentation. Authorized signoff is obtained from finance operations and, where required, senior accounting officers. The close is formally completed.

**Capabilities activated:** `cap.financial_audit_trail_and_lineage_management`, `cap.period_close_support_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Is the close evidence package complete with all required components?
- Are all required signatories available and authorized?
- Are there qualifications or exceptions to note in the signoff?
- Does the evidence package satisfy regulatory and audit requirements?

**Risks:** `risk.incomplete_close_evidence`, `risk.signoff_without_review`, `risk.missing_required_signatory`

**Controls:** `ctrl.close_evidence_package_with_signoff`, `ctrl.evidence_completeness_checklist`, `ctrl.signoff_authority_verification`

**Evidence generated:** Close evidence package (reconciliation confirmations, suspense report, break summary, accrual/fee completion confirmation, balance roll-forward verification, exception documentation), signoff records with signatories and timestamps, close completion timestamp, any qualifications or exceptions noted

**Handoff:** Period close complete. Closed-period data available for downstream regulatory reporting, financial statements, and management information.

## 7. Decision Points

Key decisions across the process:
- Close readiness assessment — proceed vs remediate blocking items
- Accrual and fee completeness gate
- Suspense disposition — clear, document, write-off, or reclassify
- Reconciliation status — acceptable for close vs must-resolve breaks
- Balance finalization and GL agreement confirmation
- Period lock activation
- Roll-forward balance verification
- Close evidence completeness and signoff authority

## 8. Alternate Outcomes

- Soft close for management reporting with the period remaining open for an adjustment window before hard close
- Reopen of a closed period to correct a material error, with senior authorization and re-close
- Accelerated close for regulatory reporting deadlines with compressed reconciliation and evidence steps
- Year-end close with additional annual procedures (annual accrual true-ups, year-end adjustments, annual regulatory preparation)
- Close with known breaks documented and accepted at a defined materiality threshold
- Close delayed due to blocking items requiring remediation

## 9. Risks (process-level)

- Period closed with material unresolved reconciliation breaks
- Incomplete accruals or fee assessments distort period financials
- Posting to a closed period without proper authorization
- Balance roll-forward error creates opening balance discrepancy in the new period
- Suspense balances carried forward without adequate review
- Close evidence is incomplete or does not support regulatory examination
- Period lock fails, allowing inadvertent posting to the closed period

## 10. Controls (process-level)

- Pre-close checklist and readiness gate
- Accrual and fee completion confirmation before close
- Reconciliation confirmation with break documentation and materiality assessment
- Suspense review and aging threshold enforcement
- Period lock enforcement preventing unauthorized posting
- Balance roll-forward verification (closing balance = opening balance of next period)
- Close evidence package with required signoff
- Reopen controls requiring senior authorization and re-close procedures

## 11. Evidence / Audit Considerations

This process generates a comprehensive audit trail:
- pre-close checklist and readiness assessment
- accrual and fee completion confirmation with totals
- suspense position report with aging and disposition detail
- reconciliation confirmation records for each position pair
- break summary with materiality assessments and remediation plans
- period-end balance report and GL position finalization
- GL-to-subledger agreement confirmation
- period lock confirmation and lock effectiveness test
- balance roll-forward verification
- close evidence package with all components
- signoff records with authority verification

## 12. Systems Involved (summary)

- core banking platform (posting engine, balance computation, period management)
- general ledger / GL interface
- reconciliation engine / matching platform
- interest accrual engine
- fee assessment engine
- suspense management workbench
- period-close management platform / close evidence workflow
- regulatory reporting preparation platform

## 13. Provider Participation

Representative providers often adjacent to this process:
- Temenos, Fiserv, Jack Henry, FIS, Finastra, Oracle (Flexcube), SAP (GL and finance close), BlackLine, Trintech

## 14. Open Questions

- Should soft close and hard close be modeled as distinct processes or variant paths within one process? The current model treats them as a variant path.
- What is the boundary between Ledger-domain period close and enterprise finance close? The recommended model is that Ledger closes the banking books; enterprise finance may have a broader close process.
- How should period reopen be governed — as a variant path within this process or a separate controlled process requiring distinct governance?
- What level of reconciliation break materiality is acceptable at close, and who defines the threshold?
- Should year-end close be a separate process given its additional regulatory and reporting requirements?

## 15. Provenance

This process is medium-confidence because while the core period-close lifecycle (assess, finalize, reconcile, lock, evidence, sign off) is well understood, the specifics of close checklist content, reconciliation materiality thresholds, signoff hierarchies, soft-close vs hard-close governance, and reopen controls vary significantly by institution size, regulatory environment, and core banking platform capabilities. It exercises nearly every capability in the Ledger / Core Banking domain, converging accrual, fee, posting, reconciliation, suspense, and audit trail capabilities into a controlled close cycle. The process is operator- and finance-facing rather than customer-facing, reflecting its role as the highest-level institutional financial control event in the domain. It depends on the completion of the interest accrual cycle, deposit transaction posting, and reconciliation break resolution processes as close prerequisites.
