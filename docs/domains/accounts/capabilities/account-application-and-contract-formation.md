---
id: cap.account_application_and_contract_formation
type: capability
name: Account Application and Contract Formation
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Application and Contract Formation

## 1. Definition

Account Application and Contract Formation is the enduring business ability to manage the account-specific application, disclosure presentment, agreement finalization, and contractual acceptance required to create the product relationship. It covers the journey from product selection through legally binding acceptance, ensuring the customer receives required disclosures, understands terms, and provides auditable evidence of agreement before the account is opened.

## 2. Business Outcome

Ensure every deposit account is opened with a complete, version-controlled, and evidenced contractual foundation so the institution can demonstrate regulatory compliance, enforce agreed terms, and defend the validity of the account relationship.

## 3. Scope

### Included activities
- Managing the account-specific application form and data collection
- Presenting required product disclosures (Truth in Savings, fee schedules, account agreements)
- Capturing customer acceptance of terms, disclosures, and agreements
- Managing e-signature or wet-signature capture and evidence retention
- Versioning account agreements and maintaining contract-to-product traceability
- Handling multi-party acceptance for joint accounts
- Managing disclosure re-presentment when product terms change during application
- Routing incomplete or exception applications for remediation
- Producing contract formation evidence for audit and compliance

### Excluded activities
- Customer-level application intake, identity, and onboarding (`domain.customer`)
- Product definition and catalog management (`cap.deposit_account_product_management`)
- Account record creation and identifier assignment (`cap.account_opening`)
- Disclosure content authoring and legal document lifecycle (`domain.compliance_legal`)
- Channel UX, form rendering, and interaction design (`domain.channels_experience`)
- E-signature platform infrastructure and PKI management (`domain.technology_infrastructure`)

## 4. Upstream Dependencies
- `cap.deposit_account_product_management` for the product definition, disclosure mappings, and agreement templates
- `domain.customer` for verified customer identity and profile data to populate the application
- `domain.compliance_legal` for disclosure content, agreement templates, and regulatory requirements
- `domain.channels_experience` for the channel context in which the application and disclosures are presented

## 5. Downstream Dependencies
- `cap.account_opening` consumes the completed application and contract acceptance as a prerequisite for account creation
- `cap.account_terms_and_feature_management` uses the accepted terms as the basis for account-level configuration
- `domain.compliance_legal` receives contract evidence for retention and audit
- `domain.reporting_analytics` for application completion rates, disclosure presentment metrics, and exception tracking

## 6. Related Value Streams
- `vs.account_contract_establishment`
- `vs.deposit_account_acquisition`
- `vs.disclosures_and_agreements`
- `vs.regulatory_compliant_account_opening`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_checking_account`
- `journey.open_joint_account`
- `journey.accept_account_terms`
- `journey.correct_account_opening_document_issue`

## 8. Likely Processes
- Account application completion and data validation
- Disclosure presentment and acceptance capture
- Contract formation and agreement finalization
- E-signature or wet-signature execution
- Multi-party contract acceptance coordination
- Contract version control and effective dating
- Application exception routing and remediation
- Contract evidence packaging and retention handoff

## 9. Risks
- Missing required account disclosures at the point of sale creates regulatory exposure under Regulation DD
- Wrong contract version accepted due to product change during application creates enforceability questions
- Weak acceptance evidence (missing timestamps, unsigned documents, broken e-signature chains) undermines audit defensibility
- Product-to-contract drift where the product definition evolves but the agreement template does not keep pace
- Multi-party acceptance error where one joint owner's acceptance is missing or improperly recorded

## 10. Controls
- Account disclosure presentment requirements enforcing all required disclosures are shown before acceptance
- Contract version control ensuring the accepted version matches the current effective product definition
- Acceptance evidence traceability linking every acceptance to a timestamp, channel, identity, and document version
- Opening disclosure-to-product mapping validation confirming no disclosure gaps before account creation proceeds
- Multi-party contract acceptance rules requiring all required parties to accept before joint account opening

## 11. Typical Systems/Providers

### Typical systems
- Deposit application platform
- Disclosure presentment service
- E-signature platform
- Contract evidence repository
- Agreement versioning repository

### Typical providers
- DocuSign
- Q2
- Alkami
- nCino
- Temenos

## 12. Open Questions
- Should disclosure presentment tracking be a shared service across all domains or domain-specific?
- How should contract re-acceptance work when product terms change after account opening?
- Where does the boundary sit between account-level contract formation and broader customer-level agreements (e.g., master service agreements)?
- Should application abandonment and recovery be owned here or by an orchestration capability?
- How should multi-channel application continuity (start digital, finish in branch) be governed?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on the contractual and disclosure foundation of deposit account relationships. Informed by Regulation DD (Truth in Savings) disclosure requirements, ESIGN Act and UETA for electronic acceptance, and common account origination patterns in US retail banking. Boundary placement separates the contractual acceptance layer from both upstream product definition and downstream account record creation.
