---
id: proc.wire_transfer_v1
type: process
name: Wire Transfer v1
journey_id: journey.send_wire_transfer
capability_ids:
  - cap.payment_initiation_management
  - cap.payment_instruction_validation
  - cap.payment_routing_and_rail_selection
  - cap.payment_authorization_gating_coordination
  - cap.payment_limit_and_cutoff_management
  - cap.wire_transfer_execution
  - cap.payment_status_and_tracking
  - cap.payment_fee_and_pricing_application
  - cap.payment_and_transfer_risk_decisioning
  - cap.payment_exception_and_repair_management
status: draft
source_refs:
  - src.fedwire_funds_service_operating_circular
  - src.swift_standards_and_usage_guidelines
  - src.ofac_compliance_framework
  - src.ffiec_bsaaml_manual_2026
  - src.reg_j_funds_transfers
  - src.ucc_article_4a
last_reviewed: 2026-03-28
confidence: high
---

# Wire Transfer v1

## 1. Objective

Operationally execute the controlled sequence required to originate a domestic or international wire transfer, ensuring that beneficiary details are validated, sanctions and fraud screening are completed, approval workflows are enforced for high-value or high-risk wires, the wire message is formatted to Fedwire or SWIFT standards, the message is transmitted to the payment network, the source account is debited, settlement is confirmed, and any exceptions or repairs are handled within operating circular requirements.

## 2. Scope

This process covers:
- wire transfer initiation and beneficiary detail capture
- instruction validation including account eligibility, beneficiary verification, and message completeness
- OFAC and sanctions screening of all parties (originator, beneficiary, intermediary banks)
- fraud risk scoring and behavioral analysis
- approval workflow execution for high-value and high-risk wires
- wire message formatting to Fedwire Funds Service or SWIFT standards
- message transmission to the payment network
- source account debit
- settlement confirmation and reconciliation
- exception handling including message repairs, rejects, and return of funds

This process does **not** own:
- beneficiary template management (managed separately; this process consumes validated templates)
- sanctions screening decision logic (`domain.risk_fraud` / `domain.compliance`)
- fraud scoring model execution (`domain.risk_fraud`)
- general ledger posting mechanics (`domain.ledger_core`)
- account balance management (`domain.accounts`)
- channel presentation of wire transfer flows (`domain.channels_experience`)
- inbound wire processing (separate receive-side process)
- correspondent banking relationship management (`domain.treasury_operations`)

## 3. Trigger

A customer initiates a wire transfer through a digital channel, branch, or call center, or the institution initiates a wire transfer for operational purposes (e.g., correspondent settlement, interbank transfer, or regulatory payment).

## 4. Entry Conditions

- The originator is authenticated and authorized to initiate wire transfers on the source account.
- The source account is in an active, non-restricted state eligible for wire origination.
- The source account has a sufficient available balance to cover the wire amount plus applicable fees.
- Beneficiary details are available (name, account number, bank identifier, intermediary bank if applicable).
- Wire transfer limits, approval thresholds, and cutoff schedules are available.
- The wire transfer platform is operational and connectivity to Fedwire or SWIFT is active.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.wire_transfer.initiation_and_beneficiary_validation` | Initiation and Beneficiary Validation | `domain.payments` with `domain.channels_experience` participation |
| `stage.wire_transfer.instruction_validation_and_eligibility` | Instruction Validation and Eligibility | `domain.payments` with `domain.accounts` participation |
| `stage.wire_transfer.sanctions_screening_and_risk_gating` | Sanctions Screening and Risk Gating | `domain.payments` with `domain.risk_fraud`, `domain.compliance` participation |
| `stage.wire_transfer.approval_workflow_and_authorization` | Approval Workflow and Authorization | `domain.payments` with `domain.orchestration_control` participation |
| `stage.wire_transfer.message_formatting_and_transmission` | Message Formatting and Transmission | `domain.payments` |
| `stage.wire_transfer.settlement_confirmation_and_reconciliation` | Settlement Confirmation and Reconciliation | `domain.payments` with `domain.ledger_core` participation |
| `stage.wire_transfer.exception_repair_and_return_handling` | Exception, Repair, and Return Handling | `domain.payments` with `domain.accounts` participation |

## 6. Stage-by-Stage Flow

### Stage: `stage.wire_transfer.initiation_and_beneficiary_validation`

**Description:** The originator provides the wire transfer details including beneficiary name, account number, beneficiary bank identifier (ABA routing number for domestic, SWIFT BIC for international), intermediary bank details if applicable, wire amount, currency, and purpose or reference information. The payment processing platform validates the beneficiary details, checks for previously used beneficiary templates, and assigns a unique wire reference number.

**Capabilities activated:** `cap.payment_initiation_management`

**Systems involved:** `sys.digital_banking_platform`, `sys.payment_processing_platform`, `sys.wire_transfer_platform`, `sys.customer_master`

**Decision points:**
- Is the beneficiary using a previously established and validated template?
- Are all required beneficiary fields populated for the wire type (domestic vs international)?
- Is the beneficiary bank identifier valid and active?
- For international wires, is an intermediary bank required for the destination country and currency?

**Risks:** `risk.beneficiary_name_account_mismatch`, `risk.incomplete_beneficiary_details_causing_repair`, `risk.wire_fraud_social_engineering`

**Controls:** `ctrl.beneficiary_validation_and_confirmation`, `ctrl.beneficiary_field_completeness_check`, `ctrl.wire_fraud_awareness_confirmation`

**Evidence generated:** Wire reference number, initiation timestamp, channel identifier, originator details, beneficiary details, beneficiary bank identifier, intermediary bank details if applicable, wire amount, currency, purpose/reference

**Handoff:** Wire instruction created with validated beneficiary; moves to instruction validation.

### Stage: `stage.wire_transfer.instruction_validation_and_eligibility`

**Description:** The payment processing platform validates the wire instruction against source account eligibility rules, product-level wire origination permissions, and message completeness requirements for the target payment network. The source account is confirmed to be active, non-restricted, and eligible for wire debit. The balance is pre-checked to confirm the amount plus fees can be covered.

**Capabilities activated:** `cap.payment_instruction_validation`, `cap.payment_routing_and_rail_selection`

**Systems involved:** `sys.payment_processing_platform`, `sys.core_banking_platform`, `sys.wire_transfer_platform`

**Decision points:**
- Is the source account eligible for wire origination?
- Is the available balance sufficient for the wire amount plus fees?
- Which payment network is required (Fedwire for domestic, SWIFT for international)?
- Does the message contain all required fields for the selected network?
- For international wires, is the currency supported and are correspondent routing details complete?

**Risks:** `risk.insufficient_balance_for_wire_plus_fees`, `risk.incorrect_network_routing`, `risk.missing_correspondent_details_for_international`

**Controls:** `ctrl.balance_sufficiency_pre_check`, `ctrl.network_routing_determination_rules`, `ctrl.message_completeness_validation`

**Evidence generated:** Validation result, source account eligibility confirmation, balance pre-check result, network routing decision (Fedwire vs SWIFT), message completeness check result, fee calculation

**Handoff:** Instruction validated and network determined; moves to sanctions screening and risk gating.

### Stage: `stage.wire_transfer.sanctions_screening_and_risk_gating`

**Description:** The wire instruction is submitted for comprehensive sanctions screening of all parties: originator, beneficiary, beneficiary bank, and intermediary banks. The sanctions screening engine evaluates each party against OFAC SDN, sectoral sanctions, country-based restrictions, and other applicable watchlists. Simultaneously, the fraud detection engine scores the wire for fraud risk based on beneficiary characteristics, amount, frequency, geographic risk, and behavioral patterns. Any OFAC match halts processing pending investigation. Fraud risk decisions may decline, hold, or approve with conditions.

**Capabilities activated:** `cap.payment_and_transfer_risk_decisioning`, `cap.payment_authorization_gating_coordination`

**Systems involved:** `sys.sanctions_screening_engine`, `sys.fraud_detection_engine`, `sys.payment_processing_platform`, `sys.wire_transfer_platform`

**Participating domains:** `domain.risk_fraud`, `domain.compliance`

**Decision points:**
- Does any party (originator, beneficiary, intermediary) produce an OFAC or watchlist match?
- Is the destination country subject to comprehensive sanctions or geographic risk?
- Does the fraud risk score exceed the threshold for hold or decline?
- Is this a first-time beneficiary or an unusual wire pattern for this customer?
- Does the wire amount or pattern suggest structuring or evasion behavior?
- Should the wire be held for analyst review or escalated to compliance?

**Risks:** `risk.sanctions_evasion_through_intermediaries`, `risk.wire_fraud_social_engineering`, `risk.structuring_detection_evasion`, `risk.false_positive_sanctions_match_delaying_legitimate_wire`

**Controls:** `ctrl.ofac_and_sanctions_screening_at_wire`, `ctrl.wire_fraud_risk_scoring`, `ctrl.geographic_risk_assessment`, `ctrl.structuring_pattern_detection`, `ctrl.sanctions_match_investigation_workflow`

**Evidence generated:** OFAC screening results for all parties, watchlist match details if any, geographic risk assessment, fraud risk score, behavioral pattern analysis, risk decision (approve/decline/hold), analyst review record if held

**Handoff:** Screening passed; moves to approval workflow. If OFAC match, held pending compliance investigation. If fraud hold, routed to analyst queue.

### Stage: `stage.wire_transfer.approval_workflow_and_authorization`

**Description:** The wire instruction is evaluated against the institution's approval matrix to determine whether manual approval is required. Approval requirements are typically based on wire amount, customer type, beneficiary risk profile, and whether the wire is customer-initiated or institution-initiated. High-value wires, international wires, and wires to high-risk jurisdictions typically require one or more levels of supervisory approval. The approval workflow routes the wire to the appropriate approver(s) and tracks approval status against the wire cutoff deadline.

**Capabilities activated:** `cap.payment_authorization_gating_coordination`, `cap.payment_limit_and_cutoff_management`

**Systems involved:** `sys.wire_transfer_platform`, `sys.payment_processing_platform`

**Participating domains:** `domain.orchestration_control`

**Decision points:**
- Does the wire amount exceed the threshold requiring manual approval?
- How many approval levels are required based on the amount tier and wire type?
- Is the wire to a high-risk jurisdiction requiring additional approval?
- Is the wire approaching the cutoff deadline for same-day value?
- Does the approver have sufficient authority for this wire amount and type?

**Risks:** `risk.approval_bypass_for_high_value_wire`, `risk.approval_delay_causing_cutoff_miss`, `risk.unauthorized_approver`, `risk.collusion_between_initiator_and_approver`

**Controls:** `ctrl.dual_approval_for_high_value_wires`, `ctrl.approval_authority_matrix_enforcement`, `ctrl.cutoff_deadline_escalation`, `ctrl.segregation_of_duties_initiator_approver`, `ctrl.approval_sla_monitoring`

**Evidence generated:** Approval routing record, approver identity and authority level, approval or rejection decision, approval timestamp, cutoff compliance status, rejection rationale if declined

**Handoff:** Wire approved; moves to message formatting and transmission. If rejected, customer notified with rationale.

### Stage: `stage.wire_transfer.message_formatting_and_transmission`

**Description:** The wire transfer platform formats the approved wire instruction into the appropriate message standard: a Fedwire Funds Transfer message for domestic wires or a SWIFT MT103 (or ISO 20022 equivalent) for international wires. The message undergoes format validation and compliance field verification before transmission to the payment network. The source account is debited. The transmission is confirmed through network acknowledgment.

**Capabilities activated:** `cap.wire_transfer_execution`

**Systems involved:** `sys.wire_transfer_platform`, `sys.payment_processing_platform`, `sys.core_banking_platform`

**Decision points:**
- Does the formatted message pass network-specific validation rules?
- Are all compliance-required fields (e.g., originator and beneficiary information for travel rule) populated?
- Is the network connection active and the transmission window open?
- Was the network acknowledgment received confirming acceptance of the message?

**Risks:** `risk.message_formatting_rejection`, `risk.fedwire_or_swift_transmission_failure`, `risk.travel_rule_field_omission`, `risk.network_outage_during_transmission`

**Controls:** `ctrl.message_format_validation_pre_transmission`, `ctrl.travel_rule_field_completeness`, `ctrl.fedwire_swift_transmission_confirmation`, `ctrl.network_availability_check_pre_send`

**Evidence generated:** Formatted wire message, message reference number (IMAD for Fedwire, UETR for SWIFT), format validation result, transmission timestamp, network acknowledgment, source account debit record, fee debit record

**Handoff:** Wire transmitted and acknowledged by network; moves to settlement confirmation. If transmission fails, route to exception handling.

### Stage: `stage.wire_transfer.settlement_confirmation_and_reconciliation`

**Description:** The payment processing platform tracks the wire through settlement. For domestic Fedwire transfers, settlement is real-time and final upon acceptance by the Federal Reserve. For international SWIFT transfers, settlement follows the correspondent banking chain and may involve multiple intermediary credits. Settlement confirmation is matched against the originated wire. The source account debit is finalized, and reconciliation against nostro/vostro statements is performed for international wires.

**Capabilities activated:** `cap.payment_status_and_tracking`

**Systems involved:** `sys.wire_transfer_platform`, `sys.payment_processing_platform`, `sys.settlement_and_reconciliation_system`, `sys.core_banking_platform`

**Participating domains:** `domain.ledger_core`

**Decision points:**
- For Fedwire, was the transfer accepted and settled in real time?
- For SWIFT, has the beneficiary bank confirmed credit to the beneficiary?
- Are there any reconciliation breaks between the originated wire and settlement confirmations?
- For international wires, do nostro statement entries match expected settlement amounts after correspondent fees?

**Risks:** `risk.settlement_delay_or_failure`, `risk.correspondent_fee_deduction_mismatch`, `risk.reconciliation_break_on_international_wire`, `risk.beneficiary_credit_not_confirmed`

**Controls:** `ctrl.settlement_reconciliation_matching`, `ctrl.correspondent_fee_reconciliation`, `ctrl.beneficiary_credit_confirmation_tracking`, `ctrl.settlement_exception_escalation`

**Evidence generated:** Settlement confirmation (Fedwire acknowledgment or SWIFT MT199/camt message), reconciliation match result, nostro statement match for international, settlement timestamp, correspondent fee deductions if applicable

**Handoff:** Settlement confirmed and reconciled. Customer confirmation of completed wire delivered. If settlement exception, route to exception handling.

### Stage: `stage.wire_transfer.exception_repair_and_return_handling`

**Description:** If any stage encounters a failure or the network rejects or returns the wire message, the exception handling path is activated. Common exceptions include message format rejections, network transmission failures, beneficiary bank rejections, and return of funds due to incorrect beneficiary details. The wire transfer platform manages message repairs, re-submissions, and return of funds processing. The source account debit is reversed for returned wires.

**Capabilities activated:** `cap.payment_exception_and_repair_management`

**Systems involved:** `sys.wire_transfer_platform`, `sys.payment_processing_platform`, `sys.core_banking_platform`, `sys.notification_service`

**Participating domains:** `domain.accounts`

**Decision points:**
- Is the exception a repairable message format issue or a fundamental rejection?
- Can the wire be re-submitted after repair within the same processing day?
- Are funds being returned by the beneficiary bank, and what is the return reason?
- Should the source account debit be reversed immediately or held pending investigation?
- Does the exception require customer communication?

**Risks:** `risk.repair_delay_causing_value_date_miss`, `risk.return_of_funds_processing_delay`, `risk.customer_not_notified_of_wire_failure`, `risk.repeated_repair_attempts_without_resolution`

**Controls:** `ctrl.message_repair_workflow_with_sla`, `ctrl.return_of_funds_processing_sla`, `ctrl.customer_exception_notification_requirement`, `ctrl.repair_attempt_limit_with_escalation`

**Evidence generated:** Exception record, rejection or return reason, repair action taken, re-submission record if applicable, return of funds record, reversal record if applicable, customer notification record, escalation record if unresolved

**Handoff:** Exception resolved. Wire either completed through repair and re-submission, funds returned and source account credited, or escalated for manual resolution with customer notification.

## 7. Decision Points

Key decisions across the process:
- Beneficiary details complete and valid for wire type (domestic vs international)
- Network routing: Fedwire vs SWIFT
- OFAC screening: clear vs match requiring investigation
- Fraud risk: approve vs decline vs hold for review
- Approval routing: straight-through vs single approval vs dual approval
- Approval decision: approved vs rejected with rationale
- Wire cutoff: within window vs approaching deadline vs missed
- Message format: pass validation vs requires repair
- Settlement: confirmed vs exception
- Return of funds: reverse debit vs hold pending investigation

## 8. Alternate Outcomes

- Wire declined due to OFAC match pending compliance investigation
- Wire declined due to fraud risk score with customer notification
- Wire rejected by approver with rationale; customer notified
- Wire held pending approval; released after approver action before cutoff
- Wire missed cutoff; queued for next business day with customer notification
- Wire message rejected by network; repaired and re-submitted same day
- Wire message rejected by beneficiary bank; funds returned and customer credited
- International wire delayed by intermediary bank hold; status updated for customer
- Wire fee waived based on customer relationship tier
- Institution-initiated wire processed through expedited approval workflow

## 9. Risks (process-level)

- Beneficiary name-account mismatch causing misrouted funds
- Sanctions evasion through layered intermediary banks
- Wire fraud through social engineering (business email compromise, impersonation)
- Message formatting rejection delaying or preventing transmission
- Network or transmission failure causing value date miss
- Settlement delay or failure on international wires through correspondent chain
- Approval bypass for high-value wires circumventing dual authorization
- Duplicate wire submission creating double payment
- Correspondent fee deductions differing from quoted amount
- Travel rule compliance gaps on international wires

## 10. Controls (process-level)

- Beneficiary validation and confirmation including repeat-beneficiary template matching
- Comprehensive OFAC and sanctions screening of all parties with match investigation workflow
- Wire fraud risk scoring with behavioral analysis and social engineering detection
- Dual approval for high-value wires with segregation of duties enforcement
- Message format validation before transmission with travel rule field completeness
- Fedwire and SWIFT transmission confirmation with network acknowledgment requirement
- Wire duplicate detection based on beneficiary, amount, and time window
- Settlement reconciliation matching with correspondent fee reconciliation
- Cutoff deadline monitoring with escalation for approaching deadlines
- Approval SLA monitoring with automatic escalation

## 11. Evidence / Audit Considerations

This process generates a high-density audit trail critical for regulatory examination:
- wire instruction record with full originator and beneficiary details
- OFAC screening results for all parties (originator, beneficiary, intermediaries)
- fraud risk scoring results and decision records
- approval routing, approver identity, authority verification, and decision records
- formatted wire message content (Fedwire or SWIFT)
- network transmission and acknowledgment records (IMAD, UETR)
- source account debit and fee records
- settlement confirmation and reconciliation records
- nostro/vostro reconciliation for international wires
- exception, repair, and return of funds records
- customer notification records

## 12. Systems Involved (summary)

- payment processing platform
- wire transfer platform
- core banking platform
- customer master / CIF
- sanctions screening engine
- fraud detection engine
- notification service
- settlement and reconciliation system
- digital banking platform

## 13. Provider Participation

Representative providers often adjacent to this process:
- FIS, Fiserv, Jack Henry, Bottomline Technologies, ACI Worldwide, Finastra, SWIFT, Federal Reserve Financial Services, Finzly, Volante Technologies

## 14. Open Questions

- Should domestic and international wire transfers be modeled as separate process variants given the significantly different message formats, screening requirements, and settlement mechanics?
- How should correspondent bank fee transparency requirements (e.g., for SWIFT gpi) be integrated into the confirmation and reconciliation stages?
- Should institution-initiated wires (e.g., for treasury operations) follow the same approval workflow or a distinct expedited path?
- At what point should repeated wire requests to the same high-risk jurisdiction trigger enhanced due diligence beyond transaction-level screening?
- How should the process accommodate the migration from SWIFT MT messages to ISO 20022 MX messages?

## 15. Provenance

This process is the primary operational path for wire transfer origination within `domain.payments`. Wire transfers are the highest-value individual payment type at most institutions and carry elevated regulatory scrutiny due to their speed, finality, and cross-border reach. The process is examination-critical because wire transfer operations are a primary focus area in BSA/AML examinations, with examiners evaluating sanctions screening completeness, approval workflow integrity, fraud controls, and travel rule compliance. The Fedwire Funds Service Operating Circular, SWIFT standards, UCC Article 4A, Regulation J, and OFAC compliance requirements collectively define the regulatory envelope for this process. Wire fraud (particularly business email compromise) is among the highest-loss fraud categories, making the fraud screening and approval stages especially important from a risk management perspective.
