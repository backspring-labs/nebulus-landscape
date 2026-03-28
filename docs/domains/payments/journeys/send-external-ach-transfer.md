---
id: journey.send_external_ach_transfer
type: journey
name: Send External ACH Transfer
domain_ids:
  - domain.payments
  - domain.accounts
  - domain.ledger_core
  - domain.risk_fraud
  - domain.compliance
  - domain.channels_experience
  - domain.orchestration_control
capability_ids:
  - cap.payment_initiation_management
  - cap.payment_instruction_validation
  - cap.payment_routing_and_rail_selection
  - cap.payment_authorization_gating_coordination
  - cap.payment_limit_and_cutoff_management
  - cap.payment_file_and_batch_management
  - cap.payment_status_and_tracking
  - cap.payment_fee_and_pricing_application
  - cap.payment_and_transfer_risk_decisioning
  - cap.payment_return_reversal_and_recall_management
value_stream_id: vs.fund_movement
process_id: proc.external_ach_transfer_v1
entry_capability_id: cap.payment_initiation_management
status: draft
source_refs: []
last_reviewed: 2026-03-27
confidence: high
---

# Send External ACH Transfer

## 1. Objective

Enable a customer to send funds from their account to an external account at another financial institution via the ACH network, with validation of recipient details, OFAC and fraud screening, limit and cutoff enforcement, NACHA-compliant file generation and transmission, and tracking through settlement and potential return processing.

## 2. Primary Actor

Retail customer initiating an outbound ACH credit transfer to an external account (e.g., at another bank, brokerage, or payment provider).

## 3. Trigger

- Customer requests a one-time or recurring external ACH transfer through a digital channel, mobile app, branch, or call center.
- A previously scheduled recurring ACH transfer reaches its execution date.

## 4. Preconditions

- The customer is authenticated and authorized to transact on the source account.
- The source account is in an active, non-restricted state eligible for external transfers.
- The external recipient account has been previously linked and verified (e.g., via micro-deposits or instant account verification).
- The source account has a sufficient available balance.
- ACH transfer limits, cutoff schedules, and NACHA formatting rules are available.

## 5. Happy Path

### Stage 1 — Transfer initiation and instruction capture
The customer provides transfer details including source account, external recipient, amount, and optional memo or scheduling instructions.

**Capabilities activated:** `cap.payment_initiation_management`

The ACH transfer request is captured with amount, source account, linked external recipient, and scheduling parameters. The instruction is assigned a unique trace number for tracking.

### Stage 2 — Instruction validation and recipient verification
The transfer instruction is validated for completeness, and the external recipient details are confirmed.

**Capabilities activated:** `cap.payment_instruction_validation`

The external recipient routing number and account number are validated for format and structure. The recipient link status is confirmed as verified. The amount is validated against format constraints and minimum/maximum thresholds.

### Stage 3 — Risk and compliance screening
The transfer is screened for fraud indicators and sanctions compliance.

**Capabilities activated:** `cap.payment_and_transfer_risk_decisioning`

**Participating neighbor domains:** `domain.risk_fraud`, `domain.compliance`

The transfer is evaluated against fraud models and transaction patterns. OFAC screening is performed against the recipient name and details. If risk or compliance flags are raised, the transfer is routed for review or declined.

### Stage 4 — Limit and cutoff enforcement
ACH-specific transfer limits and cutoff windows are evaluated.

**Capabilities activated:** `cap.payment_limit_and_cutoff_management`

Daily, per-transaction, and rolling ACH transfer limits are checked. ACH cutoff times are assessed for same-day or next-day processing windows. If limits are exceeded or the cutoff has passed, the transfer is declined, deferred, or queued for the next processing window.

### Stage 5 — Authorization and balance hold
The transfer is authorized and funds are held in the source account.

**Capabilities activated:** `cap.payment_authorization_gating_coordination`

**Participating neighbor domains:** `domain.accounts`, `domain.ledger_core`

Available balance is confirmed. A hold is placed on the source account for the transfer amount. Authorization is granted for the transfer to proceed to file generation.

### Stage 6 — Rail selection and file generation
The ACH rail is selected and a NACHA-compliant file is generated.

**Capabilities activated:** `cap.payment_routing_and_rail_selection`, `cap.payment_file_and_batch_management`

The appropriate ACH processing window is selected (same-day ACH or standard). The transfer is formatted into a NACHA-compliant batch entry. The entry is included in the next outbound ACH file for transmission.

### Stage 7 — File transmission and source account debit
The ACH file is transmitted and the source account is debited.

**Capabilities activated:** `cap.payment_file_and_batch_management`, `cap.payment_status_and_tracking`

**Participating neighbor domains:** `domain.ledger_core`

The ACH file is transmitted to the Federal Reserve or EPN. The source account hold is converted to a debit posting. The transfer status is updated to submitted.

### Stage 8 — Fee assessment
Any applicable ACH transfer fees are assessed.

**Capabilities activated:** `cap.payment_fee_and_pricing_application`

ACH transfer fees are calculated based on product, channel, and transfer type (same-day vs. standard). Fee waivers or relationship-based exemptions are applied where applicable.

### Stage 9 — Settlement tracking and confirmation
The transfer is tracked through settlement and the customer is notified.

**Capabilities activated:** `cap.payment_status_and_tracking`

**Participating neighbor domains:** `domain.channels_experience`

Settlement confirmation is received from the ACH operator. The transfer status is updated to settled. A confirmation notification is delivered to the customer with amount, recipient, date, and trace number.

## 6. Alternate Paths

- **Scheduled or recurring ACH transfer** — The transfer is captured with a future date or recurrence schedule and executed by the ACH batch process when the execution date arrives.
- **Same-day ACH** — The customer requests expedited processing and the transfer is routed to the same-day ACH window with applicable premium fees.
- **ACH transfer to newly linked account** — The recipient account was recently linked and is still within a probationary period, triggering lower limits or additional verification.
- **High-value ACH requiring approval** — The transfer amount exceeds a threshold requiring additional approval or step-up authentication before submission.

## 7. Failure / Exception Paths

- External recipient account link is not verified or has expired.
- OFAC screening produces a match or potential match requiring manual review.
- Fraud model flags the transaction for review or decline.
- Insufficient available balance and no overdraft coverage.
- ACH transfer limit exceeded.
- ACH cutoff time missed and deferral is not acceptable to the customer.
- NACHA file generation or transmission failure.
- ACH return received (e.g., R01 insufficient funds at receiver, R02 account closed, R03 no account).
- NOC (Notification of Change) received requiring recipient detail update.

## 8. Related Domains

- `domain.payments` — owns transfer initiation, instruction validation, rail selection, file generation, limit enforcement, fee assessment, and status tracking
- `domain.accounts` — owns source account eligibility, balance verification, and hold management
- `domain.ledger_core` — owns balance inquiry, debit posting, and reconciliation
- `domain.risk_fraud` — owns fraud screening and transaction risk decisioning
- `domain.compliance` — owns OFAC screening and sanctions compliance
- `domain.channels_experience` — owns transfer request presentation, confirmation delivery, and notification
- `domain.orchestration_control` — may coordinate multi-step ACH processing workflows

## 9. Related Capabilities

- `cap.payment_initiation_management`
- `cap.payment_instruction_validation`
- `cap.payment_routing_and_rail_selection`
- `cap.payment_authorization_gating_coordination`
- `cap.payment_limit_and_cutoff_management`
- `cap.payment_file_and_batch_management`
- `cap.payment_status_and_tracking`
- `cap.payment_fee_and_pricing_application`
- `cap.payment_and_transfer_risk_decisioning`
- `cap.payment_return_reversal_and_recall_management`

## 10. Value Stream Context

This journey sits inside `vs.fund_movement` as the primary external transfer mechanism for retail customers. ACH is the highest-volume external payment rail in U.S. retail banking, and this journey represents the outbound credit path. It extends the internal transfer baseline by introducing external recipient management, compliance screening, NACHA file generation, and return handling.

## 11. Supporting Process Candidates

- `proc.external_ach_transfer_v1`
- `proc.ach_recipient_verification`
- `proc.ofac_screening`
- `proc.ach_limit_evaluation`
- `proc.nacha_file_generation`
- `proc.ach_file_transmission`
- `proc.ach_return_processing`
- `proc.ach_noc_processing`

## 12. Key Risks and Controls Encountered

### Key risks
- Funds sent to an unverified or fraudulent external account
- OFAC sanctions violation from unscreened recipient
- ACH return after funds have been released, creating a loss exposure
- Duplicate ACH submission due to file retransmission or idempotency failure
- NACHA formatting error causing file rejection
- Transfer limit bypass due to stale or inconsistent limit state
- Cutoff time miscalculation causing missed processing window

### Key controls
- External account verification requirement before first transfer
- OFAC screening gate before file generation
- Fraud model evaluation with configurable risk thresholds
- Balance hold before file transmission to mitigate return risk
- Idempotency controls at file and entry level
- NACHA format validation before transmission
- ACH limit evaluation with real-time aggregation
- Return and NOC processing workflows with automated remediation
- Audit trail for all ACH transfer lifecycle events

## 13. Typical Systems and Providers Involved

### Typical systems
- Core banking platform (account and ledger module)
- ACH processing engine / origination platform
- OFAC screening service
- Fraud detection and decisioning engine
- Digital banking platform (online/mobile)
- Notification and alerting service
- ACH file transmission gateway (Fed or EPN connection)

### Representative providers
- Fiserv (ACH origination)
- Jack Henry
- FIS
- Temenos
- Bottomline Technologies
- Verafin / NICE Actimize (fraud/AML)
- Accuity / LexisNexis (OFAC screening)

## 14. Open Questions

- Should ACH return processing be modeled as a separate journey or as a continuation of this journey?
- How should same-day ACH be represented — as a variant path or a distinct journey with different risk and limit profiles?
- What is the boundary between Payments-owned OFAC screening and Compliance-domain screening services?
- Should NOC processing be a distinct operational journey given its distinct trigger and workflow?
- How should probationary limits for newly linked external accounts be modeled — as a Payments concern or an Account-linking concern?

## 15. Provenance

This journey represents the most common external fund movement mechanism in U.S. retail banking. It extends the internal transfer baseline by introducing the ACH rail with its batch-oriented processing model, NACHA file requirements, compliance screening obligations, and return/recall handling. It exercises the full breadth of Payments-domain capabilities and establishes patterns reused by the wire transfer and instant payment journeys.
