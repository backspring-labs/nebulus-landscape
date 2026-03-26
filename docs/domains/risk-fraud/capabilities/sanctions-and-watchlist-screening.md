---
id: cap.sanctions_and_watchlist_screening
type: capability
name: Sanctions & Watchlist Screening
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Sanctions & Watchlist Screening

## 1. Definition

Sanctions & Watchlist Screening is the enduring business ability to screen customers, counterparties, transactions, and related entities against sanctions lists, PEP databases, adverse media sources, and regulatory watchlists to identify prohibited or high-risk associations and enable timely interdiction or escalation.

## 2. Business Outcome

Ensure the institution does not establish or maintain prohibited relationships, process prohibited transactions, or fail to identify high-risk associations, thereby meeting OFAC, FinCEN, and international sanctions compliance obligations while minimizing false-positive operational burden and customer friction.

## 3. Scope

### Included activities
- Screening customer and entity names against OFAC SDN, consolidated sanctions lists, and other government watchlists
- Screening counterparties and beneficiaries in payment and transaction flows
- Screening against PEP (Politically Exposed Person) databases
- Screening against adverse media sources where policy requires
- Managing fuzzy matching algorithms and threshold calibration
- Ingesting and applying list updates (additions, removals, modifications) in a timely manner
- Generating, triaging, and dispositioning screening alerts
- Escalating true matches for interdiction, blocking, or regulatory reporting
- Conducting periodic batch rescreening of the existing customer base
- Maintaining screening audit trails

### Excluded activities
- Transaction monitoring for suspicious activity patterns (`domain.risk_fraud` AML monitoring capabilities)
- Customer risk rating assignment (`cap.customer_and_entity_risk_rating`)
- Onboarding accept/reject decisions (`cap.identity_and_onboarding_risk_decisioning`)
- SAR/STR filing and regulatory reporting (`domain.risk_fraud` reporting capabilities)
- OFAC license application or blocked-property reporting (compliance operations)
- Due diligence evidence collection (`cap.customer_due_diligence_coordination` in `domain.customer`)

## 4. Upstream Dependencies
- Customer and entity data from `domain.customer`
- Transaction and payment data from `domain.payments` / `domain.transactions`
- Screening policy and threshold parameters from `cap.financial_crime_risk_appetite_and_policy_management`
- Sanctions lists, PEP databases, and adverse media feeds from external providers
- Counterparty and beneficiary data from originating systems

## 5. Downstream Dependencies
- `cap.identity_and_onboarding_risk_decisioning` for onboarding screening signal consumption
- `cap.customer_and_entity_risk_rating` for screening-informed risk factor input
- `cap.enhanced_due_diligence_and_periodic_review_decisioning` for EDD triggers from screening hits
- Payment interdiction and blocking systems for real-time transaction holds
- Regulatory reporting for OFAC blocking reports and sanctions-related filings
- `domain.customer` for relationship-level screening status

## 6. Related Value Streams
- `vs.bsa_aml_readiness`
- `vs.compliance_enablement`
- `vs.customer_onboarding`
- `vs.payment_integrity`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_business_deposit_account`
- `journey.wire_transfer_initiation`
- `journey.sanctions_alert_investigation`
- `journey.periodic_batch_rescreening`

## 8. Likely Processes
- Name and entity screening at onboarding
- Transaction and payment screening (real-time and batch)
- Screening alert generation and triage
- Alert disposition (false positive, true match, escalation)
- True match escalation and interdiction
- List update ingestion and validation
- Periodic batch rescreening of existing customer base
- Fuzzy matching threshold calibration and tuning
- Screening coverage gap analysis

## 9. Risks
- Missed sanctions match due to poor data quality, inadequate fuzzy matching, or screening coverage gaps
- Excessive false positive alerts overwhelm operations and delay legitimate transactions
- Stale watchlist data due to delayed list update ingestion
- Screening coverage gaps where certain customer types, transaction types, or channels are not screened
- Delayed list update ingestion creates a window of non-compliance
- Inconsistent screening across onboarding vs. transaction vs. periodic rescreening
- Over-reliance on name screening without adequate contextual matching (dates of birth, addresses, identifiers)

## 10. Controls
- Screening coverage completeness validation across all required touchpoints
- List update timeliness monitoring with SLA tracking
- Fuzzy matching threshold calibration and periodic tuning reviews
- Alert disposition quality review (sampling, second-level review)
- True match escalation SLA monitoring
- Screening audit trail for all screening events, alerts, and dispositions
- Screening gap analysis and remediation tracking
- Independent testing of screening effectiveness (name-based, transaction-based)
- Dual-control for alert disposition on high-risk matches

## 11. Typical Systems/Providers

### Typical systems
- Sanctions screening engines (real-time and batch)
- Watchlist management and list ingestion platforms
- Screening alert case management
- Transaction screening gateways
- PEP and adverse media screening modules
- Screening analytics and reporting dashboards

### Typical providers
- NICE Actimize
- Refinitiv (LSEG) World-Check
- Fircosoft (LMG)
- LexisNexis Risk Solutions
- Dow Jones Risk & Compliance
- Oracle Financial Crime and Compliance Management
- Napier AI
- In-house screening engines

## 12. Open Questions
- Should transaction screening and name/entity screening be modeled as separate sub-capabilities given their different operational patterns and SLA requirements?
- How should adverse media screening be governed when it overlaps with both sanctions screening and broader EDD intelligence gathering?
- Where should the boundary sit between screening execution and screening alert investigation if investigation becomes its own capability?
- Should PEP screening be embedded here or split into a dedicated capability given its distinct data sources and policy considerations?

## 13. Provenance

This capability reflects OFAC compliance requirements, BSA/AML regulatory expectations, and international sanctions obligations. Screening is one of the most operationally intensive financial crime capabilities due to the volume of events processed and the false-positive management burden. It is modeled as a standalone capability because its real-time and batch processing patterns, list management lifecycle, and alert disposition workflows are distinct from other risk and fraud capabilities.
