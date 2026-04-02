---
id: journey.resolve_reconciliation_break
type: journey
name: Resolve Reconciliation Break
domain_ids:
  - domain.ledger_core
  - domain.accounts
  - domain.payments
  - domain.orchestration_control
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
value_stream_id: vs.financial_integrity_and_control
process_id: proc.reconciliation_break_resolution_v1
entry_capability_id: cap.financial_reconciliation_management
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Resolve Reconciliation Break

## 1. Objective

Detect, investigate, and resolve a reconciliation break — a discrepancy between subledger positions, operational source records, settlement positions, or general ledger accounts — by identifying the root cause and executing the appropriate corrective action (adjustment, reposting, reclassification, or suspense clearance) to restore agreement between books. This journey ensures that financial integrity is maintained and that breaks do not persist unresolved.

## 2. Primary Actor

Operations analyst or reconciliation specialist investigating and resolving the break, supported by automated reconciliation engines that detect and classify discrepancies.

## 3. Trigger

- Automated reconciliation engine detects a variance between matched positions (e.g., subledger vs. GL, operational source vs. subledger, settlement position vs. posted position).
- An operator manually identifies a discrepancy during review or investigation.
- A settlement mismatch alert is raised by the payments or settlement processing layer.
- GL-to-subledger variance exceeds defined tolerance thresholds during period-close or routine reconciliation runs.

## 4. Preconditions

- Reconciliation rules and matching criteria are defined for the relevant position pairs.
- The positions being reconciled are available and current (subledger, GL, operational source, settlement).
- The reconciliation engine or manual review process has access to transaction-level detail.
- Operators have authority to investigate breaks and submit corrective actions.

## 5. Happy Path

### Stage 1 — Break detection and classification
The reconciliation break is detected and classified by type, severity, and affected positions.

**Capabilities activated:** `cap.financial_reconciliation_management`

The automated reconciliation engine (or manual review) identifies a variance. The break is classified by type (e.g., missing posting, duplicate posting, amount mismatch, timing difference, mapping error). Severity is assessed based on amount, age, and affected position type.

### Stage 2 — Investigation and root cause attribution
The break is investigated to determine the root cause.

**Capabilities activated:** `cap.financial_reconciliation_management`, `cap.financial_audit_trail_and_lineage_management`

Transaction-level detail is examined on both sides of the break. Posting lineage is traced from source event through journal entry to balance effect. The root cause is identified (e.g., missed posting, incorrect account mapping, timing difference between systems, duplicate event, GL summarization error).

**Participating neighbor domains:** `domain.payments` (settlement position detail), `domain.accounts` (account and product context)

### Stage 3 — Corrective action determination
The appropriate corrective action is determined based on root cause.

**Capabilities activated:** `cap.financial_reconciliation_management`, `cap.suspense_and_exception_account_management`

Based on root cause, the resolution path is selected: reposting a missed entry, reversing a duplicate, adjusting an incorrect amount, reclassifying a misrouted entry, clearing a suspense item, or correcting a GL mapping. If the break is a timing difference, it may be marked for monitoring rather than immediate correction.

### Stage 4 — Corrective entry execution
The corrective journal entry or adjustment is constructed and posted.

**Capabilities activated:** `cap.posting_adjustment_and_reversal_management`, `cap.journal_entry_construction_management`, `cap.posting_execution_management`

The corrective entry is constructed with proper references to the original break and root cause. The entry is posted to the book of record. If the correction involves suspense, the suspense position is cleared.

### Stage 5 — Balance and position update
Balances and positions are updated to reflect the correction.

**Capabilities activated:** `cap.balance_computation_management`, `cap.subledger_account_management`, `cap.general_ledger_interface_management`

Account balances are recomputed. Subledger and GL positions are updated. The reconciliation position is refreshed to confirm the break is resolved.

### Stage 6 — Resolution confirmation and audit trail
The break resolution is confirmed and fully documented.

**Capabilities activated:** `cap.financial_reconciliation_management`, `cap.financial_audit_trail_and_lineage_management`

The reconciliation engine confirms positions now agree. The break record is closed with root cause, corrective action taken, resolution timestamp, and operator identity. Full audit lineage connects the break detection, investigation, corrective entry, and resolution confirmation.

## 6. Alternate Paths

- **Timing difference** — The break is attributed to a timing difference between systems (e.g., a posting that will settle in the next cycle). The break is marked as a timing item and monitored for automatic resolution.
- **Partial resolution** — The break is partially explained, with a residual amount remaining. The resolved portion is closed and the residual is re-queued for further investigation.
- **Escalation** — The break exceeds severity or age thresholds and is escalated to senior operations, finance, or audit for review and decision.
- **Write-off** — After investigation, the break amount is determined to be immaterial or uncollectable and is written off per policy, with appropriate approvals.

## 7. Failure / Exception Paths

- Root cause cannot be determined from available data — the break ages and escalates.
- Corrective entry fails validation or posting.
- Corrective action creates a secondary break or imbalance.
- Operator lacks authority for the required corrective action (e.g., amount exceeds approval limit).
- The break involves positions across multiple domains and requires coordinated correction.
- GL interface lag prevents timely confirmation of resolution.
- Suspense clearance is blocked by a dependency on an upstream process.

## 8. Related Domains

- `domain.ledger_core` — owns reconciliation, break detection, investigation, corrective posting, suspense management, and resolution confirmation
- `domain.accounts` — provides account and product context for investigation; may be the source of account-level discrepancies
- `domain.payments` — provides settlement position detail; settlement mismatches are a common break source
- `domain.orchestration_control` — may coordinate reconciliation batch scheduling and escalation workflows

## 9. Related Capabilities

- `cap.financial_reconciliation_management`
- `cap.suspense_and_exception_account_management`
- `cap.posting_adjustment_and_reversal_management`
- `cap.journal_entry_construction_management`
- `cap.posting_execution_management`
- `cap.balance_computation_management`
- `cap.subledger_account_management`
- `cap.general_ledger_interface_management`
- `cap.financial_audit_trail_and_lineage_management`

## 10. Value Stream Context

This journey sits inside `vs.financial_integrity_and_control` as a primary financial integrity mechanism. Reconciliation breaks are inevitable in complex banking operations; the ability to detect, investigate, and resolve them quickly and accurately is a core measure of operational and financial soundness. Unresolved breaks accumulate risk and erode trust in balance truth.

## 11. Supporting Process Candidates

- `proc.reconciliation_break_resolution_v1`
- `proc.automated_reconciliation_matching`
- `proc.break_investigation_and_attribution`
- `proc.corrective_entry_construction`
- `proc.suspense_clearance`
- `proc.break_escalation`
- `proc.reconciliation_position_refresh`

## 12. Key Risks and Controls Encountered

### Key risks
- Breaks persist unresolved and accumulate, masking larger financial issues
- Incorrect root cause attribution leads to wrong corrective action
- Corrective entry introduces a new imbalance
- Unauthorized or unapproved adjustments
- Timing differences misclassified as real breaks (or vice versa)
- Suspense balances age without resolution
- Incomplete audit trail for break resolution

### Key controls
- Automated break detection with tolerance thresholds
- Break aging monitoring and escalation triggers
- Maker/checker or approval controls for corrective entries
- Corrective entry balancing validation
- Reconciliation re-run after correction to confirm resolution
- Suspense aging reports and clearance targets
- Full audit trail from detection through resolution
- Segregation of duties between investigation and correction approval

## 13. Typical Systems and Providers Involved

### Typical systems
- Reconciliation engine / matching platform
- Core banking ledger / posting engine
- Suspense management workbench
- Case management system (for break investigation tracking)
- General ledger interface / GL bridge
- Financial audit trail repository
- Operational data store (source transaction detail)

### Representative providers
- Fiserv (reconciliation modules)
- SmartStream
- Gresham Technologies (Clareti)
- Temenos
- Jack Henry
- FIS
- Trintech
- BlackLine

## 14. Open Questions

- Should timing differences be modeled as a distinct category within this journey or as a separate monitoring process?
- What tolerance thresholds should govern when a variance becomes a reportable break versus an acceptable rounding difference?
- How should cross-domain breaks (e.g., involving both Payments settlement state and Ledger posted state) be coordinated?
- Should the reconciliation break resolution journey include write-off as a terminal path, or should write-off be a separate journey?

## 15. Provenance

This journey represents the primary financial integrity and control mechanism in the Ledger / Core Banking domain. It exercises reconciliation, suspense management, and adjustment capabilities that are central to the domain's purpose of maintaining agreement between books. The journey is operator-facing rather than customer-facing, reflecting the domain's operational and control orientation. Confidence is medium because reconciliation practices vary significantly by institution, core platform, and the specific positions being reconciled.
