---
id: domain.customer
type: domain
name: Customer
status: draft
source_refs:
  - src.domain_map_v1
last_reviewed: 2026-03-24
confidence: high
---

# Customer

## 1. Definition

The Customer domain is the durable business area responsible for establishing, maintaining, and governing the institution's relationship with a customer or prospective customer. It covers the lifecycle from prospect and applicant through active relationship maintenance and eventual dormancy or exit. The domain owns the institution's business view of the customer as a party in relationship with the institution, including customer intake, customer record creation, profile maintenance, contactability, consent and preference governance, relationship status, and the coordination required to progress a person or business from interest to established customer.

This domain does **not** own authentication, session control, account product fulfillment, transaction execution, or credit/fraud decisioning. It does, however, provide the customer context those domains depend on.

A practical boundary is:
- **Customer** owns the business relationship and customer record.
- **Identity & Access** owns credentials, authentication, authorization, session trust, and often the more technical identity proofing components.
- **Accounts** owns the account contract and account lifecycle.
- **Risk & Fraud** owns risk evaluation and fraud/AML decisioning, even when those decisions occur during onboarding.

## 2. Business Purpose

Enable the institution to acquire, establish, maintain, and govern trusted customer relationships in a way that is operationally usable, regulatorily defensible, and reusable across journeys.

Without a coherent Customer domain:
- channels cannot reliably identify who the institution is dealing with,
- onboarding cannot progress consistently,
- downstream account, payment, lending, and servicing processes lack a dependable customer anchor,
- consent and communications become non-compliant or fragmented,
- customer change management becomes expensive and error-prone.

In banking and fintech, Customer is the durable relationship substrate that allows product, risk, servicing, and channel capabilities to act on a common, governed customer record rather than each reconstructing their own version of the truth.

## 3. Scope

### Includes

The Customer domain includes capabilities required to:
- attract and intake prospective customers into institution-managed relationship pipelines,
- establish a customer record and relationship identity at the business level,
- collect and maintain core customer and household/business profile data,
- manage contact points and communication preferences,
- manage consent, permissions, attestations, and disclosures tied to the customer relationship,
- coordinate onboarding progression across customer-owned steps,
- manage customer lifecycle state from prospect through active and closed relationship states,
- manage relationship context such as segmentation, relationship ownership, and household or organization linkage where modeled at the customer level,
- support customer data correction, update, remediation, and governance workflows.

### Excludes

- **Authentication and credential management** → `domain.identity_access`
  - login, MFA, device/session trust, authorization, entitlements.
- **Identity proofing execution and access identity administration** → `domain.identity_access`
  - this domain may invoke or depend on identity verification, but does not own the underlying access identity stack.
- **Account opening, account maintenance, account closure** → `domain.accounts`
  - Customer establishes the party relationship; Accounts establishes the product/account contract.
- **Fraud scoring, KYC/AML decisioning, sanctions screening, case investigation** → `domain.risk_fraud`
  - Customer participates in the workflow and may hold resulting status flags or requirements, but does not own the decision engines.
- **Payment initiation and money movement** → `domain.payments`
- **Workflow orchestration and enterprise policy enforcement engines** → `domain.orchestration_control`
- **UI presentation and channel interaction management** → `domain.channels_experience`
- **General customer service case handling, complaints, disputes** → `domain.servicing`
- **Behavioral analytics, loyalty, rewards, monetization of data** → `domain.data_rewards`

### Boundary recommendations

- **Customer profile vs identity management**: customer profile belongs in `domain.customer`; authentication identity, credentials, entitlements, and session state belong in `domain.identity_access`.
- **Consent management vs data governance**: customer-facing capture and maintenance of customer consent belongs in `domain.customer`; enterprise-wide data policy, lineage, retention, and governance standards belong elsewhere, likely a future data governance domain.
- **Lifecycle state**: customer lifecycle state should be modeled as a distinct capability, not buried inside profile maintenance. It is business-significant, journey-relevant, and often drives controls.

## 4. Included Capabilities

The following capabilities are proposed as the durable capability set for `domain.customer`.

- `cap.customer_acquisition_management` — Manage institution-led capture and progression of prospects into qualified customer onboarding opportunities.
- `cap.customer_application_intake` — Receive, normalize, validate, and manage customer-supplied application and onboarding intake information.
- `cap.customer_onboarding_orchestration` — Coordinate customer-owned onboarding progression, milestones, handoffs, exceptions, and completion status.
- `cap.customer_identity_establishment` — Establish the institution's canonical customer identity record at the business relationship level.
- `cap.customer_party_resolution` — Match, de-duplicate, and resolve whether an applicant/prospect corresponds to an existing or new customer.
- `cap.customer_due_diligence_coordination` — Coordinate collection and progression of customer due diligence requirements while external decisioning remains owned by `domain.risk_fraud` and `domain.identity_access`.
- `cap.customer_profile_management` — Create and maintain the core customer profile and its business attributes.
- `cap.customer_contact_management` — Manage customer contact points such as address, email, phone, and their usability/effectivity.
- `cap.customer_preference_management` — Manage customer communication, language, channel, and servicing preferences.
- `cap.customer_consent_management` — Capture, maintain, evidence, and revoke customer consents, permissions, and data-sharing approvals.
- `cap.customer_disclosure_attestation_management` — Capture customer acknowledgements, attestations, certifications, and disclosure acceptance during onboarding and maintenance.
- `cap.customer_relationship_management` — Manage the institution's relationship context with the customer, including ownership, segment, relationship roles, and profitability/service tier context where applicable.
- `cap.customer_householding_and_association_management` — Manage customer-to-customer, customer-to-business, and household/affiliation relationships used for servicing, risk context, and marketing alignment.
- `cap.customer_lifecycle_state_management` — Manage customer lifecycle stages and state transitions such as prospect, applicant, active, restricted, dormant, exited, or archived.
- `cap.customer_change_management` — Govern updates, corrections, effective dating, approvals, and evidence for changes to customer data.
- `cap.customer_data_issue_remediation` — Resolve incomplete, inconsistent, disputed, or exception customer data that blocks or degrades downstream operations.
- `cap.customer_record_retention_and_archival` — Manage retention, archival, suppression, and disposition of customer relationship records in accordance with policy.
- `cap.customer_exit_management` — Manage customer-level exit, relationship termination, and post-relationship handling distinct from account closure.

### Capability notes

A few boundaries are often blurry:
- `cap.customer_application_intake` vs `cap.customer_onboarding_orchestration`: intake is about receiving and validating information; orchestration is about moving the customer through the end-to-end progression.
- `cap.customer_identity_establishment` vs `cap.customer_party_resolution`: establishment creates the business identity; party resolution determines whether the party already exists.
- `cap.customer_profile_management` vs `cap.customer_change_management`: profile is the steady-state maintenance capability; change management governs how changes are controlled and evidenced.
- `cap.customer_relationship_management` may partially overlap with CRM-heavy operating models. It should remain if the repo needs business relationship constructs beyond mere profile storage.

## 5. Excluded Capabilities

- `cap.authentication` → `domain.identity_access`
  - Concerns runtime access trust, not the durable customer relationship.
- `cap.authorization_and_entitlements` → `domain.identity_access`
- `cap.identity_verification_execution` → `domain.identity_access`
  - Customer may request and consume results but does not own the verification service itself.
- `cap.account_opening` → `domain.accounts`
  - Often confused because onboarding journeys cross both domains.
- `cap.account_maintenance` → `domain.accounts`
- `cap.payment_funding` / `cap.transfer_execution` → `domain.payments`
- `cap.fraud_decisioning`, `cap.kyc_aml_decisioning`, `cap.sanctions_screening` → `domain.risk_fraud`
- `cap.case_management` / `cap.dispute_handling` → `domain.servicing`
- `cap.channel_session_management` / `cap.digital_experience_orchestration` → `domain.channels_experience`
- `cap.workflow_engine_management` / `cap.policy_engine_execution` → `domain.orchestration_control`
- `cap.customer_analytics` / `cap.rewards_management` → `domain.data_rewards`

## 6. Upstream Dependencies

The Customer domain typically depends on:

- `domain.channels_experience`
  - customer and prospect entry points, assisted channels, application capture UX, communication surfaces.
- `domain.identity_access`
  - identity proofing services, document verification, possibly access identity creation after onboarding.
- `domain.risk_fraud`
  - due diligence checks, sanctions/AML/KYC outcomes, fraud risk decisions.
- `domain.orchestration_control`
  - workflow routing, rules execution, policy sequencing, exception orchestration.
- external lead sources / marketing systems
  - prospect generation and referral flows, where in scope.
- document and e-sign services
  - disclosure presentation, signature evidence, attestation capture.
- reference data and address/contact utilities
  - address normalization, postal verification, contact validation.

## 7. Downstream Dependencies

The Customer domain commonly provides data, status, and control points to:

- `domain.accounts`
  - account opening cannot complete without an established customer and sufficient onboarding state.
- `domain.payments`
  - funding and external transfer setup depend on a known customer and consent/comms context.
- `domain.servicing`
  - customer service requires current profile, contactability, lifecycle state, and relationship context.
- `domain.data_rewards`
  - analytics, segmentation, and rewards typically consume customer relationship data and preference/consent context.
- `domain.identity_access`
  - access identity creation or linkage may occur after customer establishment.
- `domain.risk_fraud`
  - ongoing monitoring may depend on customer lifecycle state changes, profile updates, and association changes.

## 8. Related Journeys

Journeys that pass through or materially depend on the Customer domain include:

- `journey.open_savings_account`
  - flagship example; intake, identity establishment, due diligence coordination, consent capture, lifecycle transition.
- `journey.open_checking_account`
- `journey.open_joint_account`
- `journey.customer_onboarding`
- `journey.update_contact_information`
- `journey.update_customer_profile`
- `journey.manage_marketing_preferences`
- `journey.manage_data_sharing_consent`
- `journey.reactivate_dormant_customer`
- `journey.resolve_customer_data_issue`
- `journey.exit_customer_relationship`
- `journey.convert_prospect_to_customer`
- `journey.add_beneficial_owner_or_related_party`
- `journey.small_business_customer_onboarding`

Some of these journeys will ultimately span multiple domains. They are listed here because Customer is either the primary business anchor or a mandatory participating domain.

## 9. Typical Risks and Controls

### Typical Risks

- Duplicate or fragmented customer records lead to inconsistent servicing, risk blind spots, or regulatory issues.
- Incorrect customer identity establishment leads to downstream account or compliance failures.
- Incomplete onboarding data causes failed fulfillment, delayed activation, or manual rework.
- Consent is missing, stale, or inappropriately applied, creating compliance exposure.
- Customer profile changes occur without sufficient verification, approval, or evidence.
- Contact information becomes invalid, resulting in failed disclosures or servicing notices.
- Lifecycle state is inaccurate, causing servicing, marketing, or compliance missteps.
- Related-party or household relationships are missing or wrong, weakening risk or customer treatment decisions.
- Customer exit or archival is mishandled, resulting in retention, privacy, or re-engagement errors.

### Typical Controls

- Deterministic party matching and duplicate review workflows before new customer creation.
- Required-field, format, and eligibility validation on intake.
- Evidence-backed identity establishment before downstream activation steps.
- Controlled state transition rules for prospect/applicant/active/restricted/dormant/exited states.
- Consent capture with timestamp, channel, version, purpose, and revocation history.
- Dual control or step-up verification for sensitive profile changes.
- Address/contact verification and periodic recertification triggers.
- Effective-dated customer record changes with audit trail.
- Exception queues for unresolved or conflicting customer data.
- Retention and archival policies enforced by lifecycle and regulatory triggers.

## 10. Typical Systems and Providers

These are representative patterns, not prescribed architecture.

### Typical system categories

- Customer master / customer information file (CIF)
- CRM / relationship management platform
- onboarding and application intake platform
- consent and preference management platform
- document collection / disclosure / e-sign platform
- party matching / MDM tools
- address, phone, email validation services
- workflow/orchestration platforms
- case/exception remediation tools

### Representative providers often seen around this domain

- `prov.salesforce` — CRM / relationship and onboarding workflow support
- `prov.alloy` — onboarding coordination, identity/risk integrations, intake orchestration adjacency
- `prov.socure` — identity and verification adjacency used during onboarding
- `prov.trulioo` — identity and business verification adjacency
- `prov.lexisnexis_risk_solutions` — verification and due diligence adjacency
- `prov.plaid` — data-sharing consent and account-linking adjacency in certain journeys
- `prov.twilio` — contactability and communications adjacency
- `prov.docu_sign` — disclosure, consent, and signature evidence adjacency
- core banking customer master / CIF platforms
- bespoke institution customer master and onboarding stacks

Provider placement should remain careful: many vendors span multiple domains. In the canonical model, vendors should link to capabilities they support without redefining domain ownership.

## 11. Open Questions

- Should `cap.customer_acquisition_management` remain inside `domain.customer` if marketing domains are later introduced, or should it be narrowed to prospect intake only?
- How strongly should `cap.customer_relationship_management` be modeled in a retail-first landscape where some institutions treat CRM as a channel or sales concern?
- Should `cap.customer_householding_and_association_management` be first-class in v1, or modeled later once household, beneficiary, and business-party patterns are broader in the repo?
- Should disclosure acceptance and attestation remain bundled under consent, or stay separate as `cap.customer_disclosure_attestation_management`? Recommendation is to keep it separate because legal acceptance and data-sharing consent are often different control objects.
- How far should `cap.customer_exit_management` extend before overlapping with servicing, complaints, or account closure flows?
- In institutions with a strong enterprise MDM function, where should enterprise party golden-record stewardship end and customer business ownership begin?

## 12. Provenance

This artifact expands the bootstrap Customer domain seed into a research-grade domain definition for a capability-first banking/fintech landscape model. It preserves the original seed intent around acquisition, onboarding, profile, consent, and lifecycle, while separating durable capabilities and clarifying boundaries with adjacent domains.
