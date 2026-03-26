---
id: cap.fraud_detection_and_scoring
type: capability
name: Fraud Detection & Scoring
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Fraud Detection & Scoring

## 1. Definition

Fraud Detection & Scoring is the enduring business ability to detect, score, and prioritize potential fraud events across application, transaction, and account activity channels using rules, models, and behavioral analytics to enable timely interdiction, investigation, and loss prevention.

## 2. Business Outcome

Ensure the institution can identify fraudulent activity in real time or near-real time, minimize fraud losses, protect customers from unauthorized activity, and maintain regulatory compliance while keeping false-positive rates low enough to avoid excessive customer friction and operational burden.

## 3. Scope

### Included activities
- Scoring transactions for fraud risk in real time and batch
- Scoring applications for fraud indicators (synthetic identity, first-party fraud, identity theft)
- Detecting behavioral anomalies across account activity, login patterns, and device signals
- Generating and prioritizing fraud alerts based on score thresholds and rule triggers
- Triaging and dispositioning fraud alerts (confirmed fraud, false positive, escalation)
- Maintaining and tuning fraud detection rules and model parameters
- Monitoring fraud model performance (detection rates, false positive rates, model drift)
- Supporting account takeover detection through behavioral and device analytics

### Excluded activities
- Fraud investigation case management and resolution (`domain.risk_fraud` investigation capabilities)
- Customer notification and communication about suspected fraud (`domain.servicing` / `domain.communications`)
- Transaction blocking or hold execution (`domain.payments` / `domain.transactions`)
- Chargeback and dispute processing (`domain.payments`)
- SAR/STR filing for fraud-related suspicious activity (`domain.risk_fraud` reporting capabilities)
- Identity proofing and verification (`domain.identity_access`)
- Sanctions and watchlist screening (`cap.sanctions_and_watchlist_screening`)
- Onboarding risk decisioning (`cap.identity_and_onboarding_risk_decisioning`)

## 4. Upstream Dependencies
- Transaction data from `domain.payments` / `domain.transactions`
- Application data from `cap.customer_application_intake`
- Customer profile and behavioral history from `domain.customer`
- Device and session data from `cap.session_and_device_trust_management`
- Authentication events from `cap.authentication`
- Fraud policy thresholds and rule parameters from `cap.financial_crime_risk_appetite_and_policy_management`
- External fraud intelligence feeds (consortium data, threat intelligence)

## 5. Downstream Dependencies
- `cap.identity_and_onboarding_risk_decisioning` for application-stage fraud signal consumption
- Fraud investigation and case management capabilities for confirmed or escalated alerts
- `domain.payments` / `domain.transactions` for real-time transaction interdiction signals
- `cap.customer_and_entity_risk_rating` for fraud-informed risk factor input
- Regulatory reporting for fraud statistics and SAR triggers
- Customer communication capabilities for fraud notification

## 6. Related Value Streams
- `vs.fraud_prevention`
- `vs.payment_integrity`
- `vs.customer_onboarding`
- `vs.trusted_customer_relationship`

## 7. Related Journeys
- `journey.real_time_transaction_fraud_check`
- `journey.application_fraud_review`
- `journey.fraud_alert_investigation`
- `journey.account_takeover_detection`
- `journey.fraud_case_resolution`

## 8. Likely Processes
- Transaction fraud scoring (real-time and batch)
- Application fraud scoring and indicator detection
- Behavioral anomaly detection and profiling
- Fraud alert generation and prioritization
- Fraud alert triage and disposition
- Fraud model and rule tuning
- Fraud model performance monitoring and back-testing
- Fraud detection coverage gap analysis
- Consortium data integration and intelligence sharing

## 9. Risks
- Undetected fraud events result in financial losses and customer harm
- Excessive false positives create customer friction, transaction delays, and operational overload
- Model drift reduces detection accuracy over time without adequate monitoring
- Delayed fraud interdiction allows fraud to complete before detection signals are acted upon
- Fraud detection coverage gaps leave certain channels, products, or transaction types unmonitored
- Over-reliance on rules without model-based detection misses novel fraud typologies
- Inadequate account takeover detection fails to protect existing customer relationships
- Fraud scoring latency exceeds real-time transaction processing SLAs

## 10. Controls
- Fraud model validation and periodic back-testing
- Fraud rule change governance with approval workflows and testing requirements
- Alert volume and quality monitoring with trend analysis
- Interdiction timeliness SLA monitoring for real-time scoring paths
- Fraud detection coverage review across channels, products, and transaction types
- Fraud score threshold calibration with regular tuning cycles
- Model performance metrics dashboard (detection rate, false positive rate, precision, recall)
- Segregation of duties between model development, validation, and production deployment
- Consortium data quality and timeliness validation

## 11. Typical Systems/Providers

### Typical systems
- Real-time fraud detection engines
- Fraud case management platforms
- Behavioral analytics and profiling platforms
- Fraud model governance and MLOps platforms
- Device intelligence and fingerprinting services
- Consortium data integration platforms
- Fraud analytics dashboards and reporting tools

### Typical providers
- Featurespace
- NICE Actimize
- FICO (Falcon)
- SAS
- Feedzai
- LexisNexis Risk Solutions (ThreatMetrix)
- BioCatch
- Sardine
- In-house fraud detection platforms

## 12. Open Questions
- Should application fraud detection and transaction fraud detection be modeled as separate capabilities given their different data inputs, latency requirements, and operational patterns?
- How should account takeover detection be bounded when it overlaps significantly with `cap.session_and_device_trust_management` and `cap.authentication`?
- Where should the boundary sit between fraud detection/scoring and fraud investigation if investigation is later modeled as its own capability?
- Should consortium data management and fraud intelligence sharing be embedded here or in a separate intelligence capability?
- How should this capability relate to scam detection, which involves different behavioral patterns and victim-versus-perpetrator dynamics?

## 13. Provenance

This capability reflects the core fraud prevention function required across all financial institutions. It is intentionally focused on detection and scoring rather than investigation or resolution, because the detection function has distinct real-time processing requirements, model governance needs, and operational patterns. The capability spans application fraud, transaction fraud, and account takeover detection because these share common analytical infrastructure and model governance requirements, though they may be separated as the model matures to reflect different operational rhythms.
