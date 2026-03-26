---
id: cap.enhanced_due_diligence_and_periodic_review_decisioning
type: capability
name: Enhanced Due Diligence & Periodic Review Decisioning
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Enhanced Due Diligence & Periodic Review Decisioning

## 1. Definition

Enhanced Due Diligence & Periodic Review Decisioning is the enduring business ability to render risk-based decisions on enhanced due diligence requirements, periodic review outcomes, and ongoing customer relationship acceptability by evaluating accumulated risk intelligence, behavioral patterns, and compliance signals against policy thresholds.

## 2. Business Outcome

Ensure that high-risk customer relationships receive appropriately intensified scrutiny on an ongoing basis, that periodic reviews produce defensible retain/restrict/exit decisions, and that the institution can demonstrate to regulators a risk-proportionate, well-governed approach to ongoing customer relationship management.

## 3. Scope

### Included activities
- Evaluating whether enhanced due diligence is required based on risk triggers (risk rating, screening hits, transaction monitoring alerts, adverse events, regulatory guidance)
- Assessing EDD information packages to determine whether additional risk mitigation is needed
- Scheduling and managing periodic review cycles based on customer risk tier
- Rendering periodic review decisions (retain, retain-with-conditions, restrict, escalate-to-exit)
- Decisioning on relationship continuation for customers with elevated or unresolved risk
- Documenting EDD and periodic review decision rationale
- Coordinating with the customer domain for EDD information collection requirements

### Excluded activities
- Collection of EDD evidence and documents (`cap.customer_due_diligence_coordination` in `domain.customer`)
- Customer risk rating assignment or refresh (`cap.customer_and_entity_risk_rating`)
- Sanctions and watchlist screening execution (`cap.sanctions_and_watchlist_screening`)
- Transaction monitoring and suspicious activity detection (`domain.risk_fraud` AML monitoring capabilities)
- SAR filing and regulatory reporting (`domain.risk_fraud` reporting capabilities)
- Customer exit execution and account closure (`cap.customer_exit_management` in `domain.customer`, `cap.account_closure_management` in `domain.accounts`)
- Onboarding-stage risk decisions (`cap.identity_and_onboarding_risk_decisioning`)

## 4. Upstream Dependencies
- Customer risk rating from `cap.customer_and_entity_risk_rating`
- Screening results and alert history from `cap.sanctions_and_watchlist_screening`
- EDD evidence and documentation from `cap.customer_due_diligence_coordination`
- Transaction monitoring alert history from AML monitoring capabilities
- SAR filing history from regulatory reporting capabilities
- Decision policy thresholds from `cap.financial_crime_risk_appetite_and_policy_management`
- Customer profile and relationship data from `domain.customer`

## 5. Downstream Dependencies
- `cap.customer_and_entity_risk_rating` for risk rating refresh after review decisions
- `domain.customer` for relationship status updates and exit initiation
- `domain.accounts` for account restriction or closure instructions
- Transaction monitoring capabilities for monitoring intensity adjustments
- Regulatory reporting for EDD and periodic review statistics
- `domain.servicing` for case management of review actions

## 6. Related Value Streams
- `vs.bsa_aml_readiness`
- `vs.compliance_enablement`
- `vs.trusted_customer_relationship`
- `vs.enterprise_risk_governance`

## 7. Related Journeys
- `journey.escalated_due_diligence_review`
- `journey.periodic_customer_risk_review`
- `journey.high_risk_relationship_continuation`
- `journey.edd_triggered_by_adverse_event`

## 8. Likely Processes
- EDD trigger evaluation and requirement determination
- EDD information package assessment and decisioning
- Periodic review scheduling and queue management
- Periodic review decision execution (retain/restrict/exit recommendation)
- Relationship continuation decisioning for escalated cases
- EDD and periodic review decision documentation
- Review backlog monitoring and SLA management
- Decision quality assurance and second-level review

## 9. Risks
- Missed EDD triggers allow high-risk relationships to continue without appropriate scrutiny
- Overdue periodic reviews create regulatory exposure and examination findings
- Insufficient depth in EDD assessment fails to uncover material risks
- Relationships are retained despite unacceptable risk levels due to revenue pressure or insufficient governance
- Inconsistent review standards across reviewers, business lines, or customer segments
- EDD and review backlogs accumulate beyond manageable levels
- Decision documentation is insufficient to support regulatory examination

## 10. Controls
- EDD trigger completeness monitoring ensuring all qualifying relationships enter EDD
- Periodic review schedule adherence monitoring with escalation for overdue reviews
- EDD decision quality review (sampling, second-level review, calibration sessions)
- Relationship exit escalation path with clear authority and documentation requirements
- Review documentation completeness checks before decision finalization
- EDD policy alignment validation ensuring decisions reflect current policy and risk appetite
- Segregation of duties between review preparation and decision authority
- Backlog monitoring with capacity planning and escalation triggers

## 11. Typical Systems/Providers

### Typical systems
- EDD case management platforms
- Periodic review workflow engines
- Risk intelligence aggregation services (pulling screening, monitoring, rating data)
- EDD decision repositories and audit trails
- Review scheduling and queue management tools
- Relationship risk dashboards

### Typical providers
- Fenergo
- NICE Actimize
- Moody's / Bureau van Dijk
- Refinitiv (LSEG)
- LexisNexis Risk Solutions
- Dow Jones Risk & Compliance
- Quantexa
- In-house EDD and periodic review platforms

## 12. Open Questions
- Should EDD decisioning and periodic review decisioning be modeled as separate capabilities given their different triggering patterns and operational rhythms?
- How should relationship exit decisions be governed when they span multiple domains (risk decision vs. customer exit execution vs. account closure)?
- Where should the boundary sit between periodic review decisioning and ongoing transaction monitoring when monitoring alerts trigger ad-hoc reviews?
- Should decision quality assurance (calibration, second-level review) be embedded here or in a cross-cutting quality management capability?

## 13. Provenance

This capability reflects BSA/AML regulatory expectations for enhanced due diligence on high-risk customers and ongoing/periodic review of customer relationships. It is intentionally separated from evidence collection (which sits in the customer domain) and from upstream signal generation (risk rating, screening, monitoring) because the decision itself requires distinct governance, authority, and documentation standards. The combination of EDD and periodic review in a single capability reflects their shared decision framework and governance model, though they may be separated as the model matures.
