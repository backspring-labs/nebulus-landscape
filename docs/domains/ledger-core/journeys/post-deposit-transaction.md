---
id: journey.post_deposit_transaction
type: journey
name: Post Deposit Transaction
domain_ids:
  - domain.ledger_core
  - domain.accounts
  - domain.payments
  - domain.orchestration_control
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
value_stream_id: vs.deposit_account_servicing
process_id: proc.deposit_transaction_posting_v1
entry_capability_id: cap.ledger_event_accounting_interpretation
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Post Deposit Transaction

## 1. Objective

Translate a financially relevant event — such as a deposit, withdrawal, transfer settlement, fee assessment, interest capitalization, or adjustment — into a balanced, controlled journal entry, post it to the book of record, update authoritative account balances, and preserve full audit lineage. This journey represents the atomic accounting recognition cycle that every financial event must complete to become posted truth.

## 2. Primary Actor

System-initiated posting triggered by an upstream financial event (payment settlement, interest engine, fee engine, or adjustment approval), or an operator performing a manual posting or correction.

## 3. Trigger

- A payment or transfer completes and produces a settlement event requiring accounting recognition.
- The interest accrual engine capitalizes accrued interest for posting.
- The fee engine assesses a fee requiring posting.
- An operator submits an approved manual adjustment, correction, or reversal.
- A previously memo-posted item converts to final posting status.

## 4. Preconditions

- The source event is valid, authorized, and carries sufficient context (account, amount, effective date, accounting date, entry class).
- The target account exists and is in a state that permits posting.
- Product and account configuration necessary for accounting interpretation is available.
- Posting controls and integrity rules are accessible.

## 5. Happy Path

### Stage 1 — Event reception and accounting interpretation
The upstream financial event is received and interpreted into an accounting-relevant posting intent.

**Capabilities activated:** `cap.ledger_event_accounting_interpretation`

The event is validated for completeness. The effective date, accounting date, entry class, and account context are determined. The event is classified (e.g., credit posting, debit posting, memo release, reversal).

### Stage 2 — Journal entry construction
A balanced debit/credit journal entry is constructed from the interpreted posting intent.

**Capabilities activated:** `cap.journal_entry_construction_management`, `cap.double_entry_integrity_control_management`

Entry lines are built with proper account mappings, amounts, and references. Double-entry balancing is validated. Idempotency checks prevent duplicate entry construction from the same source event.

### Stage 3 — Posting execution
The journal entry is applied to the accounting book of record.

**Capabilities activated:** `cap.posting_execution_management`

The entry is posted with timestamps, sequence identifiers, and posting status. Subledger positions are updated. If a memo position existed, it is released or converted.

**Participating neighbor domains:** `domain.accounts` (account state validation)

### Stage 4 — Balance update
Authoritative account balances are recomputed to reflect the new posting.

**Capabilities activated:** `cap.balance_computation_management`, `cap.memo_and_pending_financial_position_management`

Current/posted balance is updated. Available, collected, and accrued balance states are recalculated as applicable. Memo and pending positions are adjusted.

### Stage 5 — Subledger and GL reflection
The posting is reflected in subledger positions and queued for general ledger interface.

**Capabilities activated:** `cap.subledger_account_management`, `cap.general_ledger_interface_management`

Subledger account positions are updated. GL summarization and mapping entries are prepared or queued for batch GL interface processing.

### Stage 6 — Audit trail completion
Full posting lineage is recorded from source event through journal entry to balance effect.

**Capabilities activated:** `cap.financial_audit_trail_and_lineage_management`

The complete chain — source event, interpretation decision, journal entry, posting outcome, and balance effect — is recorded for audit, investigation, and regulatory traceability.

## 6. Alternate Paths

- **Memo posting** — The event is recognized as a memo or pending item rather than a final posting. A memo position is created, and final posting occurs when the item clears or settles.
- **Backdated posting** — The effective date precedes the current accounting date, requiring backdate governance checks and potential balance restatement.
- **Multi-leg posting** — The event produces multiple journal entries (e.g., a transfer producing both a debit and credit across accounts, plus a fee entry).
- **Batch posting** — Multiple events are posted as a batch (e.g., end-of-day settlement file), with batch-level integrity controls.

## 7. Failure / Exception Paths

- Source event fails validation (missing account, invalid amount, unrecognized entry class).
- Double-entry balancing fails — the constructed entry is rejected.
- Idempotency check detects a duplicate event — posting is suppressed.
- Target account is in a state that does not permit posting (closed, frozen, restricted).
- Posting execution fails due to system error — the event is queued for retry or routed to exception handling.
- GL interface mapping fails — the posting succeeds at subledger level but GL reflection is suspended.
- Backdate governance rejects a backdated posting attempt.

## 8. Related Domains

- `domain.ledger_core` — owns the entire posting cycle from event interpretation through journal construction, posting execution, balance update, and audit trail
- `domain.accounts` — provides account context, product configuration, and account state that governs posting eligibility
- `domain.payments` — produces settlement events that are the most common posting triggers
- `domain.orchestration_control` — may coordinate batch posting workflows and retry handling

## 9. Related Capabilities

- `cap.ledger_event_accounting_interpretation`
- `cap.journal_entry_construction_management`
- `cap.double_entry_integrity_control_management`
- `cap.posting_execution_management`
- `cap.balance_computation_management`
- `cap.memo_and_pending_financial_position_management`
- `cap.subledger_account_management`
- `cap.general_ledger_interface_management`
- `cap.financial_audit_trail_and_lineage_management`

## 10. Value Stream Context

This journey sits inside `vs.deposit_account_servicing` as the fundamental accounting recognition event. Every deposit, withdrawal, fee, interest capitalization, and adjustment must pass through this posting cycle to become part of the authoritative financial record. It is the most frequently executed journey in the Ledger / Core Banking domain and the foundation upon which balance truth, reconciliation, and reporting depend.

## 11. Supporting Process Candidates

- `proc.deposit_transaction_posting_v1`
- `proc.event_accounting_interpretation`
- `proc.journal_entry_construction`
- `proc.posting_execution`
- `proc.balance_recomputation`
- `proc.gl_interface_batch`

## 12. Key Risks and Controls Encountered

### Key risks
- Unbalanced journal entry posted to the book of record
- Duplicate posting of the same source event
- Posting to an incorrect account or with incorrect amounts
- Missed posting causing balance divergence from economic reality
- Incorrect effective-date or accounting-date assignment
- GL mapping failure causing subledger/GL divergence
- Incomplete audit trail preventing traceability

### Key controls
- Double-entry balancing validation on every journal entry before posting
- Idempotency enforcement at event reception
- Account state and posting eligibility check before execution
- Posting sequence controls and timestamp integrity
- Backdate governance rules
- GL interface reconciliation controls
- Immutable audit lineage from source event to posting outcome

## 13. Typical Systems and Providers Involved

### Typical systems
- Core banking ledger / posting engine
- Journal entry construction service
- Balance computation engine
- Subledger management platform
- General ledger interface / GL bridge
- Financial audit trail repository
- Event queue / message broker (for event reception)

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

- Should memo-to-final posting conversion be modeled as a distinct journey or a variant path within this journey?
- How should batch posting be handled — as a wrapper around individual posting cycles or as a distinct batch-level journey?
- What is the boundary between posting-time balance computation and query-time balance derivation?
- Should GL interface processing be synchronous with posting or always asynchronous/batched?

## 15. Provenance

This journey represents the atomic accounting recognition cycle in the Ledger / Core Banking domain. It is the most fundamental journey in the domain, exercised by every financial event that must become posted truth. It activates the core posting pipeline capabilities from event interpretation through journal construction, posting execution, balance update, and audit trail. It is the building block upon which interest accrual cycles, fee runs, reconciliation, and period close all depend.
