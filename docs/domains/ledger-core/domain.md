---
id: domain.ledger_core
type: domain
name: Ledger / Core Banking
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Ledger / Core Banking

## 1. Definition

Ledger / Core Banking is the business domain that owns the **financial book of record** for deposit and transaction banking activity. It is responsible for translating economically meaningful events into accounting entries, preserving double-entry integrity, computing authoritative balances, and maintaining the traceable financial history from which account-level and institution-level balances are derived.

In this landscape, the domain is intentionally centered on the **ledger and accounting core**, not on every capability sometimes marketed as a "core banking platform." The domain owns the posting and balance truth that downstream servicing, statements, regulatory reporting, treasury, finance, and product-visible balance experiences ultimately depend on.

The domain answers questions such as:

- What journal entries were posted?
- What balances are financially authoritative?
- What accounting date and effective date govern a transaction?
- What was accrued, assessed, reversed, adjusted, or settled?
- What reconciliations prove that subledgers, product balances, and general ledger positions agree?

It is the domain of **posted truth**, **balance derivation**, **financial integrity**, and **accounting control**.

### Domain position on "Core Banking"

For this landscape, "Ledger / Core Banking" is modeled as the **accounting core** rather than an umbrella for every operational banking function:

- **Ledger** = the accounting and balance truth layer
- **Core Banking** = the durable financial processing layer immediately surrounding that ledger truth, including accrual, fee posting, cycle processing, subledger management, and general-ledger alignment

This domain includes journal posting, subledger management, balance computation, interest accrual/posting, fee posting, end-of-day and cycle processing, and reconciliation. It does not absorb product configuration, payment execution, customer identity, channel presentation, or orchestration.

## 2. Business Purpose

Ensure that every financially relevant event is represented as complete, controlled, traceable accounting truth.

Without a coherent Ledger / Core Banking domain:

- posted balances cannot be trusted by customers, operations, or regulators,
- interest and fee calculations lack an authoritative execution layer,
- reconciliation between operational systems and accounting books becomes fragile,
- financial close and reporting processes lack reliable inputs,
- disputes and investigations cannot trace financial effects back to source events,
- the institution cannot demonstrate financial integrity under examination.

This domain matters because a bank can continue briefly with imperfect channel experience, orchestration, or analytics. It cannot remain sound if it loses control of what was posted, how balances were derived, or whether books reconcile.

## 3. Scope

### Includes

The Ledger / Core Banking domain includes capabilities required to:

- interpret upstream financial events into accounting-relevant posting intents,
- construct balanced debit/credit journal entries,
- enforce double-entry integrity and posting controls,
- execute authorized postings to the book of record,
- handle corrections, reversals, adjustments, and reclasses,
- maintain customer-facing and product-facing financial subledgers,
- interface with enterprise general ledger structures,
- compute authoritative balances (current/posted, available, collected, accrued),
- maintain memo and pending financial positions,
- calculate and post interest accruals,
- assess and post fees,
- execute end-of-day, cycle-close, and period-close accounting processes,
- reconcile subledgers, operational sources, and general ledger positions,
- manage suspense and exception accounting items,
- preserve financial audit trail and posting lineage.

### Excludes

- **Deposit account contract, product rules, account lifecycle** → `domain.accounts`
  - Accounts configures product economics; Ledger posts the accounting effect.
- **Money movement execution, payment rail behavior, settlement messaging** → `domain.payments`
  - Payments executes movement; Ledger posts the accounting recognition.
- **Fraud, AML, sanctions decisioning** → `domain.risk_fraud`
  - Risk & Fraud decides; Ledger records the financial consequence.
- **Customer relationship and party identity** → `domain.customer`
- **Authentication, authorization, session trust** → `domain.identity_access`
- **Workflow orchestration infrastructure** → `domain.orchestration_control`
  - Ledger owns accounting logic within cycles; Orchestration may schedule and coordinate.
- **Channel presentation of balances and history** → `domain.channels_experience`
  - Channels display; Ledger provides authoritative truth.
- **Case management, complaints, disputes** → `domain.servicing`
  - Servicing initiates; Ledger posts the adjustment.
- **Card authorization and processing** → `domain.cards`
- **Network/scheme rule ownership** → `domain.networks_schemes`
- **Analytics, rewards, loyalty** → `domain.data_rewards`

### Boundary recommendations

- **Accounts vs Ledger:** Accounts owns "what product relationship exists and what are its rules?" Ledger owns "what entries were posted and how are balances financially derived?" This boundary is already established and should remain explicit.
- **Payments vs Ledger:** Payments owns movement execution state. Ledger owns the accounting effect of completed or recognized movement. Settlement execution state belongs to Payments; settlement accounting recognition belongs to Ledger.
- **Interest:** Configuration of interest rules belongs to Accounts (`cap.account_interest_and_fee_configuration_management`). Accrual and posting of interest belongs to Ledger.
- **Fees:** Configuration of fee rules belongs to Accounts. Assessment and posting of fees belongs to Ledger.
- **Balance truth:** Ledger owns authoritative posted/current balance. Memo/pending positions are managed by Ledger where they affect financial state. Execution-oriented availability signals remain in Payments.

## 4. Included Capabilities

The following capabilities are proposed as the durable capability set for `domain.ledger_core`.

- `cap.ledger_event_accounting_interpretation` — Interpret upstream financial events into accounting-relevant posting intents, including effective date, accounting date, product/account context, and entry class.
- `cap.journal_entry_construction_management` — Construct balanced debit/credit journal entries, including entry lines, account mappings, references, and posting metadata.
- `cap.double_entry_integrity_control_management` — Enforce balancing, posting completeness, posting sequence integrity, idempotency, and prevention/detection of malformed financial entries.
- `cap.posting_execution_management` — Apply authorized journal entries to the accounting book of record and record posting status, timestamps, references, and outcomes.
- `cap.posting_adjustment_and_reversal_management` — Handle corrections, reversals, reversals of reversals, reclasses, adjustments, and compensating entries while preserving audit traceability.
- `cap.subledger_account_management` — Maintain customer-facing and product-facing financial subledgers, including posting positions and transaction lineage beneath institution-level books.
- `cap.general_ledger_interface_management` — Maintain the controlled relationship between banking subledgers and enterprise general ledger structures, including summarization, mapping, and posting handoff.
- `cap.balance_computation_management` — Compute authoritative balances from posted entries, including current/posted, available, collected, and accrued states as applicable.
- `cap.memo_and_pending_financial_position_management` — Maintain memo, pending, and pre-posted financial states that influence spendability or visibility before final accounting recognition.
- `cap.interest_accrual_management` — Calculate interest accruals based on configured product economics and balance states over time.
- `cap.interest_posting_and_settlement_management` — Post accrued interest, capitalize where appropriate, and handle corrections, reversals, and settlement impacts of interest processing.
- `cap.fee_assessment_management` — Calculate and assess fees based on configured product terms, event triggers, schedules, or threshold conditions.
- `cap.fee_posting_and_adjustment_management` — Post fees, waivers, refunds, rebates, and associated adjustments into the accounting record.
- `cap.end_of_day_and_cycle_processing_management` — Execute accounting cycle events such as day close, statement-cycle close, accrual roll-forward, fee runs, dormancy treatment hooks, and posting windows.
- `cap.financial_reconciliation_management` — Reconcile subledgers, source operations, suspense positions, settlement positions, and general ledger accounts to ensure books agree.
- `cap.suspense_and_exception_account_management` — Manage unresolved financial items, suspense balances, breaks, exception queues, and aged unresolved accounting conditions.
- `cap.financial_audit_trail_and_lineage_management` — Preserve full traceability from source event to posting decision to journal outcome, balances, and downstream reporting outputs.
- `cap.period_close_support_management` — Support financial close, rollups, and control evidence needed for reporting periods, finance operations, and regulatory production.

### Capability notes

Key placement decisions:
- `cap.interest_accrual_management` and `cap.interest_posting_and_settlement_management` belong here because Accounts configures the rules but Ledger performs the financial calculation and posting.
- `cap.fee_assessment_management` and `cap.fee_posting_and_adjustment_management` belong here for the same reason — Accounts configures, Ledger executes.
- `cap.memo_and_pending_financial_position_management` belongs here because memo positions affect financial truth even before final posting.
- `cap.end_of_day_and_cycle_processing_management` owns the accounting logic within cycles; workflow scheduling may later be coordinated by `domain.orchestration_control`.

## 5. Excluded Capabilities

- `cap.account_interest_and_fee_configuration_management` → `domain.accounts` — configuration of economic rules, not execution
- `cap.account_terms_and_feature_management` → `domain.accounts` — product terms, not accounting truth
- `cap.account_status_and_restriction_management` → `domain.accounts` — account lifecycle state, not ledger state
- `cap.payment_status_and_tracking` → `domain.payments` — movement execution state, not posted accounting state
- `cap.funds_availability_and_release_management` → `domain.payments` — execution-oriented availability, not balance truth
- `cap.internal_transfer_execution` / `cap.ach_payment_execution` / `cap.wire_transfer_execution` / `cap.instant_payment_execution` → `domain.payments` — movement execution, not posting
- `cap.transaction_monitoring` → `domain.risk_fraud` — surveillance, not accounting
- `cap.case_investigation_management` → `domain.risk_fraud` — investigation, not posting

## 6. Upstream Dependencies

The Ledger / Core Banking domain typically depends on:

- `domain.accounts`
  - product/account context, economic rule configuration (rates, fees, schedules), account lifecycle state that governs posting behavior
- `domain.payments`
  - movement execution events that trigger posting (completed transfers, ACH settlements, wire completions, instant payments)
- `domain.risk_fraud`
  - decisions that produce financial consequences (holds, losses, recoveries, adjustments)
- `domain.orchestration_control`
  - may schedule and coordinate end-of-day, cycle, and batch processing workflows
- `domain.servicing`
  - may initiate adjustments, corrections, or dispute-related financial actions

## 7. Downstream Dependencies

The Ledger / Core Banking domain commonly provides data and financial truth to:

- `domain.accounts`
  - product-visible balances and transaction history consumed for customer-facing account context
- `domain.channels_experience`
  - authoritative balances and posted transaction history for display
- `domain.servicing`
  - financial evidence for disputes, inquiries, and case resolution
- `domain.data_rewards`
  - posted financial data for analytics, segmentation, and reporting
- `domain.risk_fraud`
  - financial activity data for monitoring and investigation
- enterprise finance, treasury, and regulatory reporting functions
  - GL positions, reconciliation evidence, and period-close outputs

## 8. Related Journeys

Journeys that pass through or materially depend on the Ledger / Core Banking domain include:

- `journey.post_deposit_transaction`
- `journey.execute_interest_accrual_cycle`
- `journey.execute_monthly_fee_assessment`
- `journey.process_end_of_day_close`
- `journey.resolve_reconciliation_break`
- `journey.post_payment_settlement`
- `journey.reverse_erroneous_posting`
- `journey.close_financial_period`
- `journey.resolve_suspense_item`
- `journey.compute_customer_balance`

Many of these are system/operator journeys rather than customer-facing. Ledger is heavily embedded in journeys owned by Payments, Accounts, and Servicing rather than owning many customer-facing journeys directly.

## 9. Typical Risks and Controls

### Typical Risks

- Unbalanced journal entries created or posted
- Duplicate posting of the same financial event
- Missed posting causes balance and reconciliation errors
- Incorrect effective-date or accounting-date treatment
- Broken mapping between source event and account structure
- Incorrect balance derivation
- Incorrect interest accrual or posting
- Incorrect fee assessment or posting
- Reconciliation breaks between subledger and general ledger
- Unresolved suspense balance accumulation
- Unauthorized manual adjustment
- Inadequate audit lineage for finance, controls, or regulators

### Typical Controls

- Double-entry balancing validation on every journal entry
- Idempotency and duplicate-post prevention
- Maker/checker or approval controls for sensitive adjustments
- Posting sequence controls
- Effective-date and backdate governance
- Suspense monitoring and aging controls
- Subledger-to-GL reconciliation controls
- Independent review of breaks and exceptions
- Close-process signoff controls
- Immutable or strongly controlled posting lineage
- Restricted access to high-risk financial functions
- Evidence generation for audit and regulatory reporting

## 10. Typical Systems and Providers

### Typical system categories

- core banking ledger / posting engine
- subledger management platform
- general ledger interface / GL bridge
- balance computation engine
- interest accrual engine
- fee assessment and posting engine
- end-of-day / batch processing platform
- reconciliation engine
- suspense management workbench
- financial audit trail / lineage repository

### Representative providers often seen around this domain

- `prov.temenos`
- `prov.thought_machine`
- `prov.mambu`
- `prov.fiserv`
- `prov.jack_henry`
- `prov.finastra`
- `prov.oracle_flexcube`
- `prov.fis`
- `prov.sap`

Provider placement should remain careful: many core banking vendors market themselves as owning the entire banking stack. In the canonical model, providers should map to the capabilities they actually support without redefining domain ownership.

## 11. Open Questions

- How far should general ledger ownership extend? The recommended model is that Ledger / Core Banking owns the subledger book of record and the banking-side GL interface, while enterprise finance may own broader external financial reporting structures.
- How should memo, available, and collected balances be represented? The recommended approach keeps authoritative posted/current balance derivation in Ledger, allows pending/memo treatment where spendability is affected, and models available/collected states as controlled computations.
- How much of end-of-day processing belongs here versus Orchestration & Control? Ledger should own accounting logic within cycles; Orchestration may own workflow scheduling.
- Should all settlement-related accounting live here? Yes for accounting recognition; no for rail-specific settlement workflow state.
- Should this domain include loan accounting later? The current framing fits deposit and transaction banking. Lending may later require subdomaining or a broader product-neutral ledger treatment.

## 12. Provenance

This artifact defines Ledger / Core Banking as the accounting core of the banking landscape, intentionally scoped to posting truth, balance computation, and financial integrity rather than the broader "core banking platform" marketing umbrella. It resolves the previously established Accounts/Ledger boundary (Accounts configures, Ledger posts) and the Payments/Ledger boundary (Payments executes movement, Ledger posts the accounting effect). Confidence is medium because implementation patterns vary significantly by core banking architecture, and the GL interface boundary with enterprise finance is institution-specific.

### Core business objects

Key financial objects in this domain include: Accounting Event, Journal Entry, Journal Line, Posting Batch, Subledger Account, General Ledger Account Mapping, Balance Record, Accrual Record, Fee Assessment Record, Reconciliation Break, Suspense Item, Accounting Adjustment/Reversal Record, and Financial Lineage Record.

### Events and processing motions

Common triggers: account opening, payment completion, settlement recognition, interest schedule tick, fee trigger, correction/reversal request, end-of-day close, statement cycle close, period close, reconciliation break detection, suspense aging threshold, manual adjustment approval.

Common accounting motions: post, memo, release, reverse, adjust, accrue, capitalize, assess, refund, waive, reclass, settle, reconcile, close.
