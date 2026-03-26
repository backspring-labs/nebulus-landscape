---
id: cap.financial_crime_risk_appetite_and_policy_management
type: capability
name: Financial Crime Risk Appetite & Policy Management
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Financial Crime Risk Appetite & Policy Management

## 1. Definition

Financial Crime Risk Appetite & Policy Management is the enduring business ability to define, maintain, calibrate, and operationalize the institution's financial crime risk appetite statements, tolerance thresholds, and supporting policies that govern how AML, fraud, sanctions, and related financial crime risks are identified, assessed, mitigated, and reported across the enterprise.

## 2. Business Outcome

Ensure the institution maintains a clearly articulated, board-approved financial crime risk appetite that is translated into actionable policies, tolerance thresholds, and control expectations so that all downstream risk and compliance capabilities operate within a coherent, defensible framework aligned with regulatory expectations and strategic objectives.

## 3. Scope

### Included activities
- Defining and maintaining board-level financial crime risk appetite statements
- Translating risk appetite into measurable tolerance thresholds and key risk indicators
- Authoring, versioning, and governing financial crime policies (AML, fraud, sanctions, ABC)
- Managing policy exceptions, waivers, and temporary deviations with appropriate authority and documentation
- Mapping policies to regulatory requirements and supervisory expectations
- Coordinating policy attestation and certification cycles across business lines
- Calibrating risk tolerance thresholds based on risk assessment outcomes, regulatory feedback, and emerging threats
- Tracking regulatory change and its impact on existing policies and appetite statements

### Excluded activities
- Execution of customer risk rating or screening (`cap.customer_and_entity_risk_rating`, `cap.sanctions_and_watchlist_screening`)
- Onboarding or transaction-level decisioning (`cap.identity_and_onboarding_risk_decisioning`, `cap.fraud_detection_and_scoring`)
- Operational fraud investigation or SAR filing (`domain.risk_fraud` operational capabilities)
- Enterprise-wide operational risk appetite outside financial crime (`domain.enterprise_risk`)
- Regulatory examination management and response (`domain.compliance`)

## 4. Upstream Dependencies
- Board and senior management risk governance directives
- Enterprise risk appetite framework and risk taxonomy
- Regulatory requirements, supervisory guidance, and examination findings
- BSA/AML risk assessment outputs
- Industry threat intelligence and typology updates
- Internal audit findings related to financial crime controls

## 5. Downstream Dependencies
- `cap.customer_and_entity_risk_rating` for risk factor weighting and threshold alignment
- `cap.sanctions_and_watchlist_screening` for screening coverage and matching sensitivity policies
- `cap.identity_and_onboarding_risk_decisioning` for onboarding decision policy thresholds
- `cap.enhanced_due_diligence_and_periodic_review_decisioning` for EDD trigger criteria and review frequency policies
- `cap.fraud_detection_and_scoring` for fraud rule and model threshold governance
- `domain.compliance` for regulatory reporting and examination readiness

## 6. Related Value Streams
- `vs.bsa_aml_readiness`
- `vs.compliance_enablement`
- `vs.trusted_customer_relationship`
- `vs.enterprise_risk_governance`

## 7. Related Journeys
- `journey.annual_bsa_aml_risk_assessment`
- `journey.policy_update_and_approval`
- `journey.regulatory_exam_preparation`
- `journey.new_product_risk_assessment`

## 8. Likely Processes
- Risk appetite statement definition and annual review
- Financial crime policy authoring, review, and approval
- Policy exception request and adjudication
- Risk tolerance threshold calibration and recalibration
- Policy attestation and certification tracking
- Regulatory change impact assessment on policies
- Policy-to-regulation mapping maintenance
- Board and committee reporting on risk appetite adherence

## 9. Risks
- Risk appetite statements are too abstract to drive operational control calibration
- Policies become stale and fail to reflect current regulatory expectations or threat landscape
- Policy exceptions accumulate without adequate tracking or expiration
- Inconsistent risk tolerance thresholds across business lines or geographies
- Gap between stated risk appetite and actual control effectiveness
- Regulatory expectations evolve faster than policy update cycles
- Lack of clear ownership for policy maintenance across the three lines of defense

## 10. Controls
- Annual (or event-driven) risk appetite review and board attestation
- Policy version governance with approval workflows and audit trails
- Exception approval authority matrix with escalation requirements
- Tolerance threshold monitoring against actual performance metrics
- Policy-to-regulatory-requirement mapping with gap tracking
- Board and committee attestation tracking and documentation
- Regulatory change monitoring and policy impact assessment triggers
- Independent review of policy effectiveness by compliance or internal audit

## 11. Typical Systems/Providers

### Typical systems
- Financial crime policy repositories and document management
- Risk appetite dashboards and KRI monitoring tools
- Policy workflow engines with approval routing
- Regulatory change tracking platforms
- GRC (governance, risk, compliance) platforms
- Board reporting and attestation tools

### Typical providers
- NICE Actimize
- SAS
- Oracle Financial Crime and Compliance Management
- Moody's
- Wolters Kluwer
- MetricStream
- ServiceNow GRC
- In-house policy management and risk governance platforms

## 12. Open Questions
- Should this capability own the BSA/AML risk assessment process itself, or should that be a separate capability with this one consuming its outputs?
- How should policy management for financial crime relate to broader enterprise policy management if a horizontal GRC capability is later modeled?
- Where should the boundary sit between risk appetite calibration and model risk management for financial crime models?
- Should regulatory change management be modeled as its own cross-cutting capability rather than embedded here?

## 13. Provenance

This capability reflects the foundational governance layer required by BSA/AML regulations, FFIEC examination expectations, and FinCEN guidance. It is intentionally separated from operational execution capabilities because risk appetite and policy set the parameters within which all other financial crime capabilities operate. The separation ensures that governance and calibration concerns are not conflated with detection, screening, or decisioning execution.
