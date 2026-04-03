---
id: proc.reconciliation_break_resolution
type: process
name: Reconciliation Break Resolution
journey_id: journey.resolve_reconciliation_break
capability_ids:
  - cap.financial_reconciliation_management
  - cap.suspense_and_exception_account_management
  - cap.posting_adjustment_and_reversal_management
  - cap.journal_entry_construction_management
  - cap.posting_execution_management
  - cap.balance_computation_management
  - cap.subledger_account_management
  - cap.general_ledger_interface_management
  - cap.financial_audit_trail_and_lineage_management
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Reconciliation Break Resolution

## 1. Objective

Operationally execute the controlled sequence required to detect, investigate, and resolve a reconciliation break — a discrepancy between subledger positions, operational source records, settlement positions, or general ledger accounts — by classifying the break, attributing a root cause, determining the appropriate corrective action, executing the corrective entry, updating affected positions and balances, confirming resolution, and preserving full audit lineage from detection through resolution.

## 2. Scope

This process covers:
- break detection and classification by type, severity, and affected positions
- investigation and root cause attribution using transaction-level detail and posting lineage
- corrective action determination (reposting, reversal, adjustment, reclassification, suspense clearance)
- corrective journal entry construction and posting
- balance and position update after correction
- resolution confirmation through reconciliation re-run
- full audit trail from detection through resolution

This process does **not** own:
- payment settlement logic or settlement position management (`domain.payments`)
- account product configuration or account status management (`domain.accounts`)
- batch scheduling and reconciliation cycle orchestration (`domain.orchestration_control`)
- regulatory reporting of reconciliation status (`domain.reporting_analytics`)
- fraud investigation triggered by reconciliation findings (`domain.risk_fraud`)

## 3. Trigger

An automated reconciliation engine detects a variance between matched positions (subledger vs GL, operational source vs subledger, settlement vs posted), an operator manually identifies a discrepancy during review, a settlement mismatch alert is raised by the payments or settlement layer, or a GL-to-subledger variance exceeds defined tolerance thresholds during period-close or routine reconciliation runs.

## 4. Entry Conditions

- Reconciliation rules and matching criteria are defined for the relevant position pairs.
- The positions being reconciled are available and current (subledger, GL, operational source, settlement).
- The reconciliation engine or manual review process has access to transaction-level detail.
- Operators have authority to investigate breaks and submit corrective actions.
- Approval controls for corrective entries are available and accessible.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.recon_break.break_detection_and_classification` | Break Detection and Classification | `domain.ledger_core` |
| `stage.recon_break.investigation_and_root_cause_attribution` | Investigation and Root Cause Attribution | `domain.ledger_core` with `domain.payments` and `domain.accounts` participation |
| `stage.recon_break.corrective_action_determination` | Corrective Action Determination | `domain.ledger_core` |
| `stage.recon_break.corrective_entry_execution` | Corrective Entry Execution | `domain.ledger_core` |
| `stage.recon_break.balance_and_position_update` | Balance and Position Update | `domain.ledger_core` |
| `stage.recon_break.resolution_confirmation_and_audit_trail` | Resolution Confirmation and Audit Trail | `domain.ledger_core` |

## 6. Stage-by-Stage Flow

### Stage: `stage.recon_break.break_detection_and_classification`

**Description:** The reconciliation break is detected by the automated reconciliation engine (or through manual review) and classified. The break is categorized by type (missing posting, duplicate posting, amount mismatch, timing difference, mapping error, settlement mismatch). Severity is assessed based on amount, age, and affected position type. The break is assigned a unique identifier and queued for investigation.

**Capabilities activated:** `cap.financial_reconciliation_management`

**Systems involved:** `sys.batch_reconciliation_system`, `sys.core_banking_platform`

**Decision points:**
- What type of break is this (missing posting, duplicate, amount mismatch, timing, mapping)?
- What is the severity based on amount and affected position type?
- Is this a new break or a recurrence of a previously resolved break?
- Does the break exceed immediate escalation thresholds?
- Is this potentially a timing difference that may self-resolve?

**Risks:** `risk.breaks_persist_unresolved`, `risk.timing_differences_misclassified`, `risk.break_severity_underestimated`

**Controls:** `ctrl.automated_break_detection_with_tolerance`, `ctrl.break_classification_rules`, `ctrl.immediate_escalation_threshold_check`

**Evidence generated:** Break identifier, detection timestamp, break type and classification, severity assessment, affected position pair, variance amount, detection method (automated or manual)

**Handoff:** Break detected and classified; moves to investigation.

### Stage: `stage.recon_break.investigation_and_root_cause_attribution`

**Description:** The break is investigated to determine the root cause. Transaction-level detail is examined on both sides of the break. Posting lineage is traced from source event through journal entry to balance effect. The root cause is identified — for example, a missed posting, an incorrect account mapping, a timing difference between systems, a duplicate event, or a GL summarization error.

**Capabilities activated:** `cap.financial_reconciliation_management`, `cap.financial_audit_trail_and_lineage_management`

**Systems involved:** `sys.core_banking_platform`, `sys.batch_reconciliation_system`, `sys.settlement_and_reconciliation_system`

**Participating domains:** `domain.payments`, `domain.accounts`

**Decision points:**
- Can the root cause be determined from available transaction-level data?
- Is the break caused by a single event or a pattern of events?
- Does the break originate in the Ledger domain or in an upstream domain (Payments, Accounts)?
- Is this a timing difference that will self-resolve in the next cycle?
- Does investigation reveal a systemic issue requiring broader remediation?
- Is additional information needed from another domain or team?

**Risks:** `risk.incorrect_root_cause_attribution`, `risk.investigation_exceeds_sla`, `risk.systemic_issue_masked_as_isolated_break`

**Controls:** `ctrl.transaction_level_detail_availability`, `ctrl.posting_lineage_trace_capability`, `ctrl.investigation_sla_monitoring`, `ctrl.systemic_pattern_detection`

**Evidence generated:** Investigation record, transaction-level evidence from both sides, posting lineage trace, root cause attribution with supporting evidence, domain of origin, systemic vs isolated classification

**Handoff:** Root cause attributed; moves to corrective action determination.

### Stage: `stage.recon_break.corrective_action_determination`

**Description:** Based on the root cause, the appropriate corrective action is determined. Options include reposting a missed entry, reversing a duplicate, adjusting an incorrect amount, reclassifying a misrouted entry, clearing a suspense item, or correcting a GL mapping. If the break is a timing difference, it may be marked for monitoring rather than immediate correction. Material corrections may require approval before execution.

**Capabilities activated:** `cap.financial_reconciliation_management`, `cap.suspense_and_exception_account_management`

**Systems involved:** `sys.core_banking_platform`, `sys.batch_reconciliation_system`

**Decision points:**
- What corrective action is appropriate for this root cause (repost, reverse, adjust, reclassify, suspense clear)?
- Is the break a timing difference that should be monitored rather than corrected?
- Does the corrective action require maker/checker or senior approval?
- Does the correction amount exceed the operator's authorization limit?
- Will the corrective action affect customer-visible balances?
- Should the break be partially resolved with the residual re-queued?

**Risks:** `risk.incorrect_corrective_action_selected`, `risk.unauthorized_or_unapproved_adjustments`, `risk.customer_impact_from_correction`

**Controls:** `ctrl.corrective_action_appropriateness_review`, `ctrl.maker_checker_for_corrective_entries`, `ctrl.authorization_limit_check`, `ctrl.customer_impact_assessment`

**Evidence generated:** Corrective action decision record, action type selected, approval requirements identified, authorization limit verification, timing difference designation if applicable, partial resolution plan if applicable

**Handoff:** Corrective action determined and approved; moves to corrective entry execution.

### Stage: `stage.recon_break.corrective_entry_execution`

**Description:** The corrective journal entry or adjustment is constructed and posted. The entry is built with proper references to the original break, root cause, and approval records. Double-entry balancing is validated. The entry is posted to the book of record. If the correction involves suspense, the suspense position is cleared. If the correction is a reversal, the compensating entry is linked to the original posting.

**Capabilities activated:** `cap.posting_adjustment_and_reversal_management`, `cap.journal_entry_construction_management`, `cap.posting_execution_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Is the corrective entry balanced and properly referenced?
- Does the corrective entry accurately address the root cause?
- If suspense is involved, is the suspense position fully cleared?
- If this is a reversal, is the compensating entry properly linked?
- Has the required approval been obtained before posting?

**Risks:** `risk.corrective_entry_introduces_new_imbalance`, `risk.corrective_entry_posted_without_approval`, `risk.suspense_not_fully_cleared`

**Controls:** `ctrl.corrective_entry_balancing_validation`, `ctrl.approval_verification_before_posting`, `ctrl.suspense_clearance_completeness_check`, `ctrl.reversal_linkage_validation`

**Evidence generated:** Corrective journal entry with entry lines, balancing validation result, break reference and root cause linkage, approval record reference, suspense clearance record if applicable, reversal linkage record if applicable, posting timestamp

**Handoff:** Corrective entry posted; moves to balance and position update.

### Stage: `stage.recon_break.balance_and_position_update`

**Description:** Account balances and positions are updated to reflect the correction. Subledger positions are adjusted. GL positions are updated through the GL interface. The reconciliation position is refreshed to confirm the positions now agree.

**Capabilities activated:** `cap.balance_computation_management`, `cap.subledger_account_management`, `cap.general_ledger_interface_management`

**Systems involved:** `sys.core_banking_platform`, `sys.general_ledger`

**Decision points:**
- Which balance types need recomputation as a result of the correction?
- Does the GL position update require immediate processing or can it be batched?
- Is the reconciliation position refreshed and does it now show agreement?
- Does the balance change trigger any downstream effects (e.g., overdraft, minimum balance)?

**Risks:** `risk.balance_not_updated_after_correction`, `risk.gl_position_lag_after_correction`, `risk.downstream_balance_effect_not_triggered`

**Controls:** `ctrl.post_correction_balance_recomputation`, `ctrl.subledger_position_update_verification`, `ctrl.gl_position_refresh_confirmation`

**Evidence generated:** Pre-correction and post-correction balance snapshots, subledger position update records, GL position update or queue record, reconciliation position refresh result

**Handoff:** Positions updated; moves to resolution confirmation.

### Stage: `stage.recon_break.resolution_confirmation_and_audit_trail`

**Description:** The reconciliation engine confirms that positions now agree following the correction. The break record is closed with root cause, corrective action taken, resolution timestamp, and operator identity. Full audit lineage connects the break detection, investigation, corrective entry, and resolution confirmation. If the reconciliation re-run reveals the break is not fully resolved, the residual is re-queued for further investigation.

**Capabilities activated:** `cap.financial_reconciliation_management`, `cap.financial_audit_trail_and_lineage_management`

**Systems involved:** `sys.batch_reconciliation_system`, `sys.core_banking_platform`

**Decision points:**
- Does the reconciliation re-run confirm positions now agree?
- Is the break fully resolved or is there a residual?
- Is the full audit lineage complete from detection through resolution?
- Should a post-resolution monitoring period be established?

**Risks:** `risk.incomplete_break_resolution_audit_trail`, `risk.premature_resolution_closure`, `risk.residual_break_not_requeued`

**Controls:** `ctrl.reconciliation_rerun_after_correction`, `ctrl.full_break_resolution_audit_trail`, `ctrl.resolution_completeness_verification`

**Evidence generated:** Reconciliation re-run result confirming agreement, break closure record with root cause and corrective action summary, resolution timestamp, operator identity, full audit lineage chain from detection through resolution, residual re-queue record if applicable

**Handoff:** Break resolved and closed. If residual exists, re-queued for continued investigation.

## 7. Decision Points

Key decisions across the process:
- Break classification by type and severity
- Timing difference vs real break determination
- Root cause attribution (missed posting, duplicate, mismatch, mapping, settlement)
- Corrective action selection (repost, reverse, adjust, reclassify, suspense clear)
- Approval requirement based on amount and correction type
- Full resolution vs partial resolution with residual
- Monitoring vs immediate correction for timing items
- Escalation for material or aged breaks

## 8. Alternate Outcomes

- Timing difference marked for monitoring and self-resolution in next cycle
- Partial resolution with residual amount re-queued for further investigation
- Escalation to senior operations, finance, or audit for review and decision
- Write-off of immaterial or uncollectable break amount per policy with appropriate approvals
- Systemic issue identified triggering broader remediation beyond the individual break
- Cross-domain correction requiring coordinated action with Payments or Accounts

## 9. Risks (process-level)

- Breaks persist unresolved and accumulate, masking larger financial issues
- Incorrect root cause attribution leads to wrong corrective action
- Corrective entry introduces a new imbalance
- Unauthorized or unapproved adjustments
- Timing differences misclassified as real breaks (or vice versa)
- Suspense balances age without resolution
- Incomplete audit trail for break resolution

## 10. Controls (process-level)

- Automated break detection with tolerance thresholds
- Break aging monitoring and escalation triggers
- Maker/checker or approval controls for corrective entries
- Corrective entry balancing validation
- Reconciliation re-run after correction to confirm resolution
- Suspense aging reports and clearance targets
- Full audit trail from detection through resolution
- Segregation of duties between investigation and correction approval

## 11. Evidence / Audit Considerations

This process generates a complete audit trail:
- break detection record with classification, severity, and variance amount
- investigation record with transaction-level evidence and posting lineage
- root cause attribution with supporting evidence
- corrective action decision and approval records
- corrective journal entry with break reference and root cause linkage
- pre-correction and post-correction balance and position snapshots
- reconciliation re-run confirmation of resolution
- break closure record with full lineage chain

## 12. Systems Involved (summary)

- reconciliation engine / matching platform
- core banking platform (posting engine, balance computation, subledger)
- general ledger interface / GL bridge
- suspense management workbench
- settlement and reconciliation system
- case management system (for break investigation tracking)
- financial audit trail repository

## 13. Provider Participation

Representative providers often adjacent to this process:
- Fiserv, SmartStream, Gresham Technologies (Clareti), Temenos, Jack Henry, FIS, Trintech, BlackLine

## 14. Open Questions

- Should timing differences be modeled as a distinct category within this process or as a separate monitoring process?
- What tolerance thresholds should govern when a variance becomes a reportable break versus an acceptable rounding difference?
- How should cross-domain breaks (involving both Payments settlement state and Ledger posted state) be coordinated for investigation and correction?
- Should write-off be a terminal path within this process or a separate process requiring distinct governance?
- At what break age or amount threshold should automatic escalation to senior finance or audit occur?

## 15. Provenance

This process is medium-confidence because while the core reconciliation lifecycle (detect, investigate, correct, confirm) is well understood, the specifics of reconciliation matching rules, tolerance thresholds, break classification taxonomies, and corrective action governance vary significantly by institution, core platform, and the specific position pairs being reconciled. It exercises the reconciliation, suspense management, and adjustment capabilities that are central to the Ledger / Core Banking domain's purpose of maintaining agreement between books. The process is operator-facing rather than customer-facing, reflecting the domain's operational and financial control orientation. It pairs with the period-close process, which depends on reconciliation status as a close prerequisite.
