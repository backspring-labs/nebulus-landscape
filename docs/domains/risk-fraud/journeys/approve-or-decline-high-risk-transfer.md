---
id: journey.approve_or_decline_high_risk_transfer
type: journey
name: Approve or Decline High-Risk Transfer
domain_ids:
  - domain.risk_fraud
  - domain.payments
  - domain.accounts
  - domain.customer
capability_ids:
  - cap.payment_and_transfer_risk_decisioning
  - cap.fraud_detection_and_scoring
  - cap.sanctions_and_watchlist_screening
  - cap.transaction_monitoring
  - cap.alert_triage_and_disposition
  - cap.customer_and_entity_risk_rating
  - cap.risk_restriction_and_offboarding_decisioning
value_stream_id: vs.financial_crime_prevention
process_id: proc.high_risk_transfer_decisioning_v1
status: draft
source_refs:
  - src.nacha_fraud_monitoring_rules_2026
  - src.ofac_compliance_framework
  - src.ffiec_bsaaml_manual_2026
  - src.fatf_risk_based_banking_guidance
last_reviewed: 2026-03-27
confidence: high
---

# Approve or Decline High-Risk Transfer

## 1. Objective

Evaluate a payment or funds transfer that has been flagged as high-risk by fraud scoring, sanctions screening, or transaction monitoring rules, and render a timely approval, decline, or hold decision that balances fraud and financial crime prevention with legitimate customer payment needs and payment network SLA requirements.

## 2. Primary Actor

A risk analyst within the institution's fraud operations or payment risk team who is authorized to render real-time or near-real-time decisioning on flagged transfers, or an automated policy engine operating within pre-approved decision boundaries.

## 3. Trigger

A payment or transfer in progress is intercepted by fraud scoring, sanctions screening, or transaction monitoring and flagged as high-risk based on score thresholds, rule triggers, or screening matches that exceed auto-approval tolerances.

## 4. Preconditions

- Fraud scoring models, sanctions screening, and transaction monitoring rules are deployed and actively evaluating payment flows.
- Payment intercept and hold mechanisms are operational across applicable payment rails (wire, ACH, real-time payments, international transfers).
- Risk analysts have access to the payment review queue and the customer, account, and transaction context needed for decisioning.
- Decision authority matrices are defined, specifying which decisions can be automated and which require human review.
- Payment network SLAs and cutoff times are known and tracked to ensure timely disposition.

## 5. Happy Path

### Stage 1 -- Payment intercept and risk flagging
A payment in progress is evaluated by the fraud scoring engine, sanctions screening platform, or transaction monitoring system. The payment exceeds configured risk thresholds and is intercepted -- placed in a held or pending state -- before settlement. The flagged payment is routed to the appropriate review queue with risk context.

**Capabilities activated:** `cap.fraud_detection_and_scoring`, `cap.sanctions_and_watchlist_screening`, `cap.transaction_monitoring`, `cap.payment_and_transfer_risk_decisioning`

### Stage 2 -- Contextual risk review
The risk analyst reviews the payment details (amount, originator, beneficiary, destination country, payment rail) alongside the customer's profile, transaction history, risk rating, and the specific risk signals that triggered the intercept. Prior alerts and cases for the same customer or counterparty are examined.

**Capabilities activated:** `cap.payment_and_transfer_risk_decisioning`, `cap.customer_and_entity_risk_rating`, `cap.alert_triage_and_disposition`

### Stage 3 -- Decision rendering
Based on the risk review, the analyst renders a decision: approve the transfer, decline the transfer, or hold the transfer for further investigation. Approved transfers are released for processing. Declined transfers are blocked with a reason code. Held transfers are escalated to a case for deeper investigation.

**Capabilities activated:** `cap.payment_and_transfer_risk_decisioning`, `cap.alert_triage_and_disposition`, `cap.risk_restriction_and_offboarding_decisioning`

### Stage 4 -- Action execution and customer notification
The decision is executed on the payment platform: release, reject, or hold. The customer is notified as appropriate -- declined transfers require notification with permitted disclosure (without tipping off in SAR-related situations). Approved-with-conditions decisions may trigger enhanced monitoring of subsequent activity.

**Capabilities activated:** `cap.payment_and_transfer_risk_decisioning`, `cap.customer_and_entity_risk_rating`

### Stage 5 -- Documentation and feedback
The decision, rationale, and supporting evidence are recorded in the case or alert record. Decision outcomes are fed back to fraud models and monitoring rules to improve future scoring. Metrics are captured for operational and regulatory reporting.

**Capabilities activated:** `cap.alert_triage_and_disposition`, `cap.fraud_detection_and_scoring`

## 6. Alternate Paths

- Automated decisioning approves or declines the payment without human review when the risk score and payment characteristics fall within pre-approved policy boundaries.
- Payment is flagged by multiple risk systems simultaneously (e.g., both fraud and sanctions), requiring consolidated review across risk types.
- Customer proactively contacts the institution to verify a large or unusual transfer before initiation, reducing intercept friction.
- Payment is part of a batch (e.g., ACH file) and the decision applies to individual items within the batch.
- Correspondent banking payment requires screening of all parties in the payment chain, not just the direct originator and beneficiary.
- Conditional approval is granted with velocity limits or enhanced monitoring applied to the customer for a defined period.

## 7. Failure / Exception Paths

- Decision is not rendered within the payment network's SLA or cutoff time, causing the payment to expire or default to rejection.
- Risk review data is unavailable due to system outage, forcing a conservative hold or decline-by-default.
- Analyst incorrectly approves a fraudulent or sanctions-prohibited transfer, resulting in financial loss or regulatory violation.
- Payment hold mechanism fails and the payment settles before review, requiring post-settlement recovery action.
- Customer dispute arises from a declined legitimate transfer, requiring escalation and potential compensation.
- High review volume during peak periods exceeds analyst capacity, causing queue backlogs and SLA breaches.

## 8. Related Domains

- `domain.risk_fraud` -- owns fraud scoring, sanctions screening, transaction monitoring, and risk decisioning for payments
- `domain.payments` -- owns the payment lifecycle, rails, and settlement mechanics; provides intercept and hold capabilities
- `domain.accounts` -- provides account balance, status, and restriction context; receives restriction actions from risk decisions
- `domain.customer` -- provides customer identity, risk rating, and relationship context for decisioning

## 9. Related Capabilities

- `cap.payment_and_transfer_risk_decisioning`
- `cap.fraud_detection_and_scoring`
- `cap.sanctions_and_watchlist_screening`
- `cap.transaction_monitoring`
- `cap.alert_triage_and_disposition`
- `cap.customer_and_entity_risk_rating`
- `cap.risk_restriction_and_offboarding_decisioning`

## 10. Value Stream Context

This journey is a high-stakes, time-sensitive realization of `vs.financial_crime_prevention`. Every high-risk transfer decision directly impacts fraud loss exposure, sanctions compliance, customer experience, and payment network standing. The speed-accuracy tradeoff in this journey is acute: slow decisions violate payment SLAs and frustrate customers, while fast but inaccurate decisions cause financial loss or regulatory violations. Automation of low-ambiguity decisions is essential to focus human analyst attention on genuinely complex cases.

## 11. Supporting Process Candidates

- `proc.high_risk_transfer_decisioning_v1`
- `proc.payment_intercept_and_hold`
- `proc.real_time_risk_scoring`
- `proc.payment_sanctions_screening`
- `proc.transfer_approval_execution`
- `proc.transfer_decline_notification`
- `proc.post_decision_model_feedback`
- `proc.payment_review_queue_management`

## 12. Key Risks and Controls Encountered

### Key risks
- Time pressure leads to insufficiently rigorous risk review
- Automated decisioning rules are too permissive, allowing fraudulent transfers through
- Automated decisioning rules are too conservative, blocking legitimate customer activity at scale
- Sanctions screening miss allows a prohibited transfer to settle
- Payment hold mechanism fails, allowing settlement before review
- Analyst lacks sufficient context to make an informed decision within SLA
- Declined legitimate transfers damage customer relationships and generate complaints

### Key controls
- SLA monitoring with escalation for approaching deadlines
- Decision authority matrices defining human vs. automated decision boundaries
- Dual-authorization for high-value or high-risk transfer approvals
- Real-time sanctions screening with fail-closed defaults
- Payment hold verification before marking a transaction as held
- Customer risk context pre-assembly to minimize analyst review time
- Decision outcome tracking and periodic backtesting
- Segregation of duties between risk decisioning and payment operations
- Complaint and false-decline monitoring to calibrate decision thresholds

## 13. Typical Systems and Providers Involved

### Typical systems
- Fraud scoring and decisioning engine
- Sanctions and watchlist screening platform
- Transaction monitoring system
- Payment processing and intercept platform
- Alert and review queue management system
- Customer risk profile repository
- Payment network gateway and messaging system

### Representative providers
- Featurespace (adaptive behavioral analytics)
- NICE Actimize (fraud and AML)
- Feedzai (AI-based payment fraud)
- Fircosoft / LexisNexis (sanctions screening)
- FIS / Fiserv / Jack Henry (payment processing)
- Bottomline Technologies (payment security)
- ACI Worldwide (real-time payment fraud)

## 14. Open Questions

- What percentage of high-risk transfer decisions should be fully automated versus requiring human review?
- How should the institution handle real-time payment rails (e.g., FedNow, RTP) where the decision window is measured in seconds?
- Should the institution implement customer self-service pre-authorization for expected large transfers to reduce false intercepts?
- How should cross-border transfer risk be weighted differently from domestic transfer risk in the scoring model?
- What feedback mechanisms should exist for customers to dispute declined transfers, and how should those disputes influence future scoring?

## 15. Provenance

This journey represents the real-time risk decisioning point where fraud prevention, sanctions compliance, and customer experience converge on a single payment. It is one of the highest-consequence journeys in the Risk & Fraud domain because decisions are irreversible once a payment settles. Grounded in NACHA fraud monitoring rules (2026), OFAC compliance framework requirements for payment screening, FFIEC BSA/AML Examination Manual (2026), and FATF risk-based approach guidance for banking sector payment controls.
