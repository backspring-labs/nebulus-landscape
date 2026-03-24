---
id: journey.open_savings_account
type: journey
name: Open Savings Account
domain_ids:
  - domain.customer
  - domain.identity_access
  - domain.accounts
  - domain.risk_fraud
  - domain.payments
  - domain.channels_experience
  - domain.orchestration_control
capability_ids:
  - cap.customer_acquisition_management
  - cap.customer_application_intake
  - cap.customer_onboarding_orchestration
  - cap.customer_identity_establishment
  - cap.customer_party_resolution
  - cap.customer_due_diligence_coordination
  - cap.customer_profile_management
  - cap.customer_contact_management
  - cap.customer_consent_management
  - cap.customer_disclosure_attestation_management
  - cap.customer_lifecycle_state_management
value_stream_id: vs.deposit_account_opening
process_id: proc.account_opening_v1
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Open Savings Account

## 1. Objective

Enable a retail customer to successfully open and fund a savings account through a controlled path that establishes the customer relationship, performs required verification and due diligence, creates the product/account contract, and transitions the customer into an active post-onboarding state.

## 2. Primary Actor

Retail customer seeking to open and fund a savings account.

## 3. Trigger

The customer expresses intent to open a savings account through a digital channel, branch, partner channel, or assisted channel.

## 4. Preconditions

- The institution offers a savings product through the selected channel.
- Required onboarding, identity, and product policies are available.
- The customer can provide application and verification information.
- Product eligibility criteria are met at least at a preliminary level.
- Funding mechanisms are available if initial funding is required.

## 5. Happy Path

### Stage 1 — Prospect engagement and entry
The customer arrives through a marketing, referral, branch, partner, or direct digital entry point and selects a savings product.

**Capabilities activated:** `cap.customer_acquisition_management`

The customer's product interest and acquisition source are captured. Prospect context is created or resumed. The customer is handed into the application path.

### Stage 2 — Application start and intake
The customer begins the application and provides core intake data such as name, address, contact information, tax identity details, and product selections.

**Capabilities activated:** `cap.customer_application_intake`, `cap.customer_contact_management`, `cap.customer_disclosure_attestation_management`, `cap.customer_consent_management`

Required fields are collected and validated. Disclosures, attestations, and required acknowledgements are presented. Consents needed for communications, data handling, and relevant evidence capture are recorded.

### Stage 3 — Onboarding coordination
The institution starts and manages the onboarding case across milestones, pending items, and handoffs.

**Capabilities activated:** `cap.customer_onboarding_orchestration`

The onboarding case is created. Required milestones are sequenced. Exceptions, saves, resumes, and handoffs are coordinated.

### Stage 4 — Party recognition and identity establishment
The institution determines whether the applicant already exists and either links to the existing party or creates a new canonical customer record.

**Capabilities activated:** `cap.customer_party_resolution`, `cap.customer_identity_establishment`, `cap.customer_profile_management`

Existing-customer matching is performed. Duplicate risk is assessed at the customer-record level. A canonical customer identity is created or confirmed. Core profile attributes are established.

### Stage 5 — Verification and due diligence coordination
The institution coordinates required evidence collection and monitors the status of verification and due diligence outcomes.

**Capabilities activated:** `cap.customer_due_diligence_coordination`, `cap.customer_onboarding_orchestration`

**Participating neighbor domains:** `domain.identity_access`, `domain.risk_fraud`

Identity proofing, document review, or CDD/EDD-related evidence collection is coordinated. The Customer domain tracks completion state and evidence readiness. Neighbor domains own the actual verification or risk decisions.

### Stage 6 — Account creation and funding
Once customer establishment and controls are satisfied, the institution creates the savings account and enables funding.

**Participating neighbor domains:** `domain.accounts`, `domain.payments`

The account contract is created. Initial funding is initiated or accepted. Account status becomes active when prerequisites are met.

### Stage 7 — Lifecycle transition and completion
The institution transitions the customer relationship and onboarding case to an established or active state.

**Capabilities activated:** `cap.customer_lifecycle_state_management`, `cap.customer_onboarding_orchestration`

The customer lifecycle state moves from prospect/applicant to active or established. Completion status is recorded. Downstream servicing and relationship treatment can begin.

## 6. Alternate Paths

- **Existing customer adds a new savings account** — Party resolution identifies an existing customer, so the journey skips new-party creation and uses the existing canonical customer record.
- **Abandoned then resumed application** — The customer stops mid-application and later resumes from a saved state.
- **Branch-assisted or call-center-assisted completion** — A customer begins digitally but completes via an assisted channel.
- **No initial funding at open** — The account is created first and funded later if product policy allows.

## 7. Failure / Exception Paths

- Identity or due diligence requirements remain incomplete and onboarding cannot progress.
- Matching is ambiguous and manual review is required before customer creation.
- Required disclosures or attestations are missing or version-mismatched.
- Initial funding fails or is blocked by downstream payment/account constraints.
- Channel inconsistencies create an incomplete or invalid application state.
- The customer withdraws before completion.

## 8. Related Domains

- `domain.customer` — owns relationship establishment, intake, orchestration, customer identity, consent/disclosures, and lifecycle transition
- `domain.identity_access` — performs identity proofing execution and possibly digital-access creation after successful establishment
- `domain.accounts` — owns savings account contract creation and maintenance
- `domain.risk_fraud` — owns fraud, AML, sanctions, and related decisioning
- `domain.payments` — owns funding and money movement
- `domain.channels_experience` — owns UI presentation and channel flow
- `domain.orchestration_control` — may provide enterprise workflow/policy execution beneath the business journey

## 9. Related Capabilities

- `cap.customer_acquisition_management`
- `cap.customer_application_intake`
- `cap.customer_onboarding_orchestration`
- `cap.customer_identity_establishment`
- `cap.customer_party_resolution`
- `cap.customer_due_diligence_coordination`
- `cap.customer_profile_management`
- `cap.customer_contact_management`
- `cap.customer_consent_management`
- `cap.customer_disclosure_attestation_management`
- `cap.customer_lifecycle_state_management`

## 10. Value Stream Context

This journey primarily sits inside `vs.deposit_account_opening`, with strong ties to customer onboarding, trusted relationship establishment, and initial activation.

## 11. Supporting Process Candidates

- `proc.account_opening_v1`
- `proc.application_start`
- `proc.application_validation`
- `proc.onboarding_milestone_management`
- `proc.pre_create_match_screening`
- `proc.canonical_customer_creation`
- `proc.due_diligence_completion`
- `proc.onboarding_completion`

## 12. Key Risks and Controls Encountered

### Key risks
- Incomplete or invalid application data
- Duplicate or fragmented customer creation
- Missing consent or disclosure evidence
- Due diligence incompleteness
- Product opening before customer controls are satisfied
- Funding before prerequisites are complete

### Key controls
- Required-field and cross-field validation
- Match and duplicate review before customer creation
- Controlled onboarding dependency matrix
- Disclosure and consent evidence capture
- Due diligence completion gate
- Lifecycle state transition controls before downstream activation

## 13. Typical Systems and Providers Involved

### Typical systems
- Digital onboarding platform
- Application origination platform
- Customer master / CIF
- Party match service
- Onboarding journey manager
- KYC/evidence workbench
- Core account-opening platform
- Payment/funding service

### Representative providers
- Alloy
- Socure
- LexisNexis Risk Solutions
- Trulioo
- MeridianLink
- nCino
- Temenos
- Fiserv

## 14. Open Questions

- Should product selection be modeled as part of the journey entry stage or as a distinct pre-application journey?
- At what point should digital access creation be represented in this journey versus a separate Identity & Access journey?
- How much of funding should remain in this journey if payment execution is deeply decomposed later?

## 15. Provenance

This journey is the flagship Customer-domain example because it exercises the strongest cross-domain thread from acquisition through active relationship and product establishment. It is the clearest demonstration of where Customer ownership ends and adjacent domains begin.
