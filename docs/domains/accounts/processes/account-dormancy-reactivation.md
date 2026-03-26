---
id: proc.account_dormancy_reactivation
type: process
name: Account Dormancy Reactivation
journey_id: journey.reactivate_dormant_account
capability_ids:
  - cap.account_dormancy_and_escheat_coordination
  - cap.account_status_and_restriction_management
  - cap.account_hold_and_constraint_management
  - cap.account_party_role_management
  - cap.account_maintenance_management
  - cap.customer_due_diligence_coordination
  - cap.customer_lifecycle_state_management
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: medium
---

# Account Dormancy Reactivation

## 1. Objective

Operationally execute the controlled sequence required to reactivate a dormant deposit account by assessing reactivation eligibility, re-verifying customer identity, reviewing and resolving dormancy-related holds and constraints, re-establishing party roles and access, transitioning the account back to active status, and updating the customer lifecycle state.

## 2. Scope

This process covers:
- reactivation request intake and eligibility assessment
- customer identity re-verification and due diligence refresh
- dormancy-related hold and constraint review
- party role and access re-establishment
- account status transition from dormant to active
- customer lifecycle state update and completion

This process does **not** own:
- identity proofing execution (`domain.identity_access`)
- fraud or AML re-assessment decisioning (`domain.risk_fraud`)
- payment execution for any reactivation-related transactions (`domain.payments`)
- channel presentation of reactivation flows (`domain.channels_experience`)
- customer relationship re-engagement campaigns (`domain.customer`)

## 3. Trigger

A customer contacts the institution to reactivate a dormant account, a customer attempts a transaction on a dormant account triggering a reactivation workflow, or the institution identifies a dormant account eligible for proactive re-engagement.

## 4. Entry Conditions

- The account exists and is in a dormant or inactive state (not closed or escheated).
- The requesting party can be identified and authenticated.
- Reactivation policies and re-verification requirements are available.
- The account has not progressed to an escheatment terminal state.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.dormancy_reactivation.reactivation_request_and_eligibility` | Reactivation Request and Eligibility | `domain.accounts` |
| `stage.dormancy_reactivation.customer_reverification` | Customer Re-verification | `domain.accounts` with `domain.identity_access` and `domain.risk_fraud` participation |
| `stage.dormancy_reactivation.hold_and_constraint_review` | Hold and Constraint Review | `domain.accounts` |
| `stage.dormancy_reactivation.party_role_and_access_reestablishment` | Party Role and Access Re-establishment | `domain.accounts` with `domain.identity_access` participation |
| `stage.dormancy_reactivation.status_transition_to_active` | Status Transition to Active | `domain.accounts` |
| `stage.dormancy_reactivation.customer_lifecycle_update_and_completion` | Customer Lifecycle Update and Completion | `domain.accounts` with `domain.customer` participation |

## 6. Stage-by-Stage Flow

### Stage: `stage.dormancy_reactivation.reactivation_request_and_eligibility`

**Description:** The reactivation request is received and the account is evaluated for reactivation eligibility, including dormancy state confirmation and escheatment progression check.

**Capabilities activated:** `cap.account_dormancy_and_escheat_coordination`, `cap.account_status_and_restriction_management`

**Systems involved:** `sys.core_banking_platform`, `sys.dormancy_management_module`

**Decision points:**
- Is the account confirmed as dormant (not closed, escheated, or in a terminal state)?
- Has the account progressed too far toward escheatment to allow reactivation?
- Does the requesting party have a relationship to the account?
- Is the account's product still offered, or does reactivation require conversion?

**Risks:** `risk.reactivation_of_escheatment_eligible_account`, `risk.reactivation_request_for_terminal_account`, `risk.discontinued_product_reactivation`

**Controls:** `ctrl.escheatment_progression_check`, `ctrl.dormancy_state_confirmation`, `ctrl.product_availability_check`

**Evidence generated:** Reactivation request record, dormancy state confirmation, escheatment progression assessment, eligibility determination outcome

**Handoff:** If eligible, proceed to customer re-verification. If ineligible (escheated, terminal, or product discontinued without conversion path), terminate or route to alternative path.

### Stage: `stage.dormancy_reactivation.customer_reverification`

**Description:** The institution re-verifies the customer's identity and refreshes due diligence as required by policy based on the duration and circumstances of dormancy.

**Capabilities activated:** `cap.customer_due_diligence_coordination`

**Systems involved:** `sys.identity_verification_service`, `sys.kyc_evidence_workbench`, `sys.customer_master`

**Participating domains:** `domain.identity_access`, `domain.risk_fraud`

**Decision points:**
- What level of re-verification is required based on dormancy duration?
- Does policy require CDD/EDD refresh given the elapsed time?
- Are re-verification outcomes acceptable?
- Did the risk assessment reveal new concerns?

**Risks:** `risk.identity_fraud_on_dormant_account`, `risk.reactivation_without_cdd_refresh`, `risk.stale_customer_information`

**Controls:** `ctrl.identity_reverification_assurance`, `ctrl.cdd_edd_refresh_requirement`, `ctrl.reverification_outcome_validation`

**Evidence generated:** Re-verification request and outcome, CDD/EDD refresh status, identity assurance level confirmation, risk assessment references

**Handoff:** If re-verification is successful, proceed to hold and constraint review. If re-verification fails or risk concerns are identified, hold or decline reactivation.

### Stage: `stage.dormancy_reactivation.hold_and_constraint_review`

**Description:** Holds, restrictions, and constraints applied during dormancy are reviewed and resolved or preserved as appropriate.

**Capabilities activated:** `cap.account_hold_and_constraint_management`, `cap.account_maintenance_management`

**Systems involved:** `sys.core_banking_platform`, `sys.dormancy_management_module`

**Decision points:**
- What holds or restrictions were applied as part of dormancy processing?
- Which dormancy-related constraints can be removed upon reactivation?
- Are there legal or regulatory holds that must be preserved regardless of reactivation?
- Are there pending escheatment-related constraints that need resolution?

**Risks:** `risk.reactivation_with_unresolved_holds`, `risk.premature_release_of_valid_hold`, `risk.escheatment_constraint_not_cleared`

**Controls:** `ctrl.hold_and_constraint_clearance_review`, `ctrl.legal_hold_preservation_check`, `ctrl.dormancy_constraint_resolution_checklist`

**Evidence generated:** Hold and constraint inventory, resolution or preservation decisions, constraint clearance records, hold release authorization where applicable

**Handoff:** If holds and constraints are resolved, proceed to party role re-establishment. If unresolvable holds exist, route to exception handling.

### Stage: `stage.dormancy_reactivation.party_role_and_access_reestablishment`

**Description:** Account-level party roles and authorizations are reviewed and re-established, and digital access credentials may be refreshed or re-provisioned.

**Capabilities activated:** `cap.account_party_role_management`

**Systems involved:** `sys.core_banking_platform`, `sys.customer_master`

**Participating domains:** `domain.identity_access`

**Decision points:**
- Are current party roles still valid and appropriate?
- Do any party roles need to be updated (e.g., deceased joint owner, changed circumstances)?
- Do digital access credentials need to be refreshed or re-provisioned?
- Are transactional authorities appropriately re-enabled?

**Risks:** `risk.unauthorized_party_access_reestablishment`, `risk.stale_party_role_information`, `risk.access_reprovisioned_for_invalid_party`

**Controls:** `ctrl.party_role_revalidation`, `ctrl.access_reprovisioning_authorization`, `ctrl.party_role_currency_check`

**Evidence generated:** Party role review and re-establishment records, role update decisions, access re-provisioning references, authorization re-enablement confirmation

**Handoff:** If party roles are re-established, proceed to status transition. If party role issues exist (e.g., deceased owner, unauthorized party), route to exception handling.

### Stage: `stage.dormancy_reactivation.status_transition_to_active`

**Description:** The account is transitioned from dormant to active status, re-enabling normal transaction processing and resetting the dormancy/escheatment clock.

**Capabilities activated:** `cap.account_status_and_restriction_management`, `cap.account_dormancy_and_escheat_coordination`

**Systems involved:** `sys.core_banking_platform`, `sys.dormancy_management_module`

**Decision points:**
- Are all prerequisite stages completed?
- Should the account be fully active or temporarily restricted pending completion of remaining items?
- Is the dormancy/escheatment clock properly reset?

**Risks:** `risk.status_transition_without_prerequisites`, `risk.escheatment_clock_not_reset`, `risk.transaction_enabled_before_controls_satisfied`

**Controls:** `ctrl.reactivation_prerequisite_completion_check`, `ctrl.status_transition_authorization`, `ctrl.escheatment_clock_reset_confirmation`

**Evidence generated:** Account status transition record, reactivation effective date, dormancy/escheatment clock reset record, reactivation reason and channel

**Handoff:** If active, proceed to customer lifecycle update and completion.

### Stage: `stage.dormancy_reactivation.customer_lifecycle_update_and_completion`

**Description:** The Customer domain is notified of the reactivation, the customer lifecycle state is updated if applicable, and reactivation completion is confirmed.

**Capabilities activated:** `cap.customer_lifecycle_state_management`

**Systems involved:** `sys.customer_master`

**Participating domains:** `domain.customer`

**Decision points:**
- Was the customer in a dormant or reduced-engagement lifecycle state?
- Should the customer lifecycle state be updated based on this reactivation?
- Are there other dormant accounts that remain unchanged?

**Risks:** `risk.customer_lifecycle_not_updated`, `risk.inconsistent_lifecycle_state_across_accounts`

**Controls:** `ctrl.customer_lifecycle_update_trigger`, `ctrl.lifecycle_state_consistency_check`

**Evidence generated:** Customer lifecycle update reference, reactivation completion record, confirmation notification reference

**Handoff:** Process complete; the account re-enters servicing, transactional use, and ongoing maintenance context.

## 7. Decision Points

Key decisions across the process:
- Reactivation eligible vs blocked (escheatment, terminal state)
- Re-verification acceptable vs failed
- CDD/EDD refresh required vs not required
- Holds resolvable vs unresolvable
- Party roles valid vs need update
- Full activation vs restricted activation
- Customer lifecycle update needed vs not needed

## 8. Alternate Outcomes

- Transaction-triggered reactivation with inline verification
- Institution-initiated proactive re-engagement
- Partial reactivation with temporary restrictions pending re-verification completion
- Reactivation requiring simultaneous product conversion (discontinued product)
- Reactivation of one account among multiple dormant accounts
- Reactivation declined due to risk concerns identified during re-verification

## 9. Risks (process-level)

- Reactivation of account that should have been escheated
- Identity fraud through impersonation of dormant account holder
- Reactivation without required CDD/EDD refresh
- Re-enabling access for unauthorized or invalid parties
- Reactivation with unresolved legal or regulatory holds
- Product no longer available for the dormant account type
- Customer lifecycle state drift after reactivation

## 10. Controls (process-level)

- Escheatment progression check before reactivation
- Identity re-verification at appropriate assurance level
- CDD/EDD refresh requirement based on dormancy duration
- Hold and constraint clearance review
- Party role and authorization re-validation
- Product availability check with conversion path if needed
- Customer lifecycle state evaluation on reactivation

## 11. Evidence / Audit Considerations

This process generates a targeted audit trail:
- reactivation request and eligibility assessment
- re-verification outcomes and CDD/EDD refresh status
- hold and constraint review and resolution records
- party role re-establishment and access re-provisioning records
- account status transition and escheatment clock reset
- customer lifecycle update reference
- completion confirmation

## 12. Systems Involved (summary)

- core banking platform (account management and dormancy module)
- dormancy management module
- identity verification service
- KYC/evidence workbench
- customer master / CIF
- case management system (for exceptions)

## 13. Provider Participation

Representative providers often adjacent to this process:
- Fiserv, Jack Henry, Temenos, FIS, Alloy, LexisNexis Risk Solutions

## 14. Open Questions

- What dormancy duration thresholds should trigger mandatory CDD/EDD refresh versus simple re-verification?
- Should reactivation of a discontinued product automatically route to the account conversion process?
- How should transaction-triggered reactivation interact with real-time transaction authorization — should it block, queue, or conditionally approve?
- At what point in the escheatment progression should reactivation be permanently blocked?
- Should reactivation carry any temporary restrictions or monitoring period?
- How should reactivation interact with anti-money-laundering controls for accounts dormant beyond certain thresholds?

## 15. Provenance

This process is medium-confidence because while the core reactivation flow is well understood, the intersection with escheatment progression, CDD refresh thresholds, and transaction-triggered reactivation introduces policy dependencies that vary significantly across institutions. It exercises `cap.account_dormancy_and_escheat_coordination` as its primary Accounts-domain capability and demonstrates the boundary between account-level dormancy management and customer-level lifecycle state management. It pairs with the account closure process as part of the non-standard lifecycle event set.
