---
id: cap.customer_identity_establishment
type: capability
name: Customer Identity Establishment
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Customer Identity Establishment

## 1. Definition

Customer Identity Establishment is the enduring business ability to create and maintain the institution's canonical business identity record for a person or organization at the customer-relationship level once sufficient onboarding and resolution criteria are met.

## 2. Business Outcome

Provide a trusted, durable, institution-recognized customer identity anchor that can be referenced consistently across products, accounts, servicing, risk, and analytics without conflating the customer record with credentials, login accounts, or external proofing events.

## 3. Scope

### Included activities
- Creating the canonical customer/party record once the institution is ready to recognize a relationship participant
- Assigning durable internal identifiers to the customer/party
- Establishing identity attributes needed for the institution's business understanding of the customer
- Defining survivorship and source-of-truth rules for customer identity attributes at the business relationship layer
- Recording relationship-relevant identity composition for individuals and organizations, including aliases, legal names, tax identity references, and party type
- Maintaining the authoritative relationship-level customer identity object over time
- Linking the customer record to related parties, applicant artifacts, applications, and downstream accounts
- Distinguishing customer identity from prospect records, draft applicants, and authentication identities

### Excluded activities
- Identity proofing, document verification, biometric checks, liveness checks, and credentialed identity verification (`domain.identity_access`)
- Authentication credentials, authorization, and session trust (`domain.identity_access`)
- Fraud or AML decisioning (`domain.risk_fraud`)
- Existing-party matching algorithms and duplicate resolution ownership (`cap.customer_party_resolution`), though identity establishment consumes their outcomes
- Contact preferences, marketing preferences, and broad consent governance if modeled separately
- Product/account ownership records and contractual account-holder relationships (`domain.accounts`)

## 4. Upstream Dependencies
- Applicant data from `cap.customer_application_intake`
- Party-match outcomes from `cap.customer_party_resolution`
- Verification results from `domain.identity_access`
- Risk disposition signals from `domain.risk_fraud` where needed before canonical creation
- Onboarding progression status from `cap.customer_onboarding_orchestration`

## 5. Downstream Dependencies
- `domain.accounts` for account-holder/customer references
- `domain.servicing` for customer-centric servicing lookup and relationship context
- `domain.risk_fraud` for ongoing monitoring against canonical party identity
- `domain.data_rewards` and analytics domains for customer-level reporting
- Customer capabilities such as profile, contact, consent, lifecycle, and relationship governance

## 6. Related Value Streams
- `vs.customer_onboarding`
- `vs.customer_mastering`
- `vs.enterprise_party_management`
- `vs.trusted_customer_relationship`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_checking_account`
- `journey.existing_customer_adds_new_relationship`
- `journey.correct_duplicate_customer_record`
- `journey.branch_assisted_customer_onboarding`

## 8. Likely Processes
- Canonical customer record creation
- Internal identifier assignment
- Identity attribute survivorship management
- Party-link establishment
- Canonical record update after onboarding
- Prospect/applicant to customer conversion
- Organizational party creation and principal linkage

## 9. Risks
- Canonical customer record is created prematurely before sufficient evidence or resolution
- Multiple canonical records are created for the same real-world party
- Customer identity record is polluted with non-authoritative channel or session data
- Party identifiers change or are re-used improperly
- Individual and organization identity structures are modeled inconsistently
- Downstream systems use local identifiers instead of the canonical customer identity
- Institution cannot explain which attributes are authoritative and why

## 10. Controls
- Preconditions for canonical identity creation
- Durable identifier generation and non-reuse policy
- Source-of-truth and survivorship rules for core identity attributes
- Mandatory linkage to onboarding/application provenance
- Controlled merge/unmerge governance with approval and audit
- Canonical customer publish/subscribe or mastering controls to downstream systems
- Attribute stewardship rules for legal name, tax identity reference, and party type
- Periodic duplicate and orphan-record review

## 11. Typical Systems/Providers

### Typical systems
- Customer master / party master
- MDM / golden record platforms
- CIF / enterprise customer information files
- Party relationship repositories
- Customer API layers exposing canonical IDs
- Data stewardship workbenches

### Typical providers
- Informatica MDM
- Reltio
- IBM Match 360
- Oracle Customer Hub
- Salesforce FSC person-account or party models where used
- Fiserv, FIS, Jack Henry CIF/customer master components
- In-house customer master services in modern banks/fintechs

## 12. Open Questions
- Should identity establishment wait until onboarding completion, or can the model allow earlier provisional-customer creation with later hardening?
- How should this capability handle legal entities and beneficial-owner/principal party structures if business banking later becomes more prominent?
- Where should tax identity references and sensitive identifiers be governed if a future data-governance layer becomes explicit?
- Should the canonical entity be named "customer identity" or "customer party" to better align with enterprise party models?

## 13. Provenance

This capability reflects a deliberate distinction between relationship-level customer identity and security/verification identity. In practice, banks often blur these concerns; the model separates them because durable business architecture needs a canonical party/customer anchor independent of proofing tools and login systems.
