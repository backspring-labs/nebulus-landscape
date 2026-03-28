---
id: journey.transfer_between_own_accounts
type: journey
name: Transfer Between Own Accounts
domain_ids:
  - domain.payments
  - domain.accounts
  - domain.ledger_core
  - domain.channels_experience
  - domain.risk_fraud
capability_ids:
  - cap.payment_initiation_management
  - cap.payment_instruction_validation
  - cap.payment_authorization_gating_coordination
  - cap.payment_limit_and_cutoff_management
  - cap.payment_status_and_tracking
  - cap.payment_fee_and_pricing_application
value_stream_id: vs.fund_movement
process_id: proc.internal_transfer_v1
entry_capability_id: cap.payment_initiation_management
status: draft
source_refs: []
last_reviewed: 2026-03-27
confidence: high
---

# Transfer Between Own Accounts

## 1. Objective

Enable a customer to move funds between two accounts they own at the same institution through a controlled path that validates account eligibility, confirms balance sufficiency, enforces transfer limits and cutoff times, executes the debit and credit in real time or near-real time, and delivers confirmation to the customer.

## 2. Primary Actor

Retail customer initiating a transfer between their own deposit accounts (e.g., checking to savings, savings to checking, or between two checking accounts).

## 3. Trigger

- Customer requests a one-time or recurring transfer between own accounts through a digital channel, mobile app, branch, or call center.
- A previously scheduled recurring transfer reaches its execution date.

## 4. Preconditions

- The customer is authenticated and authorized to transact on both the source and destination accounts.
- Both accounts are in an active, non-restricted state eligible for transfers.
- The source account has a sufficient available balance or approved overdraft coverage.
- Transfer limits and cutoff policies are available.

## 5. Happy Path

### Stage 1 — Transfer initiation and instruction capture
The customer provides transfer details including source account, destination account, amount, and optional memo or scheduling instructions.

**Capabilities activated:** `cap.payment_initiation_management`

The transfer request is captured with amount, source, destination, and scheduling parameters. The instruction is assigned a unique identifier for tracking.

### Stage 2 — Instruction validation
The transfer instruction is validated for completeness, account eligibility, and basic business rules.

**Capabilities activated:** `cap.payment_instruction_validation`

Both accounts are confirmed as owned by the same customer and eligible for the requested transfer type. The amount is validated as positive and within format constraints. Source and destination accounts are confirmed as distinct.

### Stage 3 — Limit and cutoff enforcement
Transfer limits and cutoff windows are evaluated to determine whether the transfer can proceed.

**Capabilities activated:** `cap.payment_limit_and_cutoff_management`

Daily, per-transaction, and rolling transfer limits are checked. Regulation D limits are evaluated if the source is a savings account. Cutoff times are assessed. If limits are exceeded or the cutoff has passed, the transfer is declined or deferred.

### Stage 4 — Authorization and balance verification
The transfer is authorized and the source account balance is confirmed as sufficient.

**Capabilities activated:** `cap.payment_authorization_gating_coordination`

**Participating neighbor domains:** `domain.accounts`, `domain.ledger_core`

Available balance is confirmed. A hold or memo-post is placed on the source account to reserve funds. Authorization is granted for the transfer to proceed to execution.

### Stage 5 — Execution and posting
The debit and credit are posted to the source and destination accounts.

**Capabilities activated:** `cap.payment_initiation_management`, `cap.payment_status_and_tracking`

**Participating neighbor domains:** `domain.ledger_core`

The source account is debited. The destination account is credited. Both postings are recorded with matching references. The transfer status is updated to completed.

### Stage 6 — Fee assessment
Any applicable transfer fees are assessed and posted.

**Capabilities activated:** `cap.payment_fee_and_pricing_application`

Transfer fees, if configured for the product or channel, are calculated and posted. Fee waivers or relationship-based exemptions are applied where applicable.

### Stage 7 — Confirmation and notification
The customer is notified of the completed transfer.

**Capabilities activated:** `cap.payment_status_and_tracking`

**Participating neighbor domains:** `domain.channels_experience`

A transfer confirmation is generated with details including amount, accounts, date, and reference number. The confirmation is delivered through the originating channel and any configured notification preferences.

## 6. Alternate Paths

- **Scheduled or recurring transfer** — The transfer is captured with a future date or recurrence schedule and executed by a batch or scheduler when the execution date arrives.
- **Insufficient funds with overdraft** — The source account lacks sufficient available balance but has overdraft protection linked to another account, triggering an overdraft advance before the transfer proceeds.
- **Regulation D limit reached** — The savings-account source has reached the federal transfer limit, and the transfer is declined or the customer is warned and given the option to proceed with potential account reclassification.
- **Branch or call center initiation** — A teller or agent captures the transfer on behalf of the customer, with additional identity verification steps.

## 7. Failure / Exception Paths

- Source or destination account is restricted, closed, or not eligible for transfers.
- Insufficient available balance and no overdraft coverage.
- Transfer limit exceeded (daily, per-transaction, or Regulation D).
- Cutoff time has passed and the transfer cannot be deferred.
- Posting failure due to ledger unavailability or reconciliation error.
- Duplicate transfer detection triggers a hold for review.

## 8. Related Domains

- `domain.payments` — owns transfer initiation, instruction validation, limit enforcement, execution, fee assessment, and status tracking
- `domain.accounts` — owns account eligibility, status verification, and restriction checks
- `domain.ledger_core` — owns balance inquiry, debit/credit posting, and reconciliation
- `domain.channels_experience` — owns transfer request presentation, confirmation delivery, and notification
- `domain.risk_fraud` — may evaluate transfer for fraud indicators or unusual patterns

## 9. Related Capabilities

- `cap.payment_initiation_management`
- `cap.payment_instruction_validation`
- `cap.payment_authorization_gating_coordination`
- `cap.payment_limit_and_cutoff_management`
- `cap.payment_status_and_tracking`
- `cap.payment_fee_and_pricing_application`

## 10. Value Stream Context

This journey sits inside `vs.fund_movement` as the simplest and most common payment journey. It represents an internal book transfer with no external rail involvement, making it the baseline fund movement pattern upon which external transfer journeys build.

## 11. Supporting Process Candidates

- `proc.internal_transfer_v1`
- `proc.transfer_limit_evaluation`
- `proc.balance_sufficiency_check`
- `proc.debit_credit_posting`
- `proc.transfer_confirmation_delivery`

## 12. Key Risks and Controls Encountered

### Key risks
- Unauthorized transfer between accounts not owned by the same customer
- Double-posting due to retry or idempotency failure
- Regulation D violation from excessive savings withdrawals
- Transfer limit bypass due to stale or inconsistent limit state
- Posting failure leaving source debited but destination not credited

### Key controls
- Ownership validation confirming both accounts belong to the requesting customer
- Idempotency key enforcement to prevent duplicate execution
- Regulation D counter tracking with real-time limit enforcement
- Atomic or compensating transaction patterns for debit/credit consistency
- Transfer limit evaluation with real-time aggregation
- Audit trail for all transfer lifecycle events

## 13. Typical Systems and Providers Involved

### Typical systems
- Core banking platform (account and ledger module)
- Payment processing engine
- Digital banking platform (online/mobile)
- Notification and alerting service
- Transfer limit and policy engine

### Representative providers
- Fiserv
- Jack Henry
- FIS
- Temenos
- Q2

## 14. Open Questions

- Should Regulation D enforcement be modeled as a Payments-domain concern or an Accounts-domain concern?
- How should scheduled and recurring transfers be represented — as a variant of this journey or a distinct scheduling journey?
- What is the boundary between Payments-owned transfer limits and Accounts-owned withdrawal limits?
- Should internal transfers that cross product types (e.g., deposit to loan) be a separate journey?

## 15. Provenance

This journey represents the most fundamental fund movement operation in retail banking. It exercises core Payments-domain capabilities around initiation, validation, limit enforcement, and status tracking without introducing external rail complexity. It serves as the baseline pattern for the more complex external ACH, wire, and instant payment journeys.
