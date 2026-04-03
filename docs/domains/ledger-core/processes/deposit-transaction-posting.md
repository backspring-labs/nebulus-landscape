---
id: proc.deposit_transaction_posting
type: process
name: Deposit Transaction Posting
journey_id: journey.post_deposit_transaction
capability_ids:
  - cap.ledger_event_accounting_interpretation
  - cap.journal_entry_construction_management
  - cap.double_entry_integrity_control_management
  - cap.posting_execution_management
  - cap.balance_computation_management
  - cap.memo_and_pending_financial_position_management
  - cap.subledger_account_management
  - cap.general_ledger_interface_management
  - cap.financial_audit_trail_and_lineage_management
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Deposit Transaction Posting

## 1. Objective

Operationally execute the controlled sequence required to translate a financially relevant event — such as a deposit, withdrawal, transfer settlement, fee assessment, interest capitalization, or adjustment — into a balanced journal entry, post it to the book of record, update authoritative account balances, reflect positions in the subledger and general ledger, and preserve full audit lineage from the source event through the posting outcome.

## 2. Scope

This process covers:
- reception and validation of an incoming financial event
- accounting interpretation and classification of the event into a posting intent
- journal entry construction with double-entry balancing validation
- idempotency enforcement to prevent duplicate postings
- posting execution against the book of record
- balance recomputation including posted, available, and memo positions
- subledger position update and general ledger interface preparation
- audit trail recording from source event through balance effect

This process does **not** own:
- payment or transfer execution logic (`domain.payments`)
- interest or fee calculation logic (separate accrual and fee processes within `domain.ledger_core`)
- account product configuration or account status management (`domain.accounts`)
- channel presentation of transaction confirmation (`domain.channels_experience`)
- fraud scoring or risk decisioning (`domain.risk_fraud`)

## 3. Trigger

A payment or transfer completes and produces a settlement event, the interest accrual engine capitalizes accrued interest, the fee engine assesses a fee, an operator submits an approved manual adjustment or correction, or a previously memo-posted item converts to final posting status.

## 4. Entry Conditions

- The source event is valid, authorized, and carries sufficient context (account, amount, effective date, accounting date, entry class).
- The target account exists and is in a state that permits posting.
- Product and account configuration necessary for accounting interpretation is available.
- Posting controls and integrity rules are accessible.
- The posting pipeline is operational and accepting events.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.deposit_posting.event_reception_and_accounting_interpretation` | Event Reception and Accounting Interpretation | `domain.ledger_core` |
| `stage.deposit_posting.journal_entry_construction_and_integrity_validation` | Journal Entry Construction and Integrity Validation | `domain.ledger_core` |
| `stage.deposit_posting.posting_execution` | Posting Execution | `domain.ledger_core` with `domain.accounts` participation |
| `stage.deposit_posting.balance_update_and_position_management` | Balance Update and Position Management | `domain.ledger_core` |
| `stage.deposit_posting.subledger_and_gl_reflection` | Subledger and GL Reflection | `domain.ledger_core` |
| `stage.deposit_posting.audit_trail_completion` | Audit Trail Completion | `domain.ledger_core` |

## 6. Stage-by-Stage Flow

### Stage: `stage.deposit_posting.event_reception_and_accounting_interpretation`

**Description:** The upstream financial event is received and validated for completeness. The event is interpreted into an accounting-relevant posting intent by determining the effective date, accounting date, entry class, and account context. The event is classified by type (credit posting, debit posting, memo release, reversal, adjustment).

**Capabilities activated:** `cap.ledger_event_accounting_interpretation`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Is the incoming event complete and well-formed (account, amount, dates, entry class present)?
- What is the accounting classification of this event (debit, credit, memo, reversal)?
- What effective date and accounting date apply?
- Has this event already been received (idempotency check)?
- Does the event require special handling (backdate, multi-leg, batch)?

**Risks:** `risk.duplicate_posting_of_source_event`, `risk.incorrect_effective_or_accounting_date`, `risk.event_misclassification`

**Controls:** `ctrl.idempotency_enforcement_at_event_reception`, `ctrl.event_completeness_validation`, `ctrl.accounting_date_determination_rules`

**Evidence generated:** Event receipt record, event classification outcome, idempotency check result, posting intent with effective date and accounting date, event source reference

**Handoff:** Posting intent established; moves to journal entry construction.

### Stage: `stage.deposit_posting.journal_entry_construction_and_integrity_validation`

**Description:** A balanced debit/credit journal entry is constructed from the interpreted posting intent. Entry lines are built with proper account mappings, amounts, and references. Double-entry balancing is validated before the entry is released for posting. Idempotency checks confirm no duplicate entry has been constructed from the same source event.

**Capabilities activated:** `cap.journal_entry_construction_management`, `cap.double_entry_integrity_control_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Are all entry lines correctly mapped to the appropriate accounts?
- Does the journal entry balance (total debits equal total credits)?
- Is this a single-leg or multi-leg entry?
- Has an entry already been constructed for this source event (duplicate detection)?
- Does the entry require approval before posting (e.g., manual adjustments above threshold)?

**Risks:** `risk.unbalanced_journal_entry_posted`, `risk.posting_to_incorrect_account`, `risk.duplicate_posting_of_source_event`

**Controls:** `ctrl.double_entry_balancing_validation`, `ctrl.account_mapping_verification`, `ctrl.idempotency_enforcement_at_event_reception`

**Evidence generated:** Journal entry record with entry lines, balancing validation result, account mapping references, duplicate check result, entry construction timestamp

**Handoff:** Balanced journal entry ready; moves to posting execution.

### Stage: `stage.deposit_posting.posting_execution`

**Description:** The journal entry is applied to the accounting book of record. The entry is posted with timestamps, sequence identifiers, and posting status. If a memo position existed for this event, it is released or converted to final posting status. Account state is verified to confirm the account permits posting at the moment of execution.

**Capabilities activated:** `cap.posting_execution_management`

**Systems involved:** `sys.core_banking_platform`

**Participating domains:** `domain.accounts`

**Decision points:**
- Is the target account in a state that permits posting (not closed, frozen, or restricted)?
- Is this a final posting or a memo posting?
- Should a previously memo-posted item be released and converted?
- Does the posting respect sequence and timing controls?

**Risks:** `risk.posting_to_ineligible_account_state`, `risk.missed_posting_causing_balance_divergence`, `risk.posting_sequence_violation`

**Controls:** `ctrl.account_state_and_posting_eligibility_check`, `ctrl.posting_sequence_and_timestamp_integrity`, `ctrl.memo_to_final_conversion_control`

**Evidence generated:** Posting confirmation record, posting timestamp, sequence identifier, posting status (final or memo), account state at posting time, memo release record if applicable

**Handoff:** Entry posted to book of record; moves to balance update.

### Stage: `stage.deposit_posting.balance_update_and_position_management`

**Description:** Authoritative account balances are recomputed to reflect the new posting. Current/posted balance is updated. Available, collected, and accrued balance states are recalculated as applicable. Memo and pending positions are adjusted to reflect the new financial state.

**Capabilities activated:** `cap.balance_computation_management`, `cap.memo_and_pending_financial_position_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Which balance types need recomputation (posted, available, collected, accrued)?
- Are there memo or pending positions that need adjustment as a result of this posting?
- Does the balance update trigger any balance-dependent rules (e.g., overdraft threshold, minimum balance)?

**Risks:** `risk.balance_computation_error`, `risk.memo_position_not_adjusted`, `risk.balance_dependent_rule_not_triggered`

**Controls:** `ctrl.balance_recomputation_verification`, `ctrl.memo_position_synchronization`, `ctrl.balance_threshold_evaluation`

**Evidence generated:** Pre-posting balance snapshot, post-posting balance snapshot, balance delta, memo position adjustments, balance type breakdown (posted, available, collected)

**Handoff:** Balances updated; moves to subledger and GL reflection.

### Stage: `stage.deposit_posting.subledger_and_gl_reflection`

**Description:** The posting is reflected in subledger positions and queued for general ledger interface processing. Subledger account positions are updated to reflect the new entry. GL summarization and mapping entries are prepared or queued for batch GL interface processing.

**Capabilities activated:** `cap.subledger_account_management`, `cap.general_ledger_interface_management`

**Systems involved:** `sys.core_banking_platform`, `sys.general_ledger`

**Decision points:**
- Is the GL interface synchronous with posting or batched for later processing?
- Does the GL mapping require summarization or pass-through of individual entries?
- Are there GL mapping rules specific to this entry class or product type?

**Risks:** `risk.gl_mapping_failure_causing_subledger_gl_divergence`, `risk.gl_summarization_error`, `risk.delayed_gl_reflection`

**Controls:** `ctrl.gl_interface_reconciliation_controls`, `ctrl.gl_mapping_rule_validation`, `ctrl.subledger_position_update_confirmation`

**Evidence generated:** Subledger position update record, GL interface queue entry, GL mapping applied, GL summarization detail if applicable

**Handoff:** Positions reflected; moves to audit trail completion.

### Stage: `stage.deposit_posting.audit_trail_completion`

**Description:** Full posting lineage is recorded from source event through journal entry to balance effect. The complete chain — source event, interpretation decision, journal entry, posting outcome, and balance effect — is linked and preserved for audit, investigation, and regulatory traceability.

**Capabilities activated:** `cap.financial_audit_trail_and_lineage_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Is the full lineage chain complete and linked (event to interpretation to entry to posting to balance)?
- Are all required audit fields populated?
- Does the lineage satisfy regulatory traceability requirements?

**Risks:** `risk.incomplete_audit_trail`, `risk.broken_lineage_chain`, `risk.missing_regulatory_audit_fields`

**Controls:** `ctrl.immutable_audit_lineage`, `ctrl.lineage_completeness_validation`, `ctrl.audit_field_population_check`

**Evidence generated:** Complete audit lineage record linking source event, accounting interpretation, journal entry, posting confirmation, balance effect, and GL reflection

**Handoff:** Process complete; the transaction is posted truth in the book of record.

## 7. Decision Points

Key decisions across the process:
- Event classification and accounting interpretation
- Effective date and accounting date determination
- Idempotency — duplicate event detection
- Journal entry balancing validation
- Account state eligibility for posting
- Final posting vs memo posting
- Balance type recomputation scope
- GL interface timing (synchronous vs batched)
- Backdate governance applicability

## 8. Alternate Outcomes

- Memo posting — event recognized as pending rather than final, with memo position created
- Backdated posting — effective date precedes current accounting date, requiring backdate governance
- Multi-leg posting — event produces multiple journal entries across accounts
- Batch posting — multiple events posted as a batch with batch-level integrity controls
- Reversal posting — a prior posting is reversed through a compensating entry
- Posting rejected due to account state — event queued for retry or routed to exception handling

## 9. Risks (process-level)

- Unbalanced journal entry posted to the book of record
- Duplicate posting of the same source event
- Posting to an incorrect account or with incorrect amounts
- Missed posting causing balance divergence from economic reality
- Incorrect effective-date or accounting-date assignment
- GL mapping failure causing subledger/GL divergence
- Incomplete audit trail preventing traceability
- Posting to an account in an ineligible state (closed, frozen, restricted)

## 10. Controls (process-level)

- Double-entry balancing validation on every journal entry before posting
- Idempotency enforcement at event reception to prevent duplicates
- Account state and posting eligibility check before execution
- Posting sequence controls and timestamp integrity
- Backdate governance rules for out-of-period postings
- GL interface reconciliation controls
- Immutable audit lineage from source event to posting outcome

## 11. Evidence / Audit Considerations

This process generates a complete audit trail:
- event receipt and idempotency check result
- accounting interpretation and classification outcome
- journal entry with entry lines and balancing confirmation
- posting confirmation with timestamp and sequence identifier
- pre-posting and post-posting balance snapshots
- subledger position update and GL interface queue entry
- complete lineage chain from source event through balance effect

## 12. Systems Involved (summary)

- core banking platform (posting engine, balance computation, subledger management)
- general ledger / GL interface
- event queue / message broker (for event reception)
- financial audit trail repository

## 13. Provider Participation

Representative providers often adjacent to this process:
- Temenos, Thought Machine, Mambu, Fiserv, Jack Henry, FIS, Finastra, Oracle (Flexcube)

## 14. Open Questions

- Should memo-to-final posting conversion be modeled as a distinct process or a variant path within this process?
- How should batch posting be handled — as a wrapper around individual posting cycles or as a distinct batch-level process?
- What is the boundary between posting-time balance computation and query-time balance derivation?
- Should GL interface processing be synchronous with posting or always asynchronous/batched?
- At what threshold should manual adjustment postings require dual approval before execution?

## 15. Provenance

This process is medium-confidence because while the core posting pipeline is well understood and fundamental to every core banking platform, the specifics of accounting interpretation rules, GL mapping conventions, and memo posting mechanics vary significantly across institutions and platforms. It is the most frequently executed process in the Ledger / Core Banking domain and the foundation upon which balance truth, reconciliation, and reporting depend. It exercises the core posting pipeline capabilities from event interpretation through journal construction, posting execution, balance update, and audit trail. Every other Ledger domain process — interest accrual, fee assessment, reconciliation, period close — ultimately depends on this posting cycle to record financial truth.
