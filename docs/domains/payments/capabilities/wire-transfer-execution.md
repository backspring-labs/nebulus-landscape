---
id: cap.wire_transfer_execution
type: capability
name: Wire Transfer Execution
domain_id: domain.payments
status: draft
source_refs: []
last_reviewed: 2026-03-27
confidence: high
---

# Wire Transfer Execution

## 1. Definition

Wire Transfer Execution is the enduring business ability to execute domestic and international wire transfer instructions through Fedwire, CHIPS, and SWIFT networks, including message formatting, correspondent bank routing, sanctions and compliance screening integration, settlement, confirmation delivery, and wire investigation and repair.

## 2. Business Outcome

Ensure the institution can reliably send and receive high-value and time-critical wire transfers across domestic and international networks, meeting customer expectations for speed and certainty while maintaining strict compliance with sanctions screening, regulatory reporting, and network-specific message standards.

## 3. Scope

### Included activities
- Formatting wire messages to Fedwire, CHIPS, and SWIFT standards
- Routing international wires through correspondent banking relationships
- Integrating with sanctions screening engines and holding wires pending screening results
- Executing domestic wires via Fedwire with real-time gross settlement
- Processing incoming domestic and international wires
- Managing wire settlement and funding, including nostro/vostro account management
- Delivering wire confirmations to originators and beneficiaries
- Handling wire repairs, amendments, and investigation requests (SWIFT gpi tracking)
- Managing wire cutoff times and deadline enforcement
- Supporting standing wire instructions and repetitive wire templates

### Excluded activities
- Payment initiation and instruction capture (`cap.payment_initiation_management`)
- Payment instruction validation (`cap.payment_instruction_validation`)
- Payment routing and rail selection (`cap.payment_routing_and_rail_selection`)
- Payment authorization and approval workflows (`cap.payment_authorization_gating_coordination`)
- Sanctions screening rule definition and list management (`domain.compliance`)
- ACH, instant payment, or internal transfer execution (separate capabilities)
- Foreign exchange rate pricing and hedging (`domain.treasury`)
- Correspondent bank relationship management (`domain.treasury`)

## 4. Upstream Dependencies
- Validated and authorized wire instruction from upstream payment capabilities
- Sanctions screening engine availability from `domain.compliance`
- Correspondent bank routing tables and BIC/SWIFT directory
- Account and beneficiary data from `cap.payee_and_external_account_setup_management`
- FX rate and conversion data for international wires from `domain.treasury`
- Limit and cutoff validation from `cap.payment_limit_and_cutoff_management`
- Fedwire, CHIPS, and SWIFT network connectivity

## 5. Downstream Dependencies
- `domain.accounts` for debit posting to originator account and credit posting for incoming wires
- `domain.finance` for settlement entries, nostro/vostro reconciliation, and general ledger
- `domain.customer` for wire confirmation delivery and status updates
- `domain.compliance` for regulatory reporting (CTR, CMIR, OFAC reporting)
- Wire investigation and repair workflows for operations teams
- Reporting and analytics for wire volumes, fees, and compliance metrics

## 6. Related Value Streams
- `vs.payment_execution`
- `vs.revenue_generation`
- `vs.regulatory_compliance`
- `vs.customer_self_service`

## 7. Related Journeys
- `journey.customer_domestic_wire`
- `journey.customer_international_wire`
- `journey.business_high_value_wire`
- `journey.incoming_wire_receipt`
- `journey.wire_repair_and_investigation`

## 8. Likely Processes
- Wire message formatting for Fedwire, CHIPS, and SWIFT standards
- Correspondent bank routing and intermediary bank selection
- Sanctions screening integration and hold-pending-screening workflow
- Wire settlement and funding execution
- Wire confirmation generation and delivery to originator and beneficiary
- Incoming wire receipt, processing, and beneficiary account posting
- Wire repair and amendment handling
- Wire investigation request processing (SWIFT gpi, Fedwire inquiry)
- Wire cutoff time monitoring and enforcement
- Nostro/vostro account reconciliation for international wires
- Wire fee calculation and assessment

## 9. Risks
- Sanctions screening miss allowing a prohibited wire to execute, resulting in severe regulatory penalties
- Correspondent bank routing error causing wire to be misdirected or delayed
- Irrevocable erroneous wire transfer sent to wrong beneficiary with limited recovery options
- Wire fraud via social engineering (business email compromise) bypassing verification controls
- Wire cutoff time missed causing payment to settle on the next business day
- Wire message format rejection by the network, delaying payment
- International wire delays due to correspondent bank processing or compliance holds
- Nostro/vostro reconciliation breaks causing balance discrepancies
- Wire confirmation delivery failure leaving customer uninformed of completion status

## 10. Controls
- Sanctions screening completion gate preventing wire release until screening is fully cleared
- Dual approval requirement for high-value wires above configured thresholds
- Correspondent bank routing validation against approved routing tables
- Wire cutoff time enforcement with automated holds for late submissions
- Wire message format validation against network-specific standards before submission
- Beneficiary confirmation (callback or verification) for new or modified wire instructions
- Wire investigation tracking and SLA monitoring for repair and amendment requests
- Incoming wire screening and posting verification before customer account credit
- Wire fee transparency and disclosure verification before execution

## 11. Typical Systems/Providers

### Typical systems
- Wire transfer platforms and execution engines
- SWIFT Alliance interfaces and messaging gateways
- Fedwire connection and message processing systems
- Sanctions screening engines
- Wire investigation and repair workbenches
- Correspondent bank directory and routing systems

### Typical providers
- FIS
- Fiserv
- Jack Henry
- SWIFT
- Federal Reserve (Fedwire)
- Bottomline Technologies
- Finastra

## 12. Open Questions
- How should the capability evolve as SWIFT migrates to ISO 20022 message formats and richer structured data becomes available for compliance screening and straight-through processing?
- Where should the boundary be drawn between wire execution and correspondent banking management, given the tight coupling between routing decisions and correspondent relationships?
- How should the capability handle multi-bank wire processing for institutions with multiple Fedwire or SWIFT BICs?
- Should wire drawdown requests (customer-initiated incoming wires) be scoped as part of this capability or treated as a distinct inbound payment type?
- How should the capability accommodate emerging cross-border payment networks that may supplement or compete with traditional correspondent banking?

## 13. Provenance

This capability reflects the critical role wire transfers play as the primary mechanism for high-value, time-sensitive, and irrevocable fund transfers in both domestic and international contexts. Wire execution demands rigorous compliance integration — particularly sanctions screening — given the irrevocable nature of wire payments and the severe regulatory consequences of compliance failures. The capability is scoped to the execution phase, encompassing message formatting, network submission, settlement, and post-execution activities like confirmations and investigations. This separation from initiation, validation, and authorization reflects the specialized expertise required for network-specific message standards, correspondent banking mechanics, and real-time gross settlement coordination.
