---
id: journey.customer_onboarding
type: journey
name: Customer Onboarding
domain_ids:
  - domain.customer
  - domain.identity_access
  - domain.risk_fraud
  - domain.channels_experience
  - domain.orchestration_control
capability_ids:
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
value_stream_id: vs.customer_onboarding
process_id: proc.customer_onboarding_v1
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Customer Onboarding

## 1. Objective

Establish a new-to-bank individual as a governed customer of the institution independent of any specific downstream product opening.

## 2. Primary Actor

Prospective new-to-bank individual.

## 3. Trigger

A prospect starts an onboarding journey intended to become a recognized customer of the institution.

## 4. Preconditions

- The institution supports a customer-establishment path independent of immediate product creation, or conceptually separates customer establishment from account opening.
- The individual can provide required identity, contact, and due diligence information.
- Onboarding policies and evidence requirements are available.

## 5. Happy Path

### Stage 1 — Entry into onboarding
The applicant begins the onboarding journey directly or via a lead/prospect handoff.

**Capabilities activated:** `cap.customer_application_intake`, `cap.customer_onboarding_orchestration`

### Stage 2 — Core information capture
The institution captures core personal and contact information, disclosures, certifications, and required permissions.

**Capabilities activated:** `cap.customer_application_intake`, `cap.customer_profile_management`, `cap.customer_contact_management`, `cap.customer_consent_management`, `cap.customer_disclosure_attestation_management`

### Stage 3 — Existing-customer check
The institution determines whether the applicant already exists or whether a new customer identity must be created.

**Capabilities activated:** `cap.customer_party_resolution`, `cap.customer_identity_establishment`

### Stage 4 — Due diligence coordination
Customer-owned onboarding coordinates required evidence and monitors verification and due diligence completion.

**Capabilities activated:** `cap.customer_due_diligence_coordination`, `cap.customer_onboarding_orchestration`

### Stage 5 — Customer establishment
The institution creates or confirms the canonical customer record and marks the onboarding journey complete.

**Capabilities activated:** `cap.customer_identity_establishment`, `cap.customer_lifecycle_state_management`

## 6. Alternate Paths

- Existing-customer recognition converts the path into relationship extension rather than new-customer establishment.
- Assisted onboarding continues across multiple channels.
- Additional evidence is requested after initial intake.
- The applicant is partially onboarded and resumed later.

## 7. Failure / Exception Paths

- Ambiguous match blocks customer establishment.
- Required attestation evidence is missing.
- Due diligence evidence cannot be completed.
- The applicant abandons onboarding before completion.
- Customer data issues require remediation before establishment can proceed.

## 8. Related Domains

- `domain.customer` — owns the journey spine
- `domain.identity_access` — performs verification/proofing execution where required
- `domain.risk_fraud` — owns risk and compliance decisions that influence onboarding readiness
- `domain.channels_experience` — owns presentation and entry point interactions
- `domain.orchestration_control` — may execute business workflow under the journey

## 9. Related Capabilities

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

This journey is the clearest representation of `vs.customer_onboarding` and trusted relationship establishment before product-specific execution.

## 11. Supporting Process Candidates

- `proc.customer_onboarding_v1`
- `proc.application_submission`
- `proc.onboarding_case_initiation`
- `proc.pre_create_match_screening`
- `proc.canonical_customer_creation`
- `proc.cip_cdd_evidence_collection`
- `proc.onboarding_completion`

## 12. Key Risks and Controls Encountered

### Key risks
- New customer is created prematurely
- Due diligence requirements are incomplete
- Customer evidence is fragmented across channels
- Customer status is advanced before prerequisites are satisfied

### Key controls
- Customer-creation preconditions
- Match/manual review controls
- Evidence requirement matrix
- Disclosure/consent capture controls
- Onboarding milestone completion rules
- Lifecycle transition approval controls

## 13. Typical Systems and Providers Involved

### Typical systems
- Onboarding platform
- Customer master
- Party resolution service
- Evidence/document repository
- KYC coordination workbench
- Status dashboard

### Representative providers
- Alloy
- Fenergo
- Salesforce
- Pega
- Appian
- Reltio

## 14. Open Questions

- Should a bank that always binds onboarding to product opening still model this as a distinct journey?
- Where should digital credentials creation appear if onboarding completion immediately triggers access setup?
- Should business and consumer onboarding be separate journeys at this layer?

## 15. Provenance

This journey is the purest Customer-domain journey because it establishes the customer relationship itself rather than the downstream product contract.
