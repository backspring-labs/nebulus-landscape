---
id: cap.risk_restriction_and_offboarding_decisioning
type: capability
name: Risk Restriction & Offboarding Decisioning
domain_id: domain.risk_fraud
status: draft
source_refs: []
last_reviewed: 2026-03-26
confidence: high
---

# Risk Restriction & Offboarding Decisioning

## 1. Definition

Risk Restriction & Offboarding Decisioning is the enduring business ability to evaluate and decision account restrictions, relationship limitations, and customer offboarding (de-risking or exit) actions based on financial crime risk findings, fraud risk indicators, policy violations, and regulatory requirements, while managing the legal, reputational, and customer impact considerations inherent in these consequential decisions.

## 2. Business Outcome

Ensure the institution can take timely and appropriate action to restrict or exit customer relationships that present unacceptable financial crime or fraud risk, comply with regulatory and law enforcement requirements for account action, maintain consistency and defensibility in restriction and offboarding decisions, and manage the legal and reputational risks associated with these actions including fair banking and anti-discrimination considerations.

## 3. Scope

### Included activities
- Evaluating whether investigation findings, risk rating changes, or regulatory triggers warrant account restriction or customer offboarding
- Decisioning the type and scope of restrictions (transaction limits, product restrictions, channel limitations, full account freeze)
- Decisioning customer offboarding (voluntary exit, involuntary exit, regulatory-driven exit)
- Coordinating legal review for high-risk or sensitive offboarding decisions
- Managing restriction and offboarding appeal or reconsideration processes
- Documenting restriction and offboarding rationale and supporting evidence
- Coordinating with law enforcement and regulatory bodies when restrictions are directed or required
- Monitoring restriction and offboarding decisions for consistency, fairness, and pattern analysis
- Managing timing considerations including SAR filing coordination, law enforcement holds, and customer notification sequencing

### Excluded activities
- Account closure execution and operational processing (`cap.account_closure_management` / `domain.accounts`)
- Customer notification and communication about restrictions or exits (`domain.servicing` / `domain.communications`)
- Investigation and case management that precedes restriction decisions (investigation capabilities)
- Customer risk rating calculation (`cap.customer_and_entity_risk_rating`)
- Sanctions-driven blocking and rejection (`cap.sanctions_and_watchlist_screening`)
- Fraud-driven transaction blocking at point of sale (`cap.payment_and_transfer_risk_decisioning`)
- Account status management and operational holds (`cap.account_status_and_restriction_management`)

## 4. Upstream Dependencies
- Investigation findings and recommendations from investigation capabilities within `domain.risk_fraud`
- Customer risk rating and risk factor data from `cap.customer_and_entity_risk_rating`
- Alert and case disposition data from `cap.alert_triage_and_disposition` and investigation capabilities
- SAR/STR filing status and law enforcement request data from regulatory reporting capabilities
- Account and relationship data from `domain.accounts` and `domain.customer`
- Restriction and offboarding policy parameters from `cap.financial_crime_risk_appetite_and_policy_management`
- Legal and compliance guidance for sensitive decisions
- Regulatory directives and law enforcement instructions when applicable

## 5. Downstream Dependencies
- `cap.account_closure_management` / `domain.accounts` for restriction implementation and account closure execution
- `cap.account_status_and_restriction_management` for operational restriction application
- Customer communication capabilities for restriction or offboarding notification
- Regulatory reporting for offboarding statistics and de-risking metrics
- Legal and compliance for appeal review and litigation support
- Management and board reporting for restriction and offboarding program metrics
- `domain.payments` for payment restriction implementation

## 6. Related Value Streams
- `vs.financial_crime_detection`
- `vs.regulatory_compliance`
- `vs.trusted_customer_relationship`
- `vs.risk_mitigation`

## 7. Related Journeys
- `journey.account_restriction_decision`
- `journey.customer_offboarding_risk_review`
- `journey.risk_based_service_limitation`
- `journey.restriction_appeal_review`
- `journey.regulatory_driven_account_closure`

## 8. Likely Processes
- Restriction risk assessment and recommendation review
- Offboarding decisioning and approval workflow
- Restriction scope and type determination
- Legal review coordination for sensitive decisions
- Restriction and offboarding documentation and evidence assembly
- Restriction appeal or reconsideration evaluation
- Regulatory and law enforcement coordination for directed actions
- Restriction and offboarding consistency review
- De-risking pattern analysis and fair banking assessment
- Restriction timing coordination with SAR filing and law enforcement
- Offboarding notification sequencing and coordination

## 9. Risks
- Premature account restriction disrupts legitimate customer activity and creates reputational harm
- Delayed restriction on high-risk accounts allows continued financial crime or fraud
- Offboarding without regulatory compliance (e.g., before SAR filing, during law enforcement investigation) creates legal exposure
- Inconsistent restriction decisions create fair banking, discrimination, or disparate impact risk
- Legal exposure from offboarding actions that appear discriminatory or lack documented basis
- Restriction or offboarding actions that inadvertently alert subjects of investigation (tipping off)
- Inadequate documentation of restriction rationale creates regulatory and litigation exposure
- Over-reliance on de-risking as default response rather than risk-managed relationship continuation

## 10. Controls
- Restriction decision governance with defined approval authorities and escalation thresholds
- Offboarding regulatory compliance checklist (SAR status, law enforcement coordination, notification timing)
- Restriction decision consistency review and calibration across analysts and teams
- Restriction appeal process timeliness and fairness standards
- Offboarding documentation completeness review
- Legal review requirement for high-risk or sensitive offboarding decisions
- De-risking pattern analysis for fair banking and disparate impact monitoring
- Tipping-off risk assessment for restriction timing
- Segregation of duties between restriction recommendation and approval

## 11. Typical Systems/Providers

### Typical systems
- Restriction decisioning and workflow platforms
- Offboarding management systems
- Account restriction tracking systems
- Regulatory closure and coordination tools
- Case management systems (for restriction-related case tracking)
- Decision documentation and evidence repositories

### Typical providers
- NICE Actimize
- Oracle Financial Services
- Fiserv
- In-house risk and compliance platforms
- Hummingbird
- Regulatory technology providers

## 12. Open Questions
- How should this capability relate to `cap.account_status_and_restriction_management` in `domain.accounts` which handles the operational implementation of restrictions?
- Should restriction decisioning and offboarding decisioning be modeled as separate capabilities given their different risk profiles, approval requirements, and regulatory considerations?
- How should this capability address the regulatory tension between de-risking (which regulators discourage) and the institution's obligation to manage financial crime risk?
- Where should the boundary sit between investigation recommendation (suggesting restriction) and restriction decisioning (approving and directing restriction)?
- How should law enforcement-directed account actions (e.g., seizure warrants, keep-open requests) be handled within this capability?
- How should this capability address the timing complexities around SAR filing, customer notification, and account action sequencing?

## 13. Provenance

This capability reflects the consequential end-of-lifecycle decisions that institutions must make when financial crime or fraud risk exceeds acceptable thresholds. It is modeled separately from investigation because the decisioning function involves distinct governance, legal review, and fairness considerations that differ from investigative analysis. The capability spans both restriction (limiting activity) and offboarding (exiting the relationship) because these decisions share common governance frameworks, legal considerations, and regulatory requirements, though they differ in severity and reversibility. The emphasis on legal, reputational, and fairness considerations reflects growing regulatory scrutiny of de-risking practices and the recognition that restriction and offboarding decisions carry significant consequences for both institutions and customers.
