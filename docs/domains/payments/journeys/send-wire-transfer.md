---
id: journey.send_wire_transfer
type: journey
name: Send Wire Transfer
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
  - cap.payment_status_and_tracking
  - cap.payment_fee_and_pricing_application
  - cap.payment_and_transfer_risk_decisioning
  - cap.payment_exception_and_repair_management
value_stream_id: vs.fund_movement
process_id: proc.wire_transfer_v1
entry_capability_id: cap.payment_initiation_management
status: draft
source_refs: []
last_reviewed: 2026-03-27
confidence: high
---

# Send Wire Transfer

## 1. Objective

Enable a customer or institution to send funds via a domestic or international wire transfer, with beneficiary validation, OFAC and sanctions screening, approval workflows for high-value or unusual transfers, Fedwire or SWIFT message generation and transmission, real-time debit of the source account, and tracking through to final settlement confirmation.

## 2. Primary Actor

Retail or commercial customer initiating a wire transfer, or an institutional actor initiating a wire on behalf of a customer or for an operational purpose.

## 3. Trigger

- Customer requests a domestic or international wire transfer through a digital channel, branch, or call center.
- Institution initiates a wire for an operational purpose (e.g., loan disbursement, correspondent banking).
- A pre-authorized or standing wire instruction reaches its execution date.

## 4. Preconditions

- The customer is authenticated and authorized to initiate wire transfers on the source account.
- The source account is in an active, non-restricted state eligible for wire transfers.
- Beneficiary details (name, account, routing/SWIFT BIC, intermediary bank if applicable) are available.
- The source account has a sufficient available balance to cover the wire amount plus fees.
- Wire transfer limits, cutoff schedules, and approval policies are available.

## 5. Happy Path

### Stage 1 — Wire initiation and instruction capture
The customer or operator provides wire transfer details including source account, beneficiary, amount, currency, purpose, and any special instructions.

**Capabilities activated:** `cap.payment_initiation_management`

The wire transfer request is captured with amount, currency, source account, beneficiary details, and purpose of payment. For international wires, intermediary bank and correspondent details are captured. The instruction is assigned a unique reference for tracking.

### Stage 2 — Instruction validation and beneficiary verification
The wire instruction is validated for completeness, formatting, and beneficiary detail accuracy.

**Capabilities activated:** `cap.payment_instruction_validation`

Beneficiary name, account number, and routing/SWIFT BIC are validated for format and structure. For international wires, IBAN format and country-specific requirements are checked. Required fields for the target rail (Fedwire or SWIFT) are confirmed present.

### Stage 3 — Risk and compliance screening
The wire is screened for fraud indicators, sanctions compliance, and suspicious activity patterns.

**Capabilities activated:** `cap.payment_and_transfer_risk_decisioning`

**Participating neighbor domains:** `domain.risk_fraud`, `domain.compliance`

The wire is evaluated against fraud models and transaction patterns. OFAC and global sanctions screening is performed against the beneficiary, intermediary banks, and any related parties. For international wires, country-level risk assessment is performed. If risk or compliance flags are raised, the wire is routed for manual review or declined.

### Stage 4 — Approval workflow
High-value or policy-triggered wires are routed for operational approval.

**Capabilities activated:** `cap.payment_authorization_gating_coordination`

**Participating neighbor domains:** `domain.orchestration_control`

Wires exceeding approval thresholds or matching policy triggers (first-time beneficiary, high-risk country, unusual pattern) are routed to the appropriate approval queue. Dual-control or multi-level approval is enforced where required. The wire is held until approval is granted or the wire is rejected.

### Stage 5 — Limit and cutoff enforcement
Wire-specific transfer limits and cutoff windows are evaluated.

**Capabilities activated:** `cap.payment_limit_and_cutoff_management`

Daily, per-transaction, and rolling wire limits are checked. Fedwire or SWIFT cutoff times are assessed. If limits are exceeded, the wire is declined. If the cutoff has passed, the wire is queued for the next business day or declined.

### Stage 6 — Balance hold and source account debit
Funds are reserved and the source account is debited including applicable fees.

**Capabilities activated:** `cap.payment_authorization_gating_coordination`, `cap.payment_fee_and_pricing_application`

**Participating neighbor domains:** `domain.accounts`, `domain.ledger_core`

Available balance is confirmed for the wire amount plus fees. The source account is debited. Wire fees are assessed and posted. The debit is recorded with the wire reference.

### Stage 7 — Message generation and transmission
A Fedwire or SWIFT message is generated and transmitted to the payment network.

**Capabilities activated:** `cap.payment_routing_and_rail_selection`, `cap.payment_status_and_tracking`

The appropriate rail is selected (Fedwire for domestic, SWIFT for international). The wire message is formatted per rail specifications. The message is transmitted to the payment network. The wire status is updated to submitted.

### Stage 8 — Settlement tracking and confirmation
The wire is tracked through settlement and the customer is notified.

**Capabilities activated:** `cap.payment_status_and_tracking`

**Participating neighbor domains:** `domain.channels_experience`

Settlement confirmation or acknowledgment is received from the payment network. For international wires, intermediary confirmations may be tracked. The wire status is updated to settled. A confirmation notification is delivered to the customer with amount, beneficiary, date, and reference number.

## 6. Alternate Paths

- **International wire with currency conversion** — The wire requires foreign exchange conversion, introducing FX rate quotation, acceptance, and conversion execution before message generation.
- **Institution-initiated wire** — The institution initiates a wire for an operational purpose (e.g., loan disbursement, correspondent settlement), bypassing the customer-facing initiation flow but still requiring compliance screening and approval.
- **Standing or recurring wire** — A pre-authorized wire instruction is executed on a schedule, with balance verification and compliance re-screening at execution time.
- **Wire amendment or cancellation** — The customer or operator requests a change or cancellation after initiation but before transmission, triggering a repair or recall workflow.
- **Branch-initiated wire with wet signature** — The wire is initiated at a branch with a physical wire request form and signature, requiring digitization and manual entry into the wire platform.

## 7. Failure / Exception Paths

- Beneficiary details are invalid or incomplete for the target rail.
- OFAC or sanctions screening produces a match requiring manual review or resulting in decline.
- Fraud model flags the wire for review or decline.
- Approval workflow times out or the wire is rejected by an approver.
- Insufficient available balance to cover wire amount plus fees.
- Wire limit exceeded.
- Fedwire or SWIFT cutoff time missed.
- Wire message rejected by the payment network due to formatting or routing errors.
- Intermediary bank rejects or returns the wire (international).
- Beneficiary bank rejects the credit.

## 8. Related Domains

- `domain.payments` — owns wire initiation, instruction validation, rail selection, message generation, limit enforcement, fee assessment, and status tracking
- `domain.accounts` — owns source account eligibility, balance verification, and debit posting
- `domain.ledger_core` — owns balance inquiry, debit posting, and reconciliation
- `domain.risk_fraud` — owns fraud screening and transaction risk decisioning for wires
- `domain.compliance` — owns OFAC screening, sanctions compliance, and country-risk assessment
- `domain.channels_experience` — owns wire request presentation, confirmation delivery, and notification
- `domain.orchestration_control` — owns approval workflows and dual-control enforcement

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

This journey sits inside `vs.fund_movement` as the high-value, real-time gross settlement payment mechanism. Wire transfers are irrevocable and final upon settlement, distinguishing them from ACH and creating heightened risk and compliance requirements. This journey exercises the most rigorous approval, screening, and validation patterns in the Payments domain.

## 11. Supporting Process Candidates

- `proc.wire_transfer_v1`
- `proc.wire_beneficiary_validation`
- `proc.ofac_screening`
- `proc.wire_approval_workflow`
- `proc.wire_limit_evaluation`
- `proc.fedwire_message_generation`
- `proc.swift_message_generation`
- `proc.wire_settlement_tracking`
- `proc.wire_repair_and_recall`

## 12. Key Risks and Controls Encountered

### Key risks
- Funds sent to a fraudulent or sanctioned beneficiary
- OFAC sanctions violation from unscreened or inadequately screened parties
- Unauthorized wire initiated through compromised credentials or social engineering
- Irrevocable wire sent with incorrect beneficiary details
- High-value wire approved without adequate dual-control
- Wire message formatting error causing rejection or misrouting
- International wire routed through a sanctioned correspondent
- Cutoff time miscalculation causing settlement delay

### Key controls
- OFAC and global sanctions screening gate before message generation
- Fraud model evaluation with wire-specific risk thresholds
- Step-up authentication for wire initiation
- Dual-control approval for wires above configurable thresholds
- Beneficiary detail validation against rail-specific format requirements
- First-time beneficiary flagging and enhanced verification
- Fedwire and SWIFT message format validation before transmission
- Real-time debit with immediate fund removal to prevent double-spend
- Audit trail for all wire lifecycle events including approvals
- Country-risk assessment for international wires

## 13. Typical Systems and Providers Involved

### Typical systems
- Core banking platform (account and ledger module)
- Wire transfer processing platform
- Fedwire connection / interface
- SWIFT Alliance or equivalent messaging platform
- OFAC screening service
- Fraud detection and decisioning engine
- Approval workflow engine
- Digital banking platform (online/mobile)
- Notification and alerting service

### Representative providers
- Fiserv (wire processing)
- Jack Henry
- FIS
- Bottomline Technologies (wire and payments hub)
- Finastra (SWIFT connectivity)
- NICE Actimize / Verafin (fraud/AML)
- Accuity / LexisNexis (OFAC screening)
- Fircosoft (sanctions filtering)

## 14. Open Questions

- Should domestic and international wires be modeled as separate journeys given their distinct rail, compliance, and routing requirements?
- How should wire amendments and recalls be represented — as a continuation of this journey or a distinct exception-handling journey?
- What is the boundary between Payments-owned approval workflows and Orchestration-domain workflow management?
- Should FX conversion for international wires be modeled as an embedded sub-journey or a separate Treasury/FX domain journey?
- How should correspondent bank risk assessment be modeled — as a Compliance concern or a Payments routing concern?

## 15. Provenance

This journey represents the highest-value and most operationally sensitive fund movement mechanism in retail and commercial banking. It exercises the full depth of Payments-domain capabilities including initiation, validation, risk decisioning, approval gating, rail selection, message generation, and exception handling. The irrevocable nature of wires drives the most stringent screening and approval controls across all payment journeys.
