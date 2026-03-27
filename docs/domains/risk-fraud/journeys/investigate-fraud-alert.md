---
id: journey.investigate_fraud_alert
type: journey
name: Investigate Fraud Alert
domain_ids:
  - domain.risk_fraud
  - domain.customer
  - domain.accounts
  - domain.payments
capability_ids:
  - cap.fraud_detection_and_scoring
  - cap.alert_triage_and_disposition
  - cap.case_investigation_management
  - cap.transaction_monitoring
  - cap.account_and_relationship_surveillance
  - cap.fraud_rules_and_model_governance
  - cap.suspicious_activity_reporting
value_stream_id: vs.financial_crime_prevention
process_id: proc.fraud_alert_investigation_v1
status: draft
source_refs:
  - src.ffiec_bsaaml_manual_2026
  - src.ffiec_suspicious_activity_monitoring_appendix_s
  - src.fincen_sar_faq_2025
  - src.nacha_fraud_monitoring_rules_2026
last_reviewed: 2026-03-27
confidence: high
---

# Investigate Fraud Alert

## 1. Objective

Investigate a system-generated fraud alert to determine whether the flagged activity represents actual fraud, requires escalation for deeper investigation, or can be dismissed as a false positive -- and to take appropriate protective action within regulatory and policy-defined timelines.

## 2. Primary Actor

A fraud analyst within the institution's financial crimes or fraud operations team who holds case investigation authority and access to relevant transaction, customer, and account data.

## 3. Trigger

A fraud detection engine or transaction monitoring system generates an alert based on rule triggers, model scores, or anomaly detection that exceeds configured thresholds.

## 4. Preconditions

- Fraud detection models and rules are deployed, tuned, and actively scoring transactions and activity.
- An alert management queue and case management system are operational and accessible.
- The analyst has appropriate entitlements and access to customer, account, and transaction data needed for investigation.
- Alert routing and assignment rules are configured to distribute alerts to qualified analysts.
- Regulatory timelines for investigation and disposition are defined and tracked.

## 5. Happy Path

### Stage 1 -- Alert receipt and initial triage
The fraud alert appears in the analyst's queue with a severity score, triggering rule or model identifier, and summary of the flagged activity. The analyst reviews the alert metadata, score, and initial context to determine priority and whether the alert warrants full investigation or can be quickly resolved.

**Capabilities activated:** `cap.fraud_detection_and_scoring`, `cap.alert_triage_and_disposition`

### Stage 2 -- Activity and context review
The analyst examines the underlying transactions, account history, customer profile, device context, and behavioral patterns associated with the alert. Related alerts and prior cases for the same customer or account are reviewed to identify patterns.

**Capabilities activated:** `cap.transaction_monitoring`, `cap.account_and_relationship_surveillance`, `cap.case_investigation_management`

### Stage 3 -- Evidence gathering and documentation
The analyst assembles relevant evidence including transaction records, communication logs, third-party data, and screenshots. Investigation notes and findings are documented in the case record according to institutional standards.

**Capabilities activated:** `cap.case_investigation_management`, `cap.fraud_rules_and_model_governance`

### Stage 4 -- Disposition decision
Based on the investigation, the analyst renders a disposition: confirmed fraud, escalated to senior investigator, or dismissed as false positive. Confirmed fraud triggers protective actions (account restrictions, card blocks, customer notification) and potential SAR filing. False positives are documented with rationale to inform model tuning.

**Capabilities activated:** `cap.alert_triage_and_disposition`, `cap.case_investigation_management`, `cap.suspicious_activity_reporting`

### Stage 5 -- Case closure and feedback loop
The case is closed with a final disposition, and feedback is provided to the fraud detection system to improve future scoring accuracy. Metrics are recorded for regulatory reporting and operational dashboards.

**Capabilities activated:** `cap.fraud_rules_and_model_governance`, `cap.case_investigation_management`

## 6. Alternate Paths

- Alert is auto-dispositioned by policy rules without analyst review when the score falls below a defined threshold and the activity matches a known benign pattern.
- Alert is combined with other alerts into a consolidated case when multiple alerts relate to the same actor or scheme.
- Alert investigation reveals activity that crosses into AML/BSA territory, requiring handoff to the BSA team.
- Customer contacts the institution to report fraud before the alert is generated, creating a customer-initiated investigation path.
- Real-time fraud intercept blocks a transaction before settlement, and the alert becomes a post-action review.

## 7. Failure / Exception Paths

- Alert data is incomplete or enrichment services are unavailable, preventing adequate triage.
- Investigation exceeds regulatory or policy-defined timelines, triggering escalation and compliance notification.
- Analyst lacks access to required data due to entitlement gaps or system outages.
- Disposition decision is overturned on quality review, requiring case reopening.
- Protective actions (account blocks, card holds) fail to execute, leaving the customer exposed.
- Case management system is unavailable, forcing manual tracking and creating audit risk.

## 8. Related Domains

- `domain.risk_fraud` -- owns alert generation, triage, investigation, and disposition lifecycle
- `domain.customer` -- provides canonical customer identity and relationship context for investigation
- `domain.accounts` -- provides account status, history, and restriction capabilities for protective actions
- `domain.payments` -- provides transaction detail and payment channel context for the flagged activity

## 9. Related Capabilities

- `cap.fraud_detection_and_scoring`
- `cap.alert_triage_and_disposition`
- `cap.case_investigation_management`
- `cap.transaction_monitoring`
- `cap.account_and_relationship_surveillance`
- `cap.fraud_rules_and_model_governance`
- `cap.suspicious_activity_reporting`

## 10. Value Stream Context

This journey is a core realization of `vs.financial_crime_prevention`. Every fraud alert that is not investigated or is investigated poorly represents direct financial loss, regulatory exposure, and customer harm. The speed and quality of alert investigation directly determines the institution's fraud loss rate, false positive burden on operations, and ability to meet SAR filing obligations.

## 11. Supporting Process Candidates

- `proc.fraud_alert_investigation_v1`
- `proc.alert_triage_and_routing`
- `proc.case_evidence_assembly`
- `proc.fraud_disposition_and_protective_action`
- `proc.sar_referral_evaluation`
- `proc.model_feedback_submission`
- `proc.alert_quality_review`

## 12. Key Risks and Controls Encountered

### Key risks
- Analyst fatigue leads to missed true positives among high false-positive volume
- Investigation delay allows continued fraudulent activity and increased losses
- Incomplete documentation fails regulatory examination or quality review
- Model drift causes fraud scoring to degrade without detection
- Protective actions applied incorrectly restrict legitimate customer activity
- Confidential investigation details leaked to the subject of the investigation

### Key controls
- SLA monitoring and escalation for investigation timelines
- Dual-review or quality assurance sampling of disposition decisions
- Mandatory documentation standards enforced by case management workflow
- Model performance monitoring with periodic backtesting and validation
- Segregation of duties between alert generation and disposition
- Audit trail for all investigation actions and data access
- Tipping-off prevention controls on customer-facing communications

## 13. Typical Systems and Providers Involved

### Typical systems
- Fraud detection and scoring engine
- Alert management and routing platform
- Case management system
- Transaction monitoring platform
- Customer and account data repositories
- SAR filing and regulatory reporting system
- Model governance and performance monitoring platform

### Representative providers
- NICE Actimize
- Featurespace
- SAS Fraud Management
- Feedzai
- Verafin (now Nasdaq)
- Pega Financial Crime
- Palantir Foundry

## 14. Open Questions

- How should the institution balance automated alert disposition against regulatory expectations for human review?
- What is the appropriate threshold for auto-closing low-score alerts without analyst review?
- How should real-time fraud intercept decisions integrate with the post-action investigation journey?
- Should machine learning model explainability outputs be required as part of the investigation record?
- How should cross-channel fraud patterns (e.g., card + ACH + wire) be consolidated into a single investigation?

## 15. Provenance

This journey represents the central investigative workflow of the Risk & Fraud domain. It is triggered by outputs of fraud detection and transaction monitoring capabilities and may feed into the SAR filing journey. Grounded in FFIEC BSA/AML Examination Manual (2026), FFIEC Appendix S on suspicious activity monitoring, FinCEN SAR FAQ (2025), and NACHA fraud monitoring rules (2026).
