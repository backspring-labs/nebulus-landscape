---
id: proc.account_opening_v1
type: process
name: Account Opening v1
journey_id: journey.open_savings_account
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
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Account Opening v1

## 1. Objective

Operationally execute the controlled sequence required to intake, verify, approve, establish, open, and optionally fund a retail savings account while preserving proper customer-record creation, due diligence gating, disclosure evidence, and downstream handoff integrity.

## 2. Scope

This process covers:
- prospect/application intake into operational execution
- onboarding case initiation and progression
- party resolution and customer establishment
- due diligence coordination and gating
- product-account creation handoff
- initial funding handoff
- customer lifecycle transition and process completion

This process does **not** own:
- underlying identity proofing execution logic (`domain.identity_access`)
- fraud or AML decisioning engines (`domain.risk_fraud`)
- product/account ledger semantics (`domain.accounts`)
- payment execution rails (`domain.payments`)
- channel presentation logic (`domain.channels_experience`)

## 3. Trigger

A customer submits or advances a savings-account opening request through a supported channel.

## 4. Entry Conditions

- Application entry point is active and product eligible.
- Required onboarding policy set is available.
- Intake schema and disclosure packages are current.
- Identity/risk integrations are available or operationally substitutable.
- Product-opening pathway is enabled for the selected channel.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.account_opening.intake_and_case_start` | Intake and Case Start | `domain.customer` |
| `stage.account_opening.disclosure_and_consent_capture` | Disclosure and Consent Capture | `domain.customer` |
| `stage.account_opening.party_resolution_and_customer_establishment` | Party Resolution and Customer Establishment | `domain.customer` |
| `stage.account_opening.identity_and_due_diligence_gating` | Identity and Due Diligence Gating | `domain.customer` with `domain.identity_access` and `domain.risk_fraud` participation |
| `stage.account_opening.account_creation_handoff` | Account Creation Handoff | transition from `domain.customer` to `domain.accounts` |
| `stage.account_opening.initial_funding` | Initial Funding | `domain.payments` with `domain.accounts` participation |
| `stage.account_opening.lifecycle_activation_and_completion` | Lifecycle Activation and Completion | `domain.customer` |

## 6. Stage-by-Stage Flow

### Stage: `stage.account_opening.intake_and_case_start`

**Description:** The institution receives the application, validates basic completeness, and opens the operational onboarding/account-opening case.

**Capabilities activated:** `cap.customer_application_intake`, `cap.customer_onboarding_orchestration`

**Systems involved:** `sys.application_origination_platform`, `sys.digital_onboarding_platform`, `sys.branch_intake_workbench`, `sys.onboarding_journey_manager`

**Decision points:**
- Is the intake payload complete enough to continue?
- Is the application new, resumed, or assisted-channel continuation?
- Does the selected product/channel combination qualify for this process?

**Risks:** `risk.incomplete_application_data`, `risk.cross_channel_rule_inconsistency`

**Controls:** `ctrl.canonical_application_schema`, `ctrl.required_field_validation`, `ctrl.cross_field_validation`, `ctrl.standard_onboarding_state_model`

**Evidence generated:** Application submission record, onboarding case ID, channel/source attribution, intake timestamp and initial validation status

**Handoff:** If intake is complete enough, route to disclosure/consent capture. If not, hold in pending-item or error state.

### Stage: `stage.account_opening.disclosure_and_consent_capture`

**Description:** Required legal disclosures, attestations, and permissions are presented and captured before customer establishment proceeds.

**Capabilities activated:** `cap.customer_disclosure_attestation_management`, `cap.customer_consent_management`, `cap.customer_application_intake`

**Systems involved:** `sys.disclosure_presentment_service`, `sys.esignature_platform`, `sys.consent_registry`, `sys.evidence_store`

**Decision points:**
- Were all required disclosures presented in the right version?
- Were required attestations completed?
- Were mandatory permissions captured successfully?

**Risks:** `risk.disclosure_not_delivered_or_not_provable`, `risk.invalid_or_missing_consent`, `risk.version_mismatch`

**Controls:** `ctrl.disclosure_version_control`, `ctrl.versioned_attestation_capture`, `ctrl.consent_evidence_and_audit_trail`, `ctrl.non_repudiation_and_audit_controls`

**Evidence generated:** Disclosure version acceptance records, consent records with timestamp/channel/purpose, attestation records, e-sign evidence where applicable

**Handoff:** If evidence is complete, proceed to party resolution/customer establishment. If incomplete, return to pending-item path or terminate.

### Stage: `stage.account_opening.party_resolution_and_customer_establishment`

**Description:** The institution determines whether the applicant already exists, resolves duplicates, and either establishes a new canonical customer identity or links to an existing customer.

**Capabilities activated:** `cap.customer_party_resolution`, `cap.customer_identity_establishment`, `cap.customer_profile_management`, `cap.customer_contact_management`

**Systems involved:** `sys.party_match_service`, `sys.customer_search_api`, `sys.customer_master`, `sys.party_master`, `sys.data_stewardship_workbench`

**Decision points:**
- Is this party already known?
- Is the match ambiguous and does it require manual review?
- Can a new customer record be created yet?

**Risks:** `risk.missed_true_match`, `risk.bad_merge`, `risk.premature_customer_record_creation`, `risk.duplicate_canonical_customer`

**Controls:** `ctrl.match_threshold_governance`, `ctrl.ambiguous_match_manual_review`, `ctrl.customer_creation_preconditions`, `ctrl.durable_identifier_policy`

**Evidence generated:** Match decision outcome, new-customer or existing-customer determination, canonical customer ID assignment, steward review record if manual intervention occurred

**Handoff:** If party resolution and customer establishment are complete, move to verification/due diligence gating. If ambiguous or blocked, route to manual review/remediation.

### Stage: `stage.account_opening.identity_and_due_diligence_gating`

**Description:** The process coordinates identity proofing, verification outcomes, and due diligence evidence while enforcing the rule that downstream activation cannot proceed until prerequisite gates are satisfied.

**Capabilities activated:** `cap.customer_due_diligence_coordination`, `cap.customer_onboarding_orchestration`, `cap.customer_lifecycle_state_management`

**Systems involved:** `sys.kyc_intake_workbench`, `sys.due_diligence_requirement_engine`, `sys.evidence_repository`, `sys.identity_service`, `sys.onboarding_status_dashboard`

**Participating domains:** `domain.identity_access`, `domain.risk_fraud`

**Decision points:**
- Are identity proofing results acceptable?
- Are all due diligence requirements complete?
- Does policy permit advancement, manual review, or denial?

**Risks:** `risk.missing_due_diligence_requirement`, `risk.unmapped_evidence`, `risk.edd_not_escalated`, `risk.prerequisite_bypass`

**Controls:** `ctrl.due_diligence_requirement_matrix`, `ctrl.requirement_evidence_mapping`, `ctrl.dd_completion_gate`, `ctrl.completion_dependency_matrix`, `ctrl.edd_escalation_control`

**Evidence generated:** Due diligence completion status, identity and verification result references, manual review flags, gating decision record

**Handoff:** If gated requirements are satisfied, send an approved handoff to Accounts. If not, remain pending, escalate, or decline.

### Stage: `stage.account_opening.account_creation_handoff`

**Description:** Customer-domain onboarding hands off a vetted, approved customer context to `domain.accounts` for product/account creation.

**Capabilities activated:** `cap.customer_onboarding_orchestration`, `cap.customer_lifecycle_state_management`

**Systems involved:** `sys.onboarding_journey_manager`, `sys.customer_master`, `sys.core_account_opening_platform`, `sys.ops_workbench`

**Decision points:**
- Is the handoff package complete?
- Has the customer reached an eligible lifecycle state for account creation?
- Did Accounts accept or reject the handoff?

**Risks:** Incomplete customer context at handoff, ownership confusion between customer establishment and account contract creation, cross-domain state desynchronization

**Controls:** Handoff payload completeness rules, lifecycle-state eligibility control, accepted/rejected handoff acknowledgement, cross-domain state audit

**Evidence generated:** Handoff event record, account-opening request package, acceptance/rejection response from Accounts

**Handoff:** If Accounts accepts, move to initial funding or activation. If rejected, route back to exception handling.

### Stage: `stage.account_opening.initial_funding`

**Description:** If product policy requires or allows it, initial funding is executed or scheduled.

**Participating domains:** `domain.accounts`, `domain.payments`

**Systems involved:** `sys.funding_service`, `sys.payment_execution_service`, `sys.core_account_opening_platform`

**Decision points:**
- Is initial funding required?
- Did payment authorization and transfer succeed?
- Can the account remain pending if funding is delayed?

**Risks:** Funding attempted before prerequisites are met, payment failure, account status mismatch between created and funded states

**Controls:** Funding precondition control, payment initiation authorization checks, funding success acknowledgement and reconciliation

**Evidence generated:** Funding instruction, payment authorization status, funding completion/failure event

**Handoff:** On successful or policy-acceptable funding status, move to lifecycle activation and completion.

### Stage: `stage.account_opening.lifecycle_activation_and_completion`

**Description:** The process closes the onboarding/account-opening path by transitioning the customer to the proper lifecycle state and marking process completion.

**Capabilities activated:** `cap.customer_lifecycle_state_management`, `cap.customer_onboarding_orchestration`

**Systems involved:** `sys.customer_master`, `sys.lifecycle_rules_engine`, `sys.status_dashboard`, `sys.onboarding_journey_manager`

**Decision points:**
- Is the customer eligible to move to active/established?
- Are all prior stages completed or appropriately waived?
- Should any post-open tasks remain pending?

**Risks:** Inaccurate lifecycle state, account open while customer remains in applicant status, completion marked prematurely

**Controls:** `ctrl.allowed_transition_matrix`, `ctrl.lifecycle_reason_code_requirements`, `ctrl.state_change_audit_trail`, completion dependency matrix

**Evidence generated:** Lifecycle transition record, process completion marker, final onboarding status package

**Handoff:** Process complete; customer moves into servicing, relationship management, and ongoing maintenance context.

## 7. Decision Points

Key decisions across the process:
- Intake sufficient vs insufficient
- Disclosure/consent/attestation complete vs incomplete
- Existing-customer match vs new-customer creation vs manual review
- Due diligence satisfied vs pending/escalated/declined
- Account-creation handoff accepted vs rejected
- Funding successful vs pending/failed
- Lifecycle transition to active vs held/restricted

## 8. Alternate Outcomes

- Existing-customer add-on instead of new-to-bank opening
- Application saved and resumed
- Manual review path before customer creation
- Approved account without immediate funding
- Pending onboarding due to unresolved evidence
- Decline or withdrawal before account opening

## 9. Risks (process-level)

- Cross-domain ownership confusion between Customer and Accounts
- Premature customer or account activation
- Incomplete evidence package
- Duplicate or fragmented customer records
- Policy-bypass during manual intervention
- Cross-system state desynchronization

## 10. Controls (process-level)

- Canonical intake schema and validation
- Disclosure and consent evidence controls
- Duplicate prevention and customer-creation preconditions
- Due diligence gating controls
- Lifecycle transition controls
- Cross-domain handoff acknowledgement and reconciliation

## 11. Evidence / Audit Considerations

This process generates a high-density audit trail:
- intake submission and validation events
- disclosure, consent, and attestation evidence
- match and customer-establishment decisions
- verification and due diligence references
- account-creation handoff records
- funding status references
- lifecycle activation records

## 12. Systems Involved (summary)

- onboarding platform
- application origination platform
- disclosure/e-sign services
- consent registry
- customer master / CIF
- party resolution services
- KYC/evidence coordination tools
- core account-opening platform
- funding/payment services
- lifecycle/state dashboard

## 13. Provider Participation

Representative providers often adjacent to this process:
- Alloy, Socure, LexisNexis Risk Solutions, Trulioo, MeridianLink, nCino, Temenos, Fiserv

## 14. Open Questions

- Should the account-creation handoff be modeled as a distinct shared process between Customer and Accounts?
- How much of payment funding should remain here versus in a Payments-owned process artifact?
- Should approval and decline outcomes be modeled as separate process variants later?

## 15. Provenance

This process is the most operationally complex Customer-domain process because it combines customer establishment, cross-domain control gating, and downstream account/payment execution into a single operational spine.
