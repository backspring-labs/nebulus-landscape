---
id: cap.case_investigation_management
type: capability
name: Case Investigation Management
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Case Investigation Management

## 1. Definition

Case Investigation Management is the enduring business ability to manage the end-to-end lifecycle of financial crime and fraud investigations from case creation through resolution, including evidence gathering, investigative analysis, link analysis, documentation of findings, and determination of outcomes to support regulatory reporting decisions, remediation actions, and program feedback, while maintaining quality, consistency, and regulatory-defensible documentation standards.

## 2. Business Outcome

Ensure the institution can thoroughly investigate escalated alerts and identified suspicious activity, reach well-supported and consistent case determinations, document findings to regulatory-defensible standards, support timely SAR/STR filing decisions, identify patterns and networks of illicit activity, and manage investigator capacity and case aging within acceptable service levels to fulfill the institution's regulatory obligations and protect against financial crime.

## 3. Scope

### Included activities
- Creating and initializing cases from escalated alerts, referrals, and self-identified suspicious activity
- Assigning cases to investigators based on expertise, capacity, and case complexity
- Gathering and assembling evidence from internal systems, customer records, transaction data, and external sources
- Performing investigative analysis including link analysis, timeline reconstruction, and pattern identification
- Documenting investigative steps, findings, and supporting evidence throughout the case lifecycle
- Making case determination decisions (substantiated, unsubstantiated, inconclusive) with documented rationale
- Escalating cases for supervisory review, peer review, or subject matter expert consultation
- Closing cases with complete documentation and archival of evidence
- Performing quality assurance review of case investigations for consistency and completeness
- Managing case aging, workload distribution, and investigator capacity

### Excluded activities
- Alert generation and detection logic (`cap.transaction_monitoring`, `cap.fraud_detection_and_scoring`, `cap.sanctions_and_watchlist_screening`)
- Alert triage and initial disposition (`cap.alert_triage_and_disposition`)
- SAR/STR preparation, filing, and submission (suspicious activity reporting capabilities)
- Customer notification, contact, or remediation actions (`domain.servicing` / `domain.communications`)
- Account restriction or closure decisions (`cap.risk_restriction_and_offboarding_decisioning`)
- Law enforcement referral and coordination (regulatory information sharing capabilities)
- Detection model tuning based on investigation outcomes (`cap.fraud_rules_and_model_governance`)

## 4. Upstream Dependencies
- Escalated alerts and preliminary analysis from `cap.alert_triage_and_disposition`
- Customer profile, risk rating, and KYC data from `domain.customer` and `cap.customer_and_entity_risk_rating`
- Transaction data and history from `domain.payments` / `domain.transactions`
- Account data and relationship information from `domain.accounts`
- Sanctions and watchlist screening results from `cap.sanctions_and_watchlist_screening`
- Prior investigation history and related case data from internal case records
- External data sources for investigative research (public records, adverse media, beneficial ownership registries)

## 5. Downstream Dependencies
- Suspicious activity reporting capabilities for SAR/STR filing decisions based on case determinations
- `cap.risk_restriction_and_offboarding_decisioning` for cases warranting account restriction or exit
- `cap.customer_and_entity_risk_rating` for investigation-outcome-informed risk rating updates
- Regulatory information sharing and law enforcement coordination for cases requiring referral
- `cap.fraud_rules_and_model_governance` and `cap.transaction_monitoring` for investigation feedback to improve detection
- `cap.enhanced_due_diligence_and_periodic_review_decisioning` for cases identifying EDD triggers
- Management and board reporting for investigation metrics and program effectiveness

## 6. Related Value Streams
- `vs.financial_crime_detection`
- `vs.fraud_prevention`
- `vs.regulatory_compliance`
- `vs.operational_efficiency`

## 7. Related Journeys
- `journey.case_creation_and_assignment`
- `journey.evidence_gathering_and_analysis`
- `journey.investigator_review_and_determination`
- `journey.case_escalation_and_peer_review`
- `journey.case_closure_and_documentation`

## 8. Likely Processes
- Case creation, intake, and initialization from escalated alerts and referrals
- Case assignment and workload balancing across investigator teams
- Evidence collection and assembly from internal and external sources
- Investigative analysis including link analysis, timeline reconstruction, and network mapping
- Case determination decision-making and findings documentation
- Case escalation and supervisory or peer review
- Case closure, documentation finalization, and evidence archival
- Case quality assurance review and sampling
- Case aging and SLA monitoring and management
- Investigator performance and capacity reporting
- Investigation feedback loop to detection and triage capabilities

## 9. Risks
- Incomplete investigations missing material evidence result in incorrect determinations and regulatory exposure
- Investigation timelines exceed regulatory deadlines, creating compliance violations for SAR filing
- Inconsistent case determination standards across investigators create audit and regulatory risk
- Insufficient case documentation fails to demonstrate regulatory-defensible investigative rationale
- Investigator capacity bottlenecks cause case backlogs that degrade program timeliness and effectiveness
- Failure to identify linked cases or networks of activity results in fragmented understanding of illicit schemes
- Loss of investigation feedback to detection systems prevents scenario and model improvement
- Inadequate evidence preservation creates challenges for potential law enforcement referrals

## 10. Controls
- Case aging and SLA monitoring with escalation thresholds for approaching deadlines
- Case determination consistency review through periodic calibration exercises
- Case documentation completeness checks for all investigation closures
- Case quality assurance sampling with documented review findings and feedback
- Case escalation timeliness tracking for supervisory and peer review
- Investigator workload monitoring and capacity management
- Evidence chain of custody and preservation controls
- Segregation of duties between investigation and quality assurance review
- Case determination authority levels based on case complexity and risk severity

## 11. Typical Systems/Providers

### Typical systems
- Case management platforms
- Evidence repository and document management systems
- Link analysis and network visualization tools
- Investigation workflow engines
- Case analytics and operational dashboards
- Secure communication and collaboration tools for investigative teams

### Typical providers
- NICE Actimize
- Verafin
- Oracle Financial Services
- SAS
- Hummingbird
- WorkFusion
- Palantir (link analysis)
- In-house case management platforms

## 12. Open Questions
- Should case investigation be modeled as a single capability spanning all financial crime types (AML, fraud, sanctions) or separated by investigation domain given different regulatory requirements, skills, and determination standards?
- How should AI-assisted investigation tools (automated narrative generation, intelligent evidence assembly) be governed within this capability?
- Where should the boundary sit between investigation (analysis and determination) and regulatory reporting (SAR preparation and filing) given their tight operational coupling?
- How should multi-subject and multi-institution investigations be coordinated within this capability model?
- Should investigation quality assurance be embedded within this capability or modeled as part of a broader program governance capability?

## 13. Provenance

This capability reflects the core investigative function in the financial crime compliance lifecycle that sits between alert triage and regulatory reporting. Case investigation is modeled as a distinct capability because it has unique operational characteristics -- analytical depth, judgment-intensive, evidence-driven, and subject to specific regulatory standards for documentation and timeliness -- that differ from both upstream triage (high-volume, workflow-driven) and downstream reporting (form-driven, regulatory-format-specific). The capability spans investigation types across financial crime domains because the investigative methodology, evidence gathering patterns, case management workflows, and quality assurance processes are fundamentally similar regardless of the originating alert type. Effective case investigation is the linchpin that converts detection signals into regulatory-defensible determinations and actionable intelligence.
