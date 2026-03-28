---
id: proc.instant_payment_v1
type: process
name: Instant Payment v1
journey_id: journey.send_instant_payment
capability_ids:
  - cap.payment_initiation_management
  - cap.payment_instruction_validation
  - cap.payment_routing_and_rail_selection
  - cap.payment_authorization_gating_coordination
  - cap.payment_limit_and_cutoff_management
  - cap.instant_payment_execution
  - cap.payment_status_and_tracking
  - cap.payment_fee_and_pricing_application
  - cap.payment_and_transfer_risk_decisioning
  - cap.payment_exception_and_repair_management
status: draft
source_refs:
  - src.tch_rtp_operating_rules
  - src.fednow_service_operating_circular
  - src.ofac_compliance_framework
  - src.reg_e_electronic_fund_transfers
  - src.ffiec_bsaaml_manual_2026
last_reviewed: 2026-03-28
confidence: high
---

# Instant Payment v1

## 1. Objective

Operationally execute the controlled sequence required to originate a real-time payment through an instant payment rail (RTP Network or FedNow Service), ensuring that the payee is resolved and confirmed, fraud and sanctions screening are completed within real-time latency requirements, irrevocability controls are enforced before the point of no return, the payment clears and settles in real time, confirmation is delivered immediately to both sender and receiver, and any exceptions or requests for return of funds are processed according to network operating rules.

## 2. Scope

This process covers:
- payment initiation and payee resolution (alias lookup, account confirmation)
- instruction validation including account eligibility and product rules
- real-time fraud screening and sanctions gating within sub-second latency constraints
- irrevocability confirmation and debit authorization
- real-time clearing through the instant payment network
- credit notification to the receiving institution and receiver
- settlement position tracking and reconciliation
- exception handling including network timeouts, rejections, and request for return of funds

This process does **not** own:
- payee directory management or alias resolution infrastructure (network-operated services consumed by this process)
- sanctions screening decision logic (`domain.risk_fraud` / `domain.compliance`)
- fraud scoring model execution (`domain.risk_fraud`)
- general ledger posting mechanics (`domain.ledger_core`)
- account balance management (`domain.accounts`)
- channel presentation of instant payment flows (`domain.channels_experience`)
- inbound instant payment receive-side processing (separate process)
- request for payment (RfP) initiation and management (separate process)

## 3. Trigger

A customer initiates a real-time payment through a digital channel or mobile app, specifying the recipient by account number, alias (email, phone), or payee identifier.

## 4. Entry Conditions

- The customer is authenticated and authorized to transact on the source account.
- The source account is in an active, non-restricted state eligible for instant payment origination.
- The source account has a sufficient available balance (no overdraft on instant rails).
- The institution is an active participant on the selected instant payment network (RTP or FedNow).
- The instant payment gateway is operational and connected to the network.
- Real-time fraud screening and sanctions screening services are available and responding within latency requirements.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.instant_payment.initiation_and_payee_resolution` | Initiation and Payee Resolution | `domain.payments` with `domain.channels_experience` participation |
| `stage.instant_payment.instruction_validation_and_pre_validation` | Instruction Validation and Pre-Validation | `domain.payments` with `domain.accounts` participation |
| `stage.instant_payment.fraud_screening_and_risk_gating` | Fraud Screening and Risk Gating | `domain.payments` with `domain.risk_fraud` participation |
| `stage.instant_payment.irrevocability_confirmation_and_debit` | Irrevocability Confirmation and Debit | `domain.payments` with `domain.accounts` participation |
| `stage.instant_payment.clearing_and_credit_notification` | Clearing and Credit Notification | `domain.payments` |
| `stage.instant_payment.settlement_and_reconciliation` | Settlement and Reconciliation | `domain.payments` with `domain.ledger_core` participation |
| `stage.instant_payment.exception_and_request_for_return` | Exception and Request for Return | `domain.payments` with `domain.accounts` participation |

## 6. Stage-by-Stage Flow

### Stage: `stage.instant_payment.initiation_and_payee_resolution`

**Description:** The customer specifies the payment recipient and amount through a digital or mobile channel. If the recipient is identified by alias (email address, phone number), the payment processing platform queries the network directory service (e.g., RTP Directory, FedNow alias service) to resolve the alias to a destination account and receiving institution. If the recipient is identified by account number, the routing number is validated against the network participant list. The customer is presented with payee confirmation details before proceeding.

**Capabilities activated:** `cap.payment_initiation_management`, `cap.payment_routing_and_rail_selection`

**Systems involved:** `sys.digital_banking_platform`, `sys.payment_processing_platform`, `sys.instant_payment_gateway`, `sys.customer_master`

**Decision points:**
- Is the recipient specified by alias or by account number?
- Does the alias resolve to a valid destination account on a participating institution?
- Is the receiving institution an active participant on the selected instant payment network?
- Does the customer confirm the resolved payee details before proceeding?
- Which instant payment rail should be used if both RTP and FedNow are available?

**Risks:** `risk.payee_resolution_mismatch`, `risk.alias_pointing_to_wrong_account`, `risk.receiving_institution_not_on_network`

**Controls:** `ctrl.payee_confirmation_before_irrevocability`, `ctrl.alias_resolution_validation`, `ctrl.network_participant_verification`

**Evidence generated:** Payment instruction ID, initiation timestamp, channel identifier, source account ID, recipient identifier (alias or account), resolved destination account and institution, payee confirmation acknowledgment, selected rail

**Handoff:** Payment instruction created with resolved payee; moves to instruction validation.

### Stage: `stage.instant_payment.instruction_validation_and_pre_validation`

**Description:** The payment processing platform validates the instant payment instruction against source account eligibility rules, product-level instant payment permissions, and network-specific pre-validation requirements. The source account is confirmed to be active, non-restricted, and eligible for instant payment debit. Balance sufficiency is confirmed with a hard check (no overdraft on instant rails). The payment amount is validated against network transaction limits and institution-specific instant payment limits.

**Capabilities activated:** `cap.payment_instruction_validation`, `cap.payment_limit_and_cutoff_management`

**Systems involved:** `sys.payment_processing_platform`, `sys.core_banking_platform`, `sys.instant_payment_gateway`

**Decision points:**
- Is the source account eligible for instant payment origination?
- Is the available balance sufficient (hard balance check, no overdraft)?
- Does the payment amount exceed network maximum transaction limits?
- Does the payment amount exceed institution-specific instant payment limits?
- Has the customer exceeded daily or rolling period instant payment velocity limits?

**Risks:** `risk.insufficient_balance_no_overdraft_on_instant`, `risk.network_limit_exceeded`, `risk.velocity_limit_evasion`

**Controls:** `ctrl.instant_payment_velocity_and_limit_enforcement`, `ctrl.hard_balance_check_no_overdraft`, `ctrl.network_limit_validation`

**Evidence generated:** Validation result, source account eligibility confirmation, balance sufficiency result, network limit check, institution limit check, velocity limit evaluation

**Handoff:** Instruction validated; moves to fraud screening and risk gating.

### Stage: `stage.instant_payment.fraud_screening_and_risk_gating`

**Description:** The payment instruction is submitted for real-time fraud screening and sanctions screening. Because instant payments are irrevocable once cleared, the fraud and sanctions screening must complete within strict sub-second latency requirements imposed by the network. The fraud detection engine evaluates the transaction against real-time risk models, behavioral patterns, and velocity rules. The sanctions screening engine performs OFAC screening against the recipient. The risk decision must be rendered before the irrevocability point.

**Capabilities activated:** `cap.payment_and_transfer_risk_decisioning`, `cap.payment_authorization_gating_coordination`

**Systems involved:** `sys.fraud_detection_engine`, `sys.sanctions_screening_engine`, `sys.payment_processing_platform`, `sys.instant_payment_gateway`

**Participating domains:** `domain.risk_fraud`

**Decision points:**
- Does the fraud screening complete within the real-time latency SLA?
- Does the fraud risk score exceed the threshold for decline?
- Does the sanctions screening produce an OFAC match?
- If screening cannot complete within the latency window, should the payment be declined rather than sent unscreened?
- Does the behavioral pattern (amount, recipient, time of day, frequency) indicate elevated risk?

**Risks:** `risk.fraud_on_instant_rail_no_recall`, `risk.real_time_screening_latency_breach`, `risk.sanctions_screening_timeout`, `risk.model_degradation_under_latency_pressure`

**Controls:** `ctrl.real_time_fraud_screening_sla`, `ctrl.decline_on_screening_timeout_policy`, `ctrl.instant_payment_behavioral_risk_rules`, `ctrl.ofac_screening_with_latency_fallback`

**Evidence generated:** Fraud risk score, screening latency measurement, sanctions screening result, behavioral risk analysis, risk decision (approve/decline), screening timeout indicator if applicable, decline reason if applicable

**Handoff:** Screening passed within latency SLA; moves to irrevocability confirmation. If declined, customer notified immediately.

### Stage: `stage.instant_payment.irrevocability_confirmation_and_debit`

**Description:** This stage represents the point of no return. The payment processing platform confirms that all pre-conditions are met (validation passed, risk screening approved, balance confirmed), debits the source account, and submits the payment message to the instant payment network. Once the network accepts the message for clearing, the payment becomes irrevocable. The debit is final and cannot be reversed through normal payment processing (only through a request for return of funds, which the receiver may decline).

**Capabilities activated:** `cap.instant_payment_execution`

**Systems involved:** `sys.payment_processing_platform`, `sys.instant_payment_gateway`, `sys.core_banking_platform`

**Participating domains:** `domain.accounts`

**Decision points:**
- Have all pre-conditions been confirmed at the moment of commitment?
- Is the balance still sufficient at the instant of debit (final race condition check)?
- Has the payment message been accepted by the network for clearing?
- If the network rejects the message, should the debit be reversed immediately?

**Risks:** `risk.irrevocable_payment_to_wrong_recipient`, `risk.balance_race_condition_at_commitment`, `risk.network_rejection_after_debit`

**Controls:** `ctrl.irrevocability_point_of_no_return_gate`, `ctrl.final_balance_check_at_commitment`, `ctrl.immediate_reversal_on_network_rejection`

**Evidence generated:** Irrevocability confirmation timestamp, source account debit record, network message submission record, network acceptance confirmation, point-of-no-return marker

**Handoff:** Payment accepted by network; moves to clearing and credit notification. If network rejects, debit reversed and route to exception handling.

### Stage: `stage.instant_payment.clearing_and_credit_notification`

**Description:** The instant payment network clears the payment in real time, instructing the receiving institution to credit the recipient's account. The receiving institution posts the credit and returns a confirmation message through the network. The sending institution receives the clearing confirmation and delivers immediate notification to the sender confirming the payment was completed. The receiver is notified by their institution of the incoming credit.

**Capabilities activated:** `cap.payment_status_and_tracking`

**Systems involved:** `sys.instant_payment_gateway`, `sys.payment_processing_platform`, `sys.notification_service`

**Decision points:**
- Did the receiving institution accept and credit the payment within the network timeout window?
- Was the clearing confirmation received from the network?
- Which notification channels should receive the sender confirmation?

**Risks:** `risk.network_timeout_or_rejection`, `risk.receiving_institution_rejection`, `risk.confirmation_delivery_failure`

**Controls:** `ctrl.network_response_timeout_handling`, `ctrl.clearing_confirmation_validation`, `ctrl.sender_confirmation_delivery`

**Evidence generated:** Network clearing confirmation, receiving institution acceptance message, sender confirmation record, notification delivery status, clearing timestamp, end-to-end completion time

**Handoff:** Payment cleared and confirmed. Moves to settlement and reconciliation for position tracking.

### Stage: `stage.instant_payment.settlement_and_reconciliation`

**Description:** While instant payments clear in real time, settlement between participating institutions occurs through periodic net settlement cycles managed by the network operator (e.g., RTP settlement through joint account at the Federal Reserve, FedNow settlement through master accounts). The payment processing platform tracks the institution's net settlement position, reconciles cleared payments against settlement obligations, and monitors prefunded balance or credit limits to ensure continued participation.

**Capabilities activated:** `cap.payment_status_and_tracking`

**Systems involved:** `sys.payment_processing_platform`, `sys.settlement_and_reconciliation_system`, `sys.instant_payment_gateway`

**Participating domains:** `domain.ledger_core`

**Decision points:**
- Does the institution's net settlement position remain within prefunded balance or credit limits?
- Are all cleared payments matched in the settlement reconciliation?
- Is the prefunded balance approaching a threshold requiring replenishment?

**Risks:** `risk.settlement_position_breach`, `risk.prefunded_balance_exhaustion`, `risk.reconciliation_break_between_clearing_and_settlement`

**Controls:** `ctrl.settlement_position_monitoring`, `ctrl.prefunded_balance_threshold_alerting`, `ctrl.clearing_to_settlement_reconciliation`

**Evidence generated:** Net settlement position record, settlement cycle reconciliation, prefunded balance status, reconciliation match results, replenishment trigger if applicable

**Handoff:** Settlement position tracked and reconciled. Process complete for successful payments.

### Stage: `stage.instant_payment.exception_and_request_for_return`

**Description:** If the payment encounters a network timeout, rejection, or the sender subsequently identifies the payment as erroneous or fraudulent, the exception handling path is activated. Network timeouts and rejections result in immediate debit reversal and customer notification. For completed payments that were sent in error or identified as fraudulent, the sending institution may initiate a request for return of funds (RfR) through the network. The receiving institution and recipient are not obligated to honor the request, making this a best-effort recovery mechanism.

**Capabilities activated:** `cap.payment_exception_and_repair_management`

**Systems involved:** `sys.instant_payment_gateway`, `sys.payment_processing_platform`, `sys.core_banking_platform`, `sys.notification_service`

**Participating domains:** `domain.accounts`

**Decision points:**
- Is the exception a network timeout (payment status indeterminate) or a definitive rejection?
- For timeouts, has the network confirmed the payment status after investigation?
- For erroneous or fraudulent payments, should a request for return of funds be initiated?
- Did the receiving institution honor the request for return?
- If the RfR is denied, should the matter be escalated to dispute or fraud investigation?

**Risks:** `risk.request_for_return_denial`, `risk.indeterminate_payment_status_after_timeout`, `risk.customer_expectation_of_recall_on_irrevocable_payment`

**Controls:** `ctrl.request_for_return_processing_rules`, `ctrl.timeout_status_investigation_procedure`, `ctrl.customer_irrevocability_disclosure`, `ctrl.rfr_tracking_and_escalation`

**Evidence generated:** Exception record, exception type (timeout/rejection/error/fraud), debit reversal record if applicable, request for return record if initiated, RfR response from receiving institution, customer notification record, escalation record if RfR denied

**Handoff:** Exception resolved. Payment either reversed (for rejections/timeouts), funds returned (for honored RfR), or escalated to dispute/fraud investigation (for denied RfR). Customer notified of outcome.

## 7. Decision Points

Key decisions across the process:
- Payee resolution method: alias lookup vs direct account entry
- Rail selection: RTP vs FedNow when both are available
- Balance sufficiency: hard check with no overdraft allowance
- Real-time fraud screening: approve vs decline within sub-second SLA
- Screening timeout: decline payment vs send unscreened (institutional policy)
- Irrevocability gate: all pre-conditions met vs halt before point of no return
- Network acceptance: payment cleared vs rejected vs timeout
- Request for return: initiate vs not applicable
- RfR outcome: funds returned vs return denied

## 8. Alternate Outcomes

- Payment declined due to fraud screening within real-time SLA
- Payment declined due to screening timeout (decline-on-timeout policy)
- Payment declined due to OFAC match
- Payment declined due to insufficient balance (no overdraft on instant rails)
- Payment declined due to network or institution limit exceeded
- Payment rejected by network; debit reversed and customer notified
- Payment timed out at network; status investigated and resolved
- Payment completed; request for return initiated for erroneous payment
- Request for return honored; funds credited back to sender
- Request for return denied; escalated to fraud investigation or dispute
- Payee alias resolution failed; customer prompted to provide account details

## 9. Risks (process-level)

- Irrevocable payment sent to wrong recipient with no guaranteed recovery
- Fraud on instant rails exploiting the inability to recall payments
- Payee alias resolution mismatch directing funds to incorrect account
- Real-time screening latency breach causing either unscreened payments or excessive declines
- Network timeout creating indeterminate payment status
- Settlement position breach causing temporary inability to send instant payments
- Request for return denial leaving sender without recourse through the payment network
- Model degradation under latency pressure reducing fraud detection effectiveness
- Prefunded balance exhaustion halting instant payment capability

## 10. Controls (process-level)

- Payee confirmation before irrevocability with clear display of resolved recipient details
- Real-time fraud screening SLA enforcement with decline-on-timeout policy
- OFAC screening with latency-optimized processing and fallback rules
- Irrevocability point-of-no-return gate with final pre-condition verification
- Instant payment velocity and per-transaction limit enforcement
- Hard balance check with no overdraft allowance on instant rails
- Network response timeout handling with status investigation procedure
- Settlement position monitoring with prefunded balance threshold alerting
- Request for return processing rules with tracking and escalation

## 11. Evidence / Audit Considerations

This process generates a real-time audit trail:
- payment instruction record with payee resolution details
- alias lookup and resolution records
- source account eligibility and balance verification
- fraud screening results with latency measurements
- sanctions screening results
- irrevocability confirmation with point-of-no-return timestamp
- source account debit record
- network message submission and acceptance records
- clearing confirmation from receiving institution
- sender and receiver notification records
- settlement position tracking and reconciliation records
- exception records including timeout investigations
- request for return records and outcomes if applicable

## 12. Systems Involved (summary)

- payment processing platform
- instant payment gateway (RTP/FedNow connectivity)
- core banking platform
- customer master / CIF
- fraud detection engine
- sanctions screening engine
- notification service
- settlement and reconciliation system
- digital banking platform

## 13. Provider Participation

Representative providers often adjacent to this process:
- FIS, Fiserv, Jack Henry, ACI Worldwide, Volante Technologies, Finzly, The Clearing House (RTP Network), Federal Reserve Financial Services (FedNow), Temenos, Finastra, Alacriti

## 14. Open Questions

- Should RTP and FedNow be modeled as separate process variants given their different operating rules, settlement mechanics, and message specifications?
- How should the process handle the scenario where both RTP and FedNow are available and the receiving institution participates in both networks?
- What is the appropriate institutional policy for screening timeouts: decline the payment (customer friction) or send unscreened (risk acceptance)?
- How should request for return processing be coordinated between the payments domain and the fraud investigation process when the RfR is fraud-related?
- As instant payment limits increase over time, at what threshold should approval workflows similar to wire transfers be introduced for instant payments?
- How should the process accommodate future overlay services (e.g., Request for Payment) that may trigger instant payment execution?

## 15. Provenance

This process is the primary operational path for real-time payment origination within `domain.payments`. Instant payments represent the fastest-growing payment rail in the US banking system, with both the RTP Network (operated by The Clearing House) and FedNow Service (operated by the Federal Reserve) providing 24x7x365 real-time clearing and settlement. The defining characteristic of this process compared to other payment processes is irrevocability: once the network accepts the payment for clearing, the sender cannot recall or reverse the payment through normal payment processing. This irrevocability constraint fundamentally shapes the risk management approach, requiring that all fraud screening, sanctions screening, and validation occur before the point of no return, within sub-second latency constraints. The process is subject to Reg E for consumer transactions, OFAC compliance requirements, and the specific operating rules of the RTP Network and FedNow Service. The real-time nature of the process also creates unique operational risk considerations around system availability, screening service latency, and settlement position management.
