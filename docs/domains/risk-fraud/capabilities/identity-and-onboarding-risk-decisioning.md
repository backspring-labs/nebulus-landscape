---
id: cap.identity_and_onboarding_risk_decisioning
type: capability
name: Identity & Onboarding Risk Decisioning
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Identity & Onboarding Risk Decisioning

## 1. Definition

Identity & Onboarding Risk Decisioning is the enduring business ability to render risk-based accept, reject, refer, or conditional-approval decisions for customer onboarding and identity verification events by evaluating identity risk signals, fraud indicators, and compliance factors against policy thresholds.

## 2. Business Outcome

Ensure that only applicants meeting the institution's risk appetite and regulatory requirements are approved for onboarding, while minimizing friction for legitimate applicants and maintaining a defensible, auditable decision trail for every onboarding outcome.

## 3. Scope

### Included activities
- Aggregating identity risk signals from verification, proofing, screening, and fraud detection sources
- Evaluating aggregated signals against policy-driven decision matrices
- Rendering accept, reject, refer-to-manual-review, or conditional-approval decisions
- Managing conditional approvals (stipulations, time-bound conditions, follow-up requirements)
- Supporting manual adjudication workflows for referred applications
- Documenting decision rationale, signals consumed, and policy version applied
- Monitoring decision outcomes for consistency, fairness, and regulatory alignment across channels

### Excluded activities
- Identity proofing and verification execution (`domain.identity_access`)
- Customer due diligence evidence collection (`cap.customer_due_diligence_coordination` in `domain.customer`)
- Sanctions and watchlist screening execution (`cap.sanctions_and_watchlist_screening`)
- Fraud detection model execution (`cap.fraud_detection_and_scoring`)
- Customer risk rating assignment (`cap.customer_and_entity_risk_rating`)
- Account opening and product activation (`domain.accounts`)
- Credit underwriting decisions (`domain.lending`)

## 4. Upstream Dependencies
- Identity verification and proofing results from `domain.identity_access`
- Customer risk rating from `cap.customer_and_entity_risk_rating`
- Screening results from `cap.sanctions_and_watchlist_screening`
- Fraud scores from `cap.fraud_detection_and_scoring`
- Due diligence completion status from `cap.customer_due_diligence_coordination`
- Decision policy thresholds from `cap.financial_crime_risk_appetite_and_policy_management`
- Application data from `cap.customer_application_intake`

## 5. Downstream Dependencies
- `cap.customer_onboarding_orchestration` for onboarding progression or rejection handling
- `cap.customer_identity_establishment` when approval is a precondition for canonical customer creation
- `domain.accounts` for account opening continuation after approval
- `cap.enhanced_due_diligence_and_periodic_review_decisioning` when conditional approval triggers EDD
- Regulatory reporting for decision outcome statistics
- `domain.servicing` when referred decisions require manual case handling

## 6. Related Value Streams
- `vs.customer_onboarding`
- `vs.bsa_aml_readiness`
- `vs.fraud_prevention`
- `vs.trusted_customer_relationship`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_business_deposit_account`
- `journey.digital_identity_verification`
- `journey.onboarding_escalation_review`

## 8. Likely Processes
- Onboarding risk signal aggregation and normalization
- Identity risk decision execution against policy matrix
- Manual adjudication for referred applications
- Conditional approval management and stipulation tracking
- Decision outcome documentation and audit trail generation
- Decision consistency and fairness monitoring
- Channel parity analysis for decision outcomes
- Decision policy version management and rollback

## 9. Risks
- False negative approvals allow high-risk or fraudulent applicants through onboarding
- Excessive false positives create unnecessary friction and applicant abandonment
- Latency in signal aggregation delays decision rendering beyond acceptable onboarding SLAs
- Inconsistent decisions across channels (branch, digital, call center) due to signal availability gaps
- Overrides or manual decisions are undocumented or lack proper authority
- Decision policy is out of sync with current risk appetite or regulatory requirements
- Bias in decision models or rules creates fair-lending or fair-access concerns

## 10. Controls
- Onboarding decision policy matrix with clear accept/reject/refer thresholds
- Multi-signal aggregation validation ensuring all required inputs are present before decisioning
- Decision override governance with approval authority and documentation requirements
- Channel parity monitoring to detect inconsistent outcomes across onboarding channels
- Decision audit trail capturing all signals, policy version, and rationale
- False positive rate monitoring with feedback loops to signal providers
- Decision fairness and disparate impact monitoring
- Policy version control with rollback capability

## 11. Typical Systems/Providers

### Typical systems
- Onboarding decision engines / orchestration platforms
- Identity risk signal aggregation hubs
- Onboarding case management (for referred decisions)
- Decision audit repositories
- Decision analytics and monitoring dashboards
- Conditional approval tracking systems

### Typical providers
- Alloy
- LexisNexis Risk Solutions
- Socure
- NICE Actimize
- Featurespace
- Jumio
- Iovation (TransUnion)
- In-house decision engines and policy platforms

## 12. Open Questions
- Should this capability own the decision engine itself, or should it consume a shared decisioning platform from `domain.orchestration_control`?
- How should conditional approvals be tracked when the conditions span multiple domains (e.g., identity re-verification + additional due diligence)?
- Where should the boundary sit between onboarding risk decisioning and ongoing relationship risk decisioning once the customer is established?
- Should fairness monitoring be embedded in this capability or handled by a cross-cutting model governance capability?

## 13. Provenance

This capability reflects the convergence of identity risk, fraud risk, and compliance risk at the onboarding decision point. It is intentionally separated from the upstream signal-generation capabilities (screening, fraud scoring, identity proofing) because the decision itself requires a distinct governance model, audit trail, and policy framework. Many institutions struggle with this separation, conflating signal generation with decision authority, which creates accountability gaps.
