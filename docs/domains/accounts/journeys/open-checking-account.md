---
id: journey.open_checking_account
type: journey
name: Open Checking Account
domain_ids:
  - domain.accounts
  - domain.customer
  - domain.identity_access
  - domain.risk_fraud
  - domain.payments
  - domain.channels_experience
  - domain.orchestration_control
capability_ids:
  - cap.account_opening
  - cap.account_application_and_contract_formation
  - cap.account_party_role_management
  - cap.account_terms_and_feature_management
  - cap.account_funding_readiness_management
  - cap.account_status_and_restriction_management
  - cap.account_overdraft_and_protection_management
  - cap.account_interest_and_fee_configuration_management
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
last_reviewed: 2026-03-25
confidence: high
---

# Open Checking Account

## 1. Objective

Enable a retail customer to successfully open and fund a checking account through a controlled path that establishes or confirms the customer relationship, performs required verification and due diligence, creates the checking account contract with its specific terms and features, enables initial funding and optional overdraft protection, and transitions the account into an active servicing state.

## 2. Primary Actor

Retail customer seeking to open and fund a checking account.

## 3. Trigger

The customer expresses intent to open a checking account through a digital channel, branch, partner channel, or assisted channel.

## 4. Preconditions

- The institution offers a checking product through the selected channel.
- Required onboarding, identity, and product policies are available.
- The customer can provide application and verification information.
- Product eligibility criteria are met at least at a preliminary level.
- Funding mechanisms are available if initial funding is required.

## 5. Happy Path

### Stage 1 — Prospect engagement and entry
The customer arrives through a marketing, referral, branch, partner, or direct digital entry point and selects a checking product.

**Capabilities activated:** `cap.customer_acquisition_management`, `cap.account_opening`

The customer's product interest and acquisition source are captured. Prospect context is created or resumed. The customer is handed into the application path. The Accounts domain registers the product selection and begins account-opening coordination.

### Stage 2 — Application and contract formation
The customer provides core intake data and the institution assembles the checking account application, capturing product-specific terms, features, and contractual elements.

**Capabilities activated:** `cap.account_application_and_contract_formation`, `cap.customer_application_intake`, `cap.customer_contact_management`, `cap.customer_disclosure_attestation_management`, `cap.customer_consent_management`

Required fields are collected and validated. Checking-specific disclosures, fee schedules, and terms are presented. Consents and attestations are recorded. The account application record is formed.

### Stage 3 — Onboarding coordination
The institution starts and manages the onboarding case across milestones, pending items, and handoffs.

**Capabilities activated:** `cap.customer_onboarding_orchestration`

The onboarding case is created. Required milestones are sequenced. Exceptions, saves, resumes, and handoffs are coordinated.

### Stage 4 — Party recognition and identity establishment
The institution determines whether the applicant already exists and either links to the existing party or creates a new canonical customer record.

**Capabilities activated:** `cap.customer_party_resolution`, `cap.customer_identity_establishment`, `cap.customer_profile_management`, `cap.account_party_role_management`

Existing-customer matching is performed. Duplicate risk is assessed. A canonical customer identity is created or confirmed. The party-to-account relationship and role assignments are established.

### Stage 5 — Verification and due diligence coordination
The institution coordinates required evidence collection and monitors the status of verification and due diligence outcomes.

**Capabilities activated:** `cap.customer_due_diligence_coordination`, `cap.customer_onboarding_orchestration`

**Participating neighbor domains:** `domain.identity_access`, `domain.risk_fraud`

Identity proofing, document review, or CDD/EDD-related evidence collection is coordinated. The Customer domain tracks completion state and evidence readiness. Neighbor domains own the actual verification or risk decisions.

### Stage 6 — Terms, features, and configuration
Once customer establishment and controls are satisfied, the institution finalizes the checking account terms, features, and configurations.

**Capabilities activated:** `cap.account_terms_and_feature_management`, `cap.account_interest_and_fee_configuration_management`, `cap.account_overdraft_and_protection_management`

Product terms are applied. Fee schedules and interest configurations are set. Overdraft protection elections are captured and configured or deferred for later setup.

### Stage 7 — Funding readiness and initial funding
The institution enables and accepts initial funding for the checking account.

**Capabilities activated:** `cap.account_funding_readiness_management`, `cap.account_status_and_restriction_management`

**Participating neighbor domains:** `domain.payments`

Funding readiness prerequisites are validated. Initial funding is initiated or accepted. The account transitions toward active status once funding prerequisites are met.

### Stage 8 — Lifecycle transition and completion
The institution transitions the account and customer relationship to an established or active state.

**Capabilities activated:** `cap.account_status_and_restriction_management`, `cap.customer_lifecycle_state_management`, `cap.customer_onboarding_orchestration`

The account status moves to active. The customer lifecycle state moves from prospect/applicant to active or established. Completion status is recorded. Downstream servicing and relationship treatment can begin.

## 6. Alternate Paths

- **Existing customer adds a new checking account** — Party resolution identifies an existing customer, so the journey skips new-party creation and uses the existing canonical customer record.
- **Abandoned then resumed application** — The customer stops mid-application and later resumes from a saved state.
- **Branch-assisted or call-center-assisted completion** — A customer begins digitally but completes via an assisted channel.
- **No initial funding at open** — The account is created first and funded later if product policy allows.
- **Overdraft protection deferred** — The customer elects to configure overdraft protection after account opening rather than during.
- **Joint account opening** — Multiple parties are associated with the account, requiring additional party resolution and role assignment.

## 7. Failure / Exception Paths

- Identity or due diligence requirements remain incomplete and onboarding cannot progress.
- Matching is ambiguous and manual review is required before customer creation.
- Required disclosures or attestations are missing or version-mismatched.
- Initial funding fails or is blocked by downstream payment/account constraints.
- Product eligibility is not satisfied after full evaluation.
- Channel inconsistencies create an incomplete or invalid application state.
- The customer withdraws before completion.

## 8. Related Domains

- `domain.accounts` — owns checking account contract creation, terms and feature configuration, funding readiness, overdraft protection, status management, and party role assignment
- `domain.customer` — owns relationship establishment, intake, orchestration, customer identity, consent/disclosures, and lifecycle transition
- `domain.identity_access` — performs identity proofing execution and possibly digital-access creation after successful establishment
- `domain.risk_fraud` — owns fraud, AML, sanctions, and related decisioning
- `domain.payments` — owns funding and money movement
- `domain.channels_experience` — owns UI presentation and channel flow
- `domain.orchestration_control` — may provide enterprise workflow/policy execution beneath the business journey

## 9. Related Capabilities

- `cap.account_opening`
- `cap.account_application_and_contract_formation`
- `cap.account_party_role_management`
- `cap.account_terms_and_feature_management`
- `cap.account_funding_readiness_management`
- `cap.account_status_and_restriction_management`
- `cap.account_overdraft_and_protection_management`
- `cap.account_interest_and_fee_configuration_management`
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

This journey primarily sits inside `vs.deposit_account_opening`. It shares the same value stream as the Open Savings Account journey but exercises additional Accounts-domain capabilities around checking-specific terms, overdraft protection, and fee configuration. It has strong ties to customer onboarding, trusted relationship establishment, and initial activation.

## 11. Supporting Process Candidates

- `proc.account_opening_v1`
- `proc.application_start`
- `proc.application_validation`
- `proc.onboarding_milestone_management`
- `proc.pre_create_match_screening`
- `proc.canonical_customer_creation`
- `proc.due_diligence_completion`
- `proc.account_terms_configuration`
- `proc.overdraft_protection_setup`
- `proc.funding_readiness_check`
- `proc.onboarding_completion`

## 12. Key Risks and Controls Encountered

### Key risks
- Incomplete or invalid application data
- Duplicate or fragmented customer creation
- Missing consent or disclosure evidence
- Due diligence incompleteness
- Product opening before customer controls are satisfied
- Funding before prerequisites are complete
- Misconfigured overdraft protection or fee terms
- Incorrect party role assignment on joint accounts

### Key controls
- Required-field and cross-field validation
- Match and duplicate review before customer creation
- Controlled onboarding dependency matrix
- Disclosure and consent evidence capture
- Due diligence completion gate
- Terms and feature validation against product catalog
- Lifecycle state transition controls before downstream activation
- Overdraft election confirmation and disclosure requirements

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
- Overdraft management module

### Representative providers
- Alloy
- Socure
- LexisNexis Risk Solutions
- Trulioo
- MeridianLink
- nCino
- Temenos
- Fiserv
- Jack Henry

## 14. Open Questions

- How much overlap exists with the Open Savings Account journey and should shared stages be factored into a common account-opening sub-journey?
- Should overdraft protection configuration be modeled as a separate post-opening journey when deferred?
- At what point should digital access creation be represented in this journey versus a separate Identity & Access journey?
- How much of funding should remain in this journey if payment execution is deeply decomposed later?
- Should joint-account opening be treated as a variant of this journey or a distinct journey?

## 15. Provenance

This journey is the Accounts-domain counterpart to the Customer-domain Open Savings Account journey. It is authored from the Accounts domain perspective, exercising the full set of account-specific capabilities — contract formation, terms and feature management, overdraft protection, funding readiness, and status management — while still depending on the Customer domain for relationship establishment and onboarding orchestration. It demonstrates the Accounts domain's ownership boundary for deposit product opening.
