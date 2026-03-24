---
id: proc.customer_onboarding_v1
type: process
name: Customer Onboarding v1
journey_id: journey.customer_onboarding
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
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Customer Onboarding v1

## 1. Objective

Operationally establish a new-to-bank individual as a governed customer of the institution, independent of downstream product creation.

## 2. Scope

This process covers:
- onboarding case initiation
- core record capture
- disclosure/consent evidence
- party resolution
- customer identity creation
- due diligence gating
- lifecycle transition to established

It does not cover:
- product/account opening
- payment funding
- long-term customer maintenance after establishment

## 3. Trigger

A prospect or applicant enters a customer-establishment path.

## 4. Entry Conditions

- Customer-establishment path is supported.
- Intake and evidence requirements are current.
- Verification and due diligence services are available.
- No blocking policy condition exists at entry.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.customer_onboarding.case_start` | Case Start | `domain.customer` |
| `stage.customer_onboarding.record_and_evidence_capture` | Record and Evidence Capture | `domain.customer` |
| `stage.customer_onboarding.party_resolution` | Party Resolution | `domain.customer` |
| `stage.customer_onboarding.due_diligence_gating` | Due Diligence Gating | `domain.customer` with `domain.identity_access` and `domain.risk_fraud` participation |
| `stage.customer_onboarding.customer_establishment_and_activation` | Customer Establishment and Activation | `domain.customer` |

## 6. Stage-by-Stage Flow

### Stage: `stage.customer_onboarding.case_start`

**Description:** Open the onboarding case and initialize required workflow state.

**Capabilities activated:** `cap.customer_onboarding_orchestration`

**Systems involved:** `sys.onboarding_journey_manager`, `sys.ops_workbench`

**Decision points:** Is this new, resumed, or referred? Is the applicant eligible to continue?

**Risks:** Case opened with wrong policy context, incomplete workflow initialization

**Controls:** Onboarding state model initialization, source/channel attribution capture

**Evidence generated:** Onboarding case ID, journey start event

**Handoff:** Route to record/evidence capture

### Stage: `stage.customer_onboarding.record_and_evidence_capture`

**Description:** Collect core customer, contact, disclosure, and consent information needed to establish the customer relationship.

**Capabilities activated:** `cap.customer_application_intake`, `cap.customer_profile_management`, `cap.customer_contact_management`, `cap.customer_consent_management`, `cap.customer_disclosure_attestation_management`

**Systems involved:** `sys.application_origination_platform`, `sys.customer_master`, `sys.disclosure_presentment_service`, `sys.consent_registry`, `sys.evidence_store`

**Decision points:** Are all minimum record attributes present? Are required disclosures/attestations complete? Are any data issues obvious at intake?

**Risks:** Incomplete record capture, missing evidence, invalid contact or profile fields

**Controls:** Validation and standardization rules, disclosure version controls, consent evidence capture controls, required-field validation

**Evidence generated:** Profile/contact intake record, disclosure evidence, consent records, attestation capture

**Handoff:** Move to party resolution

### Stage: `stage.customer_onboarding.party_resolution`

**Description:** Determine whether the applicant already exists and whether a new customer record should be created.

**Capabilities activated:** `cap.customer_party_resolution`, `cap.customer_identity_establishment`

**Systems involved:** `sys.party_match_service`, `sys.customer_search_api`, `sys.customer_master`, `sys.data_stewardship_workbench`

**Decision points:** Existing customer vs new customer, clear match vs ambiguous match, ready for record creation vs manual review

**Risks:** Duplicate customer creation, missed existing customer, bad merge or bad linkage

**Controls:** Match threshold governance, ambiguous match manual review, customer creation preconditions

**Evidence generated:** Resolution decision, steward review if applicable, customer ID assignment if created

**Handoff:** Move to due diligence gating

### Stage: `stage.customer_onboarding.due_diligence_gating`

**Description:** Coordinate identity-related and compliance-related evidence requirements before the institution recognizes onboarding completion.

**Capabilities activated:** `cap.customer_due_diligence_coordination`, `cap.customer_onboarding_orchestration`

**Systems involved:** `sys.kyc_intake_workbench`, `sys.due_diligence_requirement_engine`, `sys.evidence_repository`, `sys.identity_service`

**Decision points:** Are all required evidence items complete? Are any escalations needed? Does policy permit customer establishment?

**Risks:** Due diligence incompleteness, unmapped evidence, escalation missed, customer advanced too early

**Controls:** Due diligence matrix, evidence mapping controls, completion gate, escalation controls

**Evidence generated:** Requirement completion status, evidence package references, escalation outcome where needed

**Handoff:** Move to customer establishment and activation

### Stage: `stage.customer_onboarding.customer_establishment_and_activation`

**Description:** Create or confirm the canonical customer record and transition lifecycle state to established.

**Capabilities activated:** `cap.customer_identity_establishment`, `cap.customer_lifecycle_state_management`

**Systems involved:** `sys.customer_master`, `sys.lifecycle_rules_engine`, `sys.status_dashboard`

**Decision points:** Establish now vs remain pending, lifecycle state active vs restricted/held

**Risks:** Customer activated without complete evidence, incorrect lifecycle state, inconsistent completion status

**Controls:** Customer creation preconditions, allowed transition matrix, state-change audit trail

**Evidence generated:** Customer establishment event, lifecycle transition record, onboarding complete marker

**Handoff:** Process complete; downstream product or servicing processes may now begin.

## 7. Decision Points

- Applicant eligible vs blocked
- Sufficient record capture vs pending
- Existing vs new customer
- Evidence complete vs incomplete
- Establish customer now vs hold/restrict

## 8. Alternate Outcomes

- Existing-customer recognition
- Partial onboarding resumed later
- Onboarding held pending evidence
- Onboarding escalated to manual review
- Onboarding declined or abandoned

## 9. Risks (process-level)

- Fragmented onboarding data
- Premature customer establishment
- Duplicate customer creation
- Incomplete compliance evidence
- Cross-channel inconsistency

## 10. Controls (process-level)

- Canonical onboarding state model
- Required record/evidence minimums
- Party resolution controls
- Due diligence completion gate
- Lifecycle activation controls

## 11. Evidence / Audit Considerations

The most important audit outputs are:
- onboarding case initiation
- intake/evidence package
- customer-resolution decision
- establishment event
- lifecycle transition record

## 12. Systems Involved (summary)

- onboarding journey manager
- customer master
- party resolution service
- consent/disclosure evidence systems
- due diligence coordination tools
- lifecycle/state systems

## 13. Provider Participation

Representative providers: Alloy, Fenergo, Reltio, Salesforce, Pega, Appian

## 14. Open Questions

- Should product-free customer onboarding remain a distinct canonical process for institutions that always bind onboarding to product opening?
- When should restricted or pending lifecycle states be applied instead of established?
- How deeply should manual-review subflows be modeled later?

## 15. Provenance

This is the cleanest Customer-owned establishment process because it stops at governed customer creation rather than extending into product execution.
