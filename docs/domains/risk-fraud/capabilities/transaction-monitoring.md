---
id: cap.transaction_monitoring
type: capability
name: Transaction Monitoring
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Transaction Monitoring

## 1. Definition

Transaction Monitoring is the enduring business ability to systematically surveil customer transactions across channels, products, and counterparties to detect patterns and behaviors indicative of money laundering, terrorist financing, fraud, and other financial crimes through rules-based scenarios, analytics-driven models, and threshold-based surveillance executed in batch and near-real-time modes.

## 2. Business Outcome

Ensure the institution can identify suspicious transaction activity that may indicate financial crime, generate timely and actionable alerts for investigation, satisfy regulatory expectations for ongoing monitoring, and maintain detection effectiveness across evolving crime typologies while managing alert volumes to sustainable operational levels.

## 3. Scope

### Included activities
- Executing rules-based transaction monitoring scenarios across all customer segments and products
- Running batch and near-real-time transaction surveillance against defined thresholds and behavioral patterns
- Generating alerts when transactions or transaction patterns exceed monitoring thresholds
- Enriching transaction data with customer context, counterparty information, and risk indicators prior to scenario execution
- Calibrating and tuning monitoring scenarios and thresholds based on risk appetite, typology evolution, and alert quality metrics
- Monitoring transaction monitoring system coverage across products, channels, and customer segments
- Managing transaction monitoring scenario inventories, including scenario documentation and rationale
- Supporting currency transaction reporting (CTR) and large-value transaction identification

### Excluded activities
- Alert triage, enrichment, and disposition (`cap.alert_triage_and_disposition`)
- Investigation and case management for suspicious activity (investigation capabilities within `domain.risk_fraud`)
- SAR/STR filing and regulatory reporting (`domain.risk_fraud` reporting capabilities)
- Real-time payment interdiction and hold decisioning (`cap.payment_and_transfer_risk_decisioning`)
- Fraud detection and scoring for individual transaction authorization (`cap.fraud_detection_and_scoring`)
- Sanctions and watchlist screening (`cap.sanctions_and_watchlist_screening`)
- Customer risk rating and periodic review (`cap.customer_and_entity_risk_rating`, `cap.enhanced_due_diligence_and_periodic_review_decisioning`)

## 4. Upstream Dependencies
- Transaction data from `domain.payments` / `domain.transactions` across all channels and products
- Customer profile, risk rating, and segmentation data from `domain.customer` and `cap.customer_and_entity_risk_rating`
- Account information and account status from `domain.accounts`
- Counterparty and beneficiary data from payment instructions
- Geographic risk data and country risk classifications
- Monitoring scenario definitions and threshold parameters from `cap.financial_crime_risk_appetite_and_policy_management`
- Watchlist and sanctions data for counterparty context enrichment

## 5. Downstream Dependencies
- `cap.alert_triage_and_disposition` for alert review and disposition
- Investigation and case management capabilities for escalated alerts
- Regulatory reporting capabilities for SAR/STR triggers and CTR generation
- `cap.customer_and_entity_risk_rating` for transaction-informed risk factor input
- `cap.enhanced_due_diligence_and_periodic_review_decisioning` for monitoring-triggered reviews
- Management and board reporting for transaction monitoring program effectiveness metrics

## 6. Related Value Streams
- `vs.financial_crime_detection`
- `vs.payment_integrity`
- `vs.regulatory_compliance`
- `vs.trusted_customer_relationship`

## 7. Related Journeys
- `journey.suspicious_transaction_detection`
- `journey.transaction_alert_review`
- `journey.transaction_pattern_investigation`
- `journey.batch_transaction_surveillance`
- `journey.real_time_transaction_screening`

## 8. Likely Processes
- Transaction data ingestion, normalization, and enrichment
- Batch transaction monitoring scenario execution
- Near-real-time transaction surveillance execution
- Transaction alert generation and prioritization
- Monitoring scenario threshold calibration and tuning
- Transaction monitoring coverage assessment across products and channels
- Scenario inventory management and documentation
- Currency transaction report identification and generation
- Transaction monitoring performance reporting and metrics
- Scenario development and back-testing for new typologies

## 9. Risks
- Undetected suspicious transaction patterns result in regulatory violations and financial crime facilitation
- Excessive alert volumes overwhelm investigation teams and degrade triage quality
- Transaction monitoring coverage gaps leave products, channels, or customer segments unmonitored
- Stale or poorly calibrated scenarios fail to detect evolving financial crime typologies
- Transaction data quality issues cause false negatives or unreliable scenario execution
- Monitoring latency delays alert generation beyond useful investigation windows
- Scenario duplication or overlap generates redundant alerts and wastes investigative resources
- Inadequate scenario documentation fails to demonstrate regulatory rationale for monitoring approach

## 10. Controls
- Transaction monitoring scenario validation and periodic back-testing
- Scenario threshold calibration governance with documented rationale
- Alert volume and quality trend monitoring with feedback loops to tuning
- Transaction monitoring coverage assessment across all products, channels, and segments
- Transaction data completeness and quality checks prior to scenario execution
- Monitoring system availability and processing timeliness SLA tracking
- Scenario inventory governance with periodic relevance review
- Suspicious activity detection timeliness metrics
- Independent validation of scenario effectiveness and coverage

## 11. Typical Systems/Providers

### Typical systems
- Transaction monitoring engines (batch and near-real-time)
- Transaction data warehouses and staging environments
- Alert management platforms
- Scenario tuning and governance tools
- Transaction monitoring analytics and reporting dashboards
- Data quality monitoring tools

### Typical providers
- NICE Actimize
- Oracle Financial Services (Mantas)
- SAS
- Verafin
- Fiserv
- Featurespace
- Napier AI
- In-house transaction monitoring platforms

## 12. Open Questions
- How should the boundary between transaction monitoring (pattern detection over time) and real-time fraud scoring (per-transaction decisioning) be drawn when modern platforms increasingly blend both approaches?
- Should currency transaction reporting be embedded within this capability or modeled separately as a regulatory reporting function?
- How should transaction monitoring for trade finance, securities, and other non-payment transaction types be scoped relative to this capability?
- Where should the boundary sit between transaction monitoring scenario governance and the broader financial crime risk appetite and policy management capability?
- How should this capability address the growing regulatory expectation for real-time or near-real-time monitoring when most existing platforms are batch-oriented?

## 13. Provenance

This capability reflects the core AML/CFT transaction monitoring function required by regulation across all financial institutions. It is deliberately scoped to the detection and alert-generation phase of the monitoring lifecycle, excluding alert triage and investigation, because the detection function has distinct technical infrastructure, scenario governance requirements, and operational cadences. The capability encompasses both batch and near-real-time monitoring because these share common scenario governance and tuning processes, though their technical implementations differ significantly. Transaction monitoring is distinguished from fraud detection by its focus on patterns over time rather than per-transaction authorization decisioning, though the boundary is blurring as platforms converge.
