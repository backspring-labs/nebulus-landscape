---
id: cap.account_opening
type: capability
name: Account Opening
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Opening

## 1. Definition

Account Opening is the enduring business ability to establish a new deposit account contract for an eligible customer under a selected product once all required upstream conditions — identity verification, due diligence, product selection, and application acceptance — are satisfied. It is the act of creating the account record of authority and binding the customer-product-account relationship in the institution's systems.

## 2. Business Outcome

Reliably create deposit account records that are correctly bound to the right customer, the right product, and the right terms so the account is immediately recognizable, governable, and ready for downstream activation, funding, and servicing.

## 3. Scope

### Included activities
- Creating the account record in the core or account system of record
- Assigning account identifiers (account number, internal keys, routing references)
- Binding the account to the selected product definition and its rule package
- Binding the account to the verified customer or party record
- Recording the account's initial state (e.g., open-pending, open-active, open-restricted)
- Executing prerequisite checks to confirm upstream conditions are met before opening
- Handling joint account creation with multiple party bindings
- Managing account opening exceptions and resolution workflows
- Producing the account opening event for downstream consumers

### Excluded activities
- Customer identity verification, due diligence, or onboarding (`domain.customer`, `domain.identity_access`, `domain.risk_fraud`)
- Product definition and product catalog management (`cap.deposit_account_product_management`)
- Application capture, disclosure presentment, and contract acceptance (`cap.account_application_and_contract_formation`)
- Account-level terms and feature instantiation (`cap.account_terms_and_feature_management`)
- Initial funding execution or payment processing (`domain.payments`)
- Account funding readiness evaluation (`cap.account_funding_readiness_management`)
- Channel UX and origination form rendering (`domain.channels_experience`)

## 4. Upstream Dependencies
- `cap.deposit_account_product_management` for the product definition the account will be opened under
- `cap.account_application_and_contract_formation` for completed application and accepted contract
- `domain.customer` for a verified, resolved customer or party record
- `domain.identity_access` for identity proofing and CIP completion
- `domain.risk_fraud` for risk and fraud screening clearance

## 5. Downstream Dependencies
- `cap.account_terms_and_feature_management` to instantiate product terms onto the new account
- `cap.account_funding_readiness_management` to evaluate and enable funding eligibility
- `cap.account_party_role_management` to formalize party roles on the account
- `domain.transaction_operations` for transaction enablement once the account is active
- `domain.channels_experience` for account visibility in digital and assisted channels
- `domain.reporting_analytics` for new-account reporting and regulatory filing

## 6. Related Value Streams
- `vs.account_contract_establishment`
- `vs.deposit_account_acquisition`
- `vs.customer_to_account_conversion`
- `vs.account_activation_readiness`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_checking_account`
- `journey.open_joint_account`
- `journey.small_business_relationship_start`
- `journey.resolve_account_opening_exception`

## 8. Likely Processes
- Account opening execution with prerequisite validation
- Joint account opening with multi-party binding
- Account opening exception capture and resolution
- Account activation preparation and state transition
- Account identifier assignment and uniqueness verification
- Cross-system account record synchronization
- Account opening event publication

## 9. Risks
- Account opened without all preconditions satisfied (identity, due diligence, application, product selection)
- Wrong party or product bound to the account due to data mapping or timing errors
- Duplicate account creation for the same customer-product combination
- Cross-system opening inconsistency where the account exists in one system but not another
- Account opened but not usable due to missing downstream activation steps

## 10. Controls
- Account opening prerequisite matrix enforcing all upstream conditions before record creation
- Account identifier uniqueness validation before assignment
- Opening preconditions attestation with auditable evidence of each satisfied requirement
- Cross-system opening reconciliation to detect and resolve divergence between systems of record
- Account opening audit trail capturing who, when, what product, what party, and what channel

## 11. Typical Systems/Providers

### Typical systems
- Account origination platform
- Core deposit account creation service
- Account identifier service
- Opening workbench (branch/call center)
- Opening exception dashboard

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- nCino
- Q2

## 12. Open Questions
- Should account opening own the state machine for account lifecycle states, or should that be a separate capability?
- How should the opening process handle partial failures where some downstream systems accept the account but others do not?
- What is the boundary between account opening exception resolution and broader operational exception management?
- Should account opening publish a domain event or rely on core system change-data-capture for downstream notification?
- How should opening handle accounts that require regulatory hold periods before activation?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on the account creation act itself. Informed by FFIEC CIP guidance on account opening requirements, core banking account creation patterns, and common origination-to-core integration models in US retail and commercial banking. Boundary placement separates the account record creation act from upstream eligibility and downstream activation concerns.
