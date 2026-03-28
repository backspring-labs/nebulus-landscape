---
id: journey.send_instant_payment
type: journey
name: Send Instant Payment
domain_ids:
  - domain.payments
  - domain.accounts
  - domain.ledger_core
  - domain.risk_fraud
  - domain.compliance
  - domain.channels_experience
capability_ids:
  - cap.payment_initiation_management
  - cap.payment_instruction_validation
  - cap.payment_routing_and_rail_selection
  - cap.payment_authorization_gating_coordination
  - cap.payment_limit_and_cutoff_management
  - cap.payment_status_and_tracking
  - cap.payment_fee_and_pricing_application
  - cap.payment_and_transfer_risk_decisioning
  - cap.payment_exception_and_repair_management
value_stream_id: vs.fund_movement
process_id: proc.instant_payment_v1
entry_capability_id: cap.payment_initiation_management
status: draft
source_refs: []
last_reviewed: 2026-03-27
confidence: high
---

# Send Instant Payment

## 1. Objective

Enable a customer to send funds in real time through an instant payment rail (RTP or FedNow), with payee resolution, pre-transaction fraud and risk screening, irrevocability controls, real-time clearing and settlement, immediate debit of the source account, and confirmation delivered to both sender and receiver within seconds.

## 2. Primary Actor

Retail customer initiating a real-time payment to another person or entity through a digital or mobile channel.

## 3. Trigger

- Customer requests an instant payment through a digital channel or mobile app.
- Customer selects an instant payment option at checkout or within a person-to-person payment flow.
- A request for payment (RfP) is received and the customer approves it for immediate execution.

## 4. Preconditions

- The customer is authenticated and authorized to transact on the source account.
- The source account is in an active, non-restricted state eligible for instant payments.
- The payee has been identified through a directory lookup, alias resolution, or direct account entry.
- The source account has a sufficient available balance (no overdraft or delayed settlement buffer applies for instant payments).
- Instant payment limits, rail availability, and receiving institution participation status are available.

## 5. Happy Path

### Stage 1 — Payment initiation and payee resolution
The customer provides payment details including payee, amount, and optional memo.

**Capabilities activated:** `cap.payment_initiation_management`

The instant payment request is captured with amount, source account, payee identifier, and optional memo. The payee is resolved through a directory service or alias lookup (e.g., email, phone number, or account identifier). The instruction is assigned a unique end-to-end identifier for tracking.

### Stage 2 — Instruction validation
The payment instruction is validated for completeness, payee eligibility, and rail compatibility.

**Capabilities activated:** `cap.payment_instruction_validation`

The payee's receiving institution is confirmed as a participant on the selected instant payment rail (RTP or FedNow). The amount is validated against format constraints and rail-specific minimums and maximums. Required message fields are confirmed present.

### Stage 3 — Risk and compliance screening
The payment is screened for fraud indicators and sanctions compliance in real time.

**Capabilities activated:** `cap.payment_and_transfer_risk_decisioning`

**Participating neighbor domains:** `domain.risk_fraud`, `domain.compliance`

The payment is evaluated against fraud models optimized for real-time decisioning. OFAC screening is performed against the payee. Because instant payments are irrevocable, risk screening must complete within the rail's timeout window (typically under 5 seconds). If risk or compliance flags are raised, the payment is declined.

### Stage 4 — Limit enforcement
Instant-payment-specific limits are evaluated.

**Capabilities activated:** `cap.payment_limit_and_cutoff_management`

Per-transaction, daily, and rolling instant payment limits are checked. Rail-imposed maximum transaction amounts are enforced (e.g., RTP network limit). If limits are exceeded, the payment is declined with an appropriate reason.

### Stage 5 — Authorization and immediate debit
The payment is authorized and the source account is debited immediately.

**Capabilities activated:** `cap.payment_authorization_gating_coordination`

**Participating neighbor domains:** `domain.accounts`, `domain.ledger_core`

Available balance is confirmed. The source account is debited immediately — no hold-then-debit pattern is used because instant payments require real-time finality. Authorization is granted for the payment to proceed to message submission.

### Stage 6 — Rail selection and message submission
The appropriate instant payment rail is selected and the payment message is submitted.

**Capabilities activated:** `cap.payment_routing_and_rail_selection`, `cap.payment_status_and_tracking`

The rail is selected based on payee institution participation, amount, and availability (RTP or FedNow). The payment message is formatted per rail specifications and submitted. The payment status is updated to submitted. The rail processes the message and delivers the credit instruction to the receiving institution.

### Stage 7 — Real-time clearing and settlement confirmation
The payment clears and settles in real time and confirmation is received.

**Capabilities activated:** `cap.payment_status_and_tracking`

The receiving institution accepts the payment and credits the payee's account. A positive acknowledgment is returned through the rail. The payment status is updated to settled. Settlement between institutions occurs in real time through the rail's settlement mechanism.

### Stage 8 — Fee assessment
Any applicable instant payment fees are assessed.

**Capabilities activated:** `cap.payment_fee_and_pricing_application`

Instant payment fees are calculated based on product, channel, and amount. Premium pricing for instant delivery may apply. Fee waivers or relationship-based exemptions are applied where applicable.

### Stage 9 — Confirmation and notification
Both sender and receiver are notified of the completed payment.

**Capabilities activated:** `cap.payment_status_and_tracking`

**Participating neighbor domains:** `domain.channels_experience`

A payment confirmation is delivered to the sender with amount, payee, date, and reference. A credit notification is delivered to the receiver. Notifications are delivered within seconds of settlement to maintain the real-time experience.

## 6. Alternate Paths

- **Request for payment (RfP) approval** — The payment is triggered by an inbound request for payment rather than a customer-initiated send, requiring the customer to review and approve the request before execution.
- **Payee alias resolution failure with fallback** — The payee alias cannot be resolved through the directory, and the customer is prompted to provide direct account and routing details instead.
- **Rail fallback** — The preferred instant payment rail is unavailable or the receiving institution does not participate, and the customer is offered an alternative rail (e.g., same-day ACH) with a different speed and fee profile.
- **Instant payment with enhanced data** — The payment includes structured remittance data or invoice references beyond the standard memo field, requiring extended message formatting.

## 7. Failure / Exception Paths

- Payee alias cannot be resolved and no fallback account details are available.
- Receiving institution is not a participant on any available instant payment rail.
- Fraud model flags the payment for decline (no review queue — instant payments require immediate decision).
- OFAC screening produces a match or the screening cannot complete within the timeout window.
- Insufficient available balance.
- Instant payment limit exceeded (per-transaction or daily).
- Rail-imposed maximum amount exceeded.
- Payment message rejected by the rail due to formatting or routing errors.
- Receiving institution rejects the payment (e.g., account closed, unable to post).
- Rail timeout — the payment network does not return an acknowledgment within the required window.
- Network or connectivity failure preventing message submission.

## 8. Related Domains

- `domain.payments` — owns payment initiation, instruction validation, rail selection, message submission, limit enforcement, fee assessment, and status tracking
- `domain.accounts` — owns source account eligibility, balance verification, and immediate debit posting
- `domain.ledger_core` — owns balance inquiry, debit posting, and real-time reconciliation
- `domain.risk_fraud` — owns real-time fraud screening and instant payment risk decisioning
- `domain.compliance` — owns OFAC screening within real-time timeout constraints
- `domain.channels_experience` — owns payment request presentation, confirmation delivery, and real-time notification

## 9. Related Capabilities

- `cap.payment_initiation_management`
- `cap.payment_instruction_validation`
- `cap.payment_routing_and_rail_selection`
- `cap.payment_authorization_gating_coordination`
- `cap.payment_limit_and_cutoff_management`
- `cap.payment_status_and_tracking`
- `cap.payment_fee_and_pricing_application`
- `cap.payment_and_transfer_risk_decisioning`
- `cap.payment_exception_and_repair_management`

## 10. Value Stream Context

This journey sits inside `vs.fund_movement` as the real-time, irrevocable payment mechanism. Instant payments represent the newest payment rail in U.S. retail banking and impose the most demanding performance requirements on fraud screening, balance verification, and message processing. The irrevocability and real-time finality of instant payments distinguish this journey from ACH (batch, revocable) and align it more closely with wires, but at lower value thresholds and with consumer-oriented use cases.

## 11. Supporting Process Candidates

- `proc.instant_payment_v1`
- `proc.payee_alias_resolution`
- `proc.real_time_fraud_screening`
- `proc.ofac_screening`
- `proc.instant_payment_limit_evaluation`
- `proc.instant_payment_message_submission`
- `proc.instant_payment_settlement_confirmation`
- `proc.request_for_payment_processing`

## 12. Key Risks and Controls Encountered

### Key risks
- Funds sent irrevocably to a fraudulent payee with no recall mechanism
- OFAC screening timeout forcing a default-allow or default-deny decision under pressure
- Fraud screening latency exceeding the rail timeout and degrading the customer experience or forcing a risky default
- Unauthorized payment initiated through compromised credentials or social engineering
- Double-payment due to retry logic when rail acknowledgment is delayed or ambiguous
- Receiving institution rejects payment after source account has been debited, creating a reconciliation gap
- Rail unavailability during high-demand periods

### Key controls
- Real-time fraud model optimized for sub-second decisioning with instant-payment-specific features
- OFAC screening with guaranteed timeout behavior and configurable default-deny policy
- Step-up authentication for instant payment initiation
- Idempotency key enforcement to prevent duplicate submission on retry
- Immediate debit with no hold period to prevent double-spend
- Receiving institution participation verification before submission
- Rail-level acknowledgment tracking with timeout-based exception handling
- Per-transaction and daily instant payment limits with real-time aggregation
- Audit trail for all instant payment lifecycle events
- Reconciliation process for rejected payments to ensure source account re-credit

## 13. Typical Systems and Providers Involved

### Typical systems
- Core banking platform (account and ledger module)
- Instant payment processing engine
- RTP network connection / interface
- FedNow connection / interface
- Payee directory / alias resolution service
- OFAC screening service (real-time capable)
- Fraud detection and decisioning engine (real-time capable)
- Digital banking platform (online/mobile)
- Notification and alerting service

### Representative providers
- The Clearing House (RTP network)
- Federal Reserve (FedNow)
- Fiserv
- Jack Henry
- FIS
- Volante Technologies (instant payment processing)
- ACI Worldwide (real-time payments)
- NICE Actimize / Verafin (real-time fraud)
- Accuity / LexisNexis (OFAC screening)

## 14. Open Questions

- Should request-for-payment (RfP) flows be modeled as a separate journey given their distinct trigger and approval pattern?
- How should rail selection between RTP and FedNow be governed — as a Payments routing decision or an institutional strategy decision?
- What is the appropriate default behavior when OFAC screening cannot complete within the rail timeout — default-deny or queue-and-hold?
- Should instant payment exception handling (e.g., receiving institution rejection) be modeled as part of this journey or a separate reconciliation journey?
- How should instant payment limits relate to broader transfer limits — are they additive, shared, or independent?
- What is the boundary between Payments-owned payee resolution and a potential Directory/Identity domain concern?

## 15. Provenance

This journey represents the newest and most performance-demanding fund movement mechanism in U.S. retail banking. It exercises Payments-domain capabilities under the tightest latency constraints, requiring real-time fraud decisioning, immediate balance verification, and sub-second message processing. The irrevocable nature of instant payments elevates pre-transaction risk controls to the highest priority and eliminates the post-transaction return and recall patterns available in ACH. This journey establishes the performance and risk ceiling for the Payments domain capability model.
