---
id: cap.customer_and_entity_risk_rating
type: capability
name: Customer & Entity Risk Rating
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Customer & Entity Risk Rating

## 1. Definition

Customer & Entity Risk Rating is the enduring business ability to assess, assign, maintain, and refresh financial crime risk ratings for customers and entities based on inherent and residual risk factors, enabling risk-proportionate treatment across onboarding, transaction monitoring, periodic review, and relationship management.

## 2. Business Outcome

Ensure that every customer and entity relationship carries an accurate, current, and defensible financial crime risk rating so that downstream capabilities can apply risk-proportionate controls, monitoring intensity, and review frequency aligned with the institution's risk appetite and regulatory expectations.

## 3. Scope

### Included activities
- Evaluating customer and entity risk factors (geography, product usage, entity type, industry, transaction behavior, PEP/sanctions exposure, source of funds/wealth)
- Executing risk rating models and scorecards
- Assigning initial risk ratings at onboarding
- Managing risk rating overrides with appropriate authority, justification, and documentation
- Refreshing risk ratings on a periodic schedule or triggered by material events
- Monitoring risk rating distribution for model health and concentration risk
- Validating and back-testing risk rating model performance

### Excluded activities
- Collection of due diligence evidence and documents (`cap.customer_due_diligence_coordination` in `domain.customer`)
- Identity verification and proofing execution (`domain.identity_access`)
- Sanctions and watchlist screening (`cap.sanctions_and_watchlist_screening`)
- Onboarding accept/reject decisions (`cap.identity_and_onboarding_risk_decisioning`)
- Transaction monitoring and suspicious activity detection (`domain.risk_fraud` operational capabilities)
- Credit risk scoring and underwriting (`domain.lending` / `domain.credit`)

## 4. Upstream Dependencies
- Customer and entity profile data from `domain.customer`
- Due diligence evidence and documentation from `cap.customer_due_diligence_coordination`
- Identity verification results from `domain.identity_access`
- Screening results from `cap.sanctions_and_watchlist_screening`
- Risk appetite and policy thresholds from `cap.financial_crime_risk_appetite_and_policy_management`
- Transaction and behavioral data from `domain.transactions` / `domain.payments`
- External risk intelligence (adverse media, PEP databases, country risk indices)

## 5. Downstream Dependencies
- `cap.identity_and_onboarding_risk_decisioning` for onboarding risk-based decisions
- `cap.enhanced_due_diligence_and_periodic_review_decisioning` for EDD trigger evaluation and review scheduling
- `cap.sanctions_and_watchlist_screening` for screening sensitivity calibration by risk tier
- Transaction monitoring capabilities for monitoring scenario intensity
- `domain.customer` for risk-tier-driven relationship management
- Regulatory reporting for risk-rated population statistics

## 6. Related Value Streams
- `vs.bsa_aml_readiness`
- `vs.customer_onboarding`
- `vs.compliance_enablement`
- `vs.trusted_customer_relationship`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_business_deposit_account`
- `journey.periodic_customer_risk_review`
- `journey.risk_rating_override_review`

## 8. Likely Processes
- Customer risk factor assessment and data aggregation
- Risk rating model/scorecard execution
- Initial risk rating assignment at onboarding
- Risk rating override request and adjudication
- Periodic risk rating refresh (scheduled and event-triggered)
- Risk rating model validation and back-testing
- Rating distribution monitoring and concentration analysis
- Risk factor weighting recalibration

## 9. Risks
- Customer risk rating understates actual risk, leading to insufficient monitoring
- Stale risk ratings persist beyond policy-mandated refresh periods
- Model drift reduces rating accuracy over time
- Risk rating overrides are applied without adequate justification or governance
- Incomplete risk factor data produces unreliable ratings
- Inconsistent rating methodologies across customer segments or business lines
- Over-reliance on a single risk factor (e.g., geography) without holistic assessment

## 10. Controls
- Risk rating model validation and periodic back-testing
- Override approval authority matrix with escalation requirements
- Periodic rating refresh schedule adherence monitoring
- Risk factor completeness checks before rating execution
- Rating distribution monitoring for anomalous shifts or concentrations
- Model performance metrics and drift detection
- Segregation of duties between model development, validation, and override authority
- Audit trail for all rating assignments, overrides, and changes

## 11. Typical Systems/Providers

### Typical systems
- Customer risk rating engines and scorecards
- Risk factor aggregation services
- Risk rating case management (for overrides and exceptions)
- Model governance and validation platforms
- Risk rating dashboards and reporting tools
- Rating distribution analytics

### Typical providers
- NICE Actimize
- Fenergo
- SAS
- Oracle Financial Crime and Compliance Management
- LexisNexis Risk Solutions
- Moody's / Bureau van Dijk
- Quantexa
- In-house risk rating engines and scorecards

## 12. Open Questions
- Should the risk rating model itself be owned by this capability, or should model development sit in a separate model risk management capability with this capability owning execution?
- How should entity risk rating for complex corporate structures (nested beneficial ownership) differ from retail customer rating?
- Should risk rating refresh be purely periodic, or should event-driven triggers (e.g., screening hit, suspicious activity) dominate?
- Where should the boundary sit between financial crime risk rating and credit risk rating when both consume overlapping data?

## 13. Provenance

This capability reflects BSA/AML regulatory expectations for risk-based customer due diligence programs. Risk rating is a foundational input to nearly every other financial crime capability, determining monitoring intensity, review frequency, and EDD requirements. It is modeled as a standalone capability because its outputs are consumed broadly and its governance (model validation, override controls, refresh scheduling) warrants dedicated ownership.
