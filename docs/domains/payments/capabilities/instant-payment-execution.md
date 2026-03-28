---
id: cap.instant_payment_execution
type: capability
name: Instant Payment Execution
domain_id: domain.payments
status: draft
source_refs: []
last_reviewed: 2026-03-27
confidence: high
---

# Instant Payment Execution

## 1. Definition

Instant Payment Execution is the enduring business ability to execute real-time payment instructions through instant payment networks such as RTP (The Clearing House) and FedNow (Federal Reserve), including message construction, real-time credit transfer, request-for-payment processing, immediate confirmation, and 24/7/365 availability management.

## 2. Business Outcome

Ensure the institution can send and receive irrevocable, real-time payments around the clock with immediate funds availability, supporting customer demand for speed and certainty, enabling new use cases such as gig economy payouts and request-to-pay, and maintaining competitive parity as instant payments become a baseline customer expectation.

## 3. Scope

### Included activities
- Constructing ISO 20022 messages for RTP and FedNow submission
- Submitting payment messages to instant payment networks in real time
- Processing network responses (acceptance, rejection, timeout) and updating payment status
- Receiving and posting inbound instant payments with immediate funds availability
- Processing request-for-payment (RfP) messages and enabling customer response workflows
- Managing prefunding positions and liquidity with the instant payment networks
- Monitoring 24/7/365 system availability and network connectivity health
- Handling instant payment message validation and enrichment
- Supporting rich remittance data and structured payment information
- Managing instant payment fee assessment and pricing

### Excluded activities
- Payment initiation and instruction capture (`cap.payment_initiation_management`)
- Payment instruction validation (`cap.payment_instruction_validation`)
- Payment routing and rail selection (`cap.payment_routing_and_rail_selection`)
- Payment authorization and approval workflows (`cap.payment_authorization_gating_coordination`)
- Real-time fraud detection rule definition and model management (`cap.payment_and_transfer_risk_decisioning`)
- ACH, wire, or internal transfer execution (separate capabilities)
- P2P payment network integration (e.g., Zelle) as a distinct capability
- Account balance management (`domain.accounts`)

## 4. Upstream Dependencies
- Validated and authorized payment instruction from upstream payment capabilities
- Real-time fraud screening results from `cap.payment_and_transfer_risk_decisioning`
- Account balance and availability data from `domain.accounts`
- Limit and cutoff validation from `cap.payment_limit_and_cutoff_management`
- Payee and account validation data from `cap.payee_and_external_account_setup_management`
- Network prefunding position data from treasury functions
- RTP and FedNow network connectivity and participant directory

## 5. Downstream Dependencies
- `domain.accounts` for immediate balance updates on both send and receive
- `domain.finance` for settlement entries and prefunding position reconciliation
- `domain.customer` for real-time payment confirmation and notification delivery
- Request-for-payment response workflows for customer-facing channels
- Reporting and analytics for instant payment volumes, success rates, and latency
- Liquidity management for prefunding position monitoring and replenishment

## 6. Related Value Streams
- `vs.payment_execution`
- `vs.customer_self_service`
- `vs.revenue_generation`
- `vs.competitive_differentiation`

## 7. Related Journeys
- `journey.customer_instant_payment`
- `journey.business_instant_payment`
- `journey.request_for_payment_receipt`
- `journey.instant_payment_receipt`
- `journey.gig_economy_instant_payout`

## 8. Likely Processes
- ISO 20022 message construction for RTP and FedNow
- Real-time payment message submission to network
- Network response processing and payment status update
- Inbound instant payment receipt and immediate posting
- Request-for-payment message processing and customer workflow orchestration
- Prefunding position monitoring and replenishment triggering
- 24/7/365 availability monitoring and failover management
- Instant payment message validation and enrichment
- Instant payment fee calculation and assessment
- Instant payment reconciliation between network settlement and internal ledger
- Instant payment timeout and exception handling

## 9. Risks
- Irrevocability of instant payments amplifying the impact of fraud or errors with no recall mechanism
- Fraud velocity risk as real-time execution eliminates batch-cycle review windows
- Liquidity shortfall in prefunding position causing payment rejection at the network level
- Network unavailability or connectivity loss preventing payment execution during 24/7 operations
- Message rejection due to format or data errors causing customer-facing failures
- Latency in fraud screening creating unacceptable delays in the real-time payment flow
- Inbound instant payment posting failure leaving funds in suspense
- Request-for-payment abuse or social engineering exploiting RfP workflows

## 10. Controls
- Real-time fraud screening integrated into the payment flow with sub-second SLA enforcement
- Prefunding balance check and automated replenishment triggering before position depletion
- Velocity limit enforcement tracking transaction count and amount over configurable windows
- Network health monitoring with automated alerting and failover activation
- ISO 20022 message validation before network submission
- Instant payment timeout handling with clear customer communication and retry logic
- Inbound instant payment posting verification and suspense monitoring
- Request-for-payment authentication and customer verification before response processing

## 11. Typical Systems/Providers

### Typical systems
- RTP network interface and messaging gateway
- FedNow network interface and messaging gateway
- Instant payment processing engines
- Real-time fraud screening engines
- Instant payment liquidity and prefunding managers
- ISO 20022 message transformation platforms

### Typical providers
- The Clearing House (RTP)
- Federal Reserve (FedNow)
- FIS
- Fiserv
- Jack Henry
- ACI Worldwide
- Volante Technologies

## 12. Open Questions
- How should the capability handle the coexistence of RTP and FedNow, including network selection logic and potential redundancy for resilience?
- Where should the boundary be drawn between instant payment execution and P2P payment capabilities (e.g., Zelle) that may use instant payment rails as underlying infrastructure?
- How should request-for-payment workflows be scoped — as part of instant payment execution or as a distinct receivables/billing capability?
- What SLA thresholds should define acceptable fraud screening latency within the real-time payment flow?
- How should the capability evolve to support potential future features like instant payment request-for-return or enhanced dispute resolution mechanisms?
- Should instant payment execution include support for alias-based routing (e.g., pay-by-email, pay-by-phone) or should that be a separate directory service capability?

## 13. Provenance

This capability reflects the transformative shift in U.S. payments toward real-time, 24/7/365 fund transfers driven by the launch of RTP (2017) and FedNow (2023). Instant payments fundamentally change execution requirements compared to batch-oriented rails like ACH: irrevocability demands pre-execution fraud screening, 24/7 availability requires always-on infrastructure, and real-time settlement necessitates active liquidity management. The capability is scoped to the execution phase — message construction, network submission, response handling, and settlement — with deliberate separation from initiation, validation, and routing. This isolation reflects the unique operational demands of instant payment networks, including sub-second processing requirements, prefunding mechanics, and the need for specialized expertise in ISO 20022 messaging and real-time network integration.
