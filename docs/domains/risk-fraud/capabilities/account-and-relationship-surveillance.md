---
id: cap.account_and_relationship_surveillance
type: capability
name: Account & Relationship Surveillance
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: medium
---

# Account & Relationship Surveillance

## 1. Definition

Account & Relationship Surveillance is the enduring business ability to continuously monitor account-level behavior, relationship patterns, and entity networks to detect anomalies, emerging risks, and indicators of financial crime that may not surface through individual transaction monitoring alone, including dormant account exploitation, beneficial ownership changes, unusual relationship structures, and account behavior shifts.

## 2. Business Outcome

Ensure the institution can identify account-level and relationship-level risk indicators that transaction-by-transaction monitoring would miss, detect structural and behavioral anomalies that signal emerging financial crime risk, and maintain a comprehensive view of customer relationship risk across the institution's portfolio.

## 3. Scope

### Included activities
- Monitoring account behavior patterns over time including balance trends, product usage shifts, and channel activity changes
- Detecting anomalies in account behavior relative to established customer profiles and peer groups
- Analyzing entity relationships and network structures for risk indicators including shell company patterns, layered ownership, and circular fund flows
- Surveilling dormant account reactivation for potential exploitation
- Monitoring for beneficial ownership changes or undisclosed ownership structures
- Generating alerts when account or relationship behavior deviates from established baselines
- Maintaining account behavior profiles and baselines for surveillance comparison
- Supporting entity resolution to link related accounts and relationships across the institution

### Excluded activities
- Individual transaction monitoring and scenario execution (`cap.transaction_monitoring`)
- Customer risk rating calculation and maintenance (`cap.customer_and_entity_risk_rating`)
- Enhanced due diligence and periodic review execution (`cap.enhanced_due_diligence_and_periodic_review_decisioning`)
- Alert triage and disposition for surveillance-generated alerts (`cap.alert_triage_and_disposition`)
- KYC data collection and verification (`domain.customer`)
- Beneficial ownership data collection at onboarding (`domain.customer`)
- Investigation and case management (investigation capabilities)

## 4. Upstream Dependencies
- Account data and account status from `domain.accounts`
- Customer profile, risk rating, and segmentation data from `domain.customer` and `cap.customer_and_entity_risk_rating`
- Transaction history and aggregated transaction data from `domain.payments` / `domain.transactions`
- Beneficial ownership data from `domain.customer` and external registries
- Product and service usage data across the relationship
- Entity resolution and linkage data
- Surveillance scenario definitions from `cap.financial_crime_risk_appetite_and_policy_management`

## 5. Downstream Dependencies
- `cap.alert_triage_and_disposition` for surveillance alert review and disposition
- `cap.customer_and_entity_risk_rating` for surveillance-informed risk factor updates
- `cap.enhanced_due_diligence_and_periodic_review_decisioning` for surveillance-triggered reviews
- Investigation capabilities for escalated relationship-level concerns
- `cap.risk_restriction_and_offboarding_decisioning` for accounts or relationships warranting restriction
- Regulatory reporting for surveillance-identified suspicious activity

## 6. Related Value Streams
- `vs.financial_crime_detection`
- `vs.trusted_customer_relationship`
- `vs.regulatory_compliance`

## 7. Related Journeys
- `journey.account_behavior_anomaly_detection`
- `journey.relationship_network_analysis`
- `journey.dormant_account_reactivation_surveillance`
- `journey.account_risk_profile_reassessment`
- `journey.beneficial_ownership_change_detection`

## 8. Likely Processes
- Account behavior profiling and baseline establishment
- Account behavior anomaly detection and alert generation
- Relationship network monitoring and analysis
- Dormant account surveillance and reactivation monitoring
- Entity resolution and relationship linkage
- Beneficial ownership change detection
- Account surveillance scenario tuning and calibration
- Surveillance coverage assessment across account types and segments
- Peer group comparison and outlier identification
- Account surveillance performance reporting and metrics

## 9. Risks
- Undetected account behavior anomalies allow financial crime to persist unidentified
- Hidden beneficial ownership changes mask illicit control of accounts
- Dormant account exploitation goes undetected, enabling money laundering or fraud
- Relationship network risks remain unidentified due to incomplete entity resolution
- Account surveillance coverage gaps leave certain account types or segments unmonitored
- Excessive reliance on transaction monitoring without account-level surveillance misses structural risk patterns
- Entity resolution errors create false linkages or miss genuine connections
- Stale account behavior baselines generate excessive false positive alerts

## 10. Controls
- Account behavior baseline calibration and periodic refresh
- Relationship network review frequency governance
- Dormant account monitoring rule validation and coverage
- Entity resolution accuracy validation and quality monitoring
- Account surveillance scenario governance and periodic review
- Surveillance coverage assessment across all account types and segments
- Beneficial ownership data completeness monitoring
- Account surveillance alert quality monitoring and feedback loops
- Independent validation of entity resolution and linkage logic

## 11. Typical Systems/Providers

### Typical systems
- Account surveillance engines
- Entity resolution platforms
- Relationship network analytics tools
- Account behavior profiling systems
- Graph analytics platforms
- Data quality and entity matching tools

### Typical providers
- NICE Actimize
- Quantexa
- Oracle Financial Services
- SAS
- Verafin
- Palantir
- In-house surveillance platforms

## 12. Open Questions
- How should this capability relate to transaction monitoring given that account-level surveillance often uses aggregated transaction data but applies different analytical approaches?
- Should entity resolution be embedded within this capability or modeled as a separate foundational data capability that serves multiple consumers?
- How should beneficial ownership surveillance interact with the KYC refresh process and enhanced due diligence capabilities?
- Where should the boundary sit between account surveillance (ongoing monitoring) and periodic customer review (point-in-time assessment)?
- How should this capability address the growing use of graph analytics and network science techniques that span traditional capability boundaries?
- Is the confidence level appropriate given the relatively nascent state of account-level surveillance as a distinct capability in many institutions?

## 13. Provenance

This capability reflects the growing regulatory expectation that institutions monitor not just individual transactions but also account-level behavior and relationship structures over time. Many institutions have historically embedded account surveillance within transaction monitoring programs, but the distinct analytical approaches (behavioral profiling, network analysis, entity resolution) and different detection horizons (trends over weeks and months rather than individual transaction patterns) warrant separate capability modeling. The medium confidence rating reflects that this capability boundary is less established in industry practice than transaction monitoring or fraud detection, and the scope may need adjustment as the model matures. The inclusion of entity resolution and relationship network analysis acknowledges that account-level risk often manifests through relationship structures rather than individual account behavior.
