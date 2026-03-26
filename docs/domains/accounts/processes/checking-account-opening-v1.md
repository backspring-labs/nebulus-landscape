---
id: proc.checking_account_opening_v1
type: process
name: Checking Account Opening v1
journey_id: journey.open_checking_account
capability_ids:
  - cap.account_opening
  - cap.account_application_and_contract_formation
  - cap.account_party_role_management
  - cap.account_terms_and_feature_management
  - cap.account_funding_readiness_management
  - cap.account_status_and_restriction_management
  - cap.account_overdraft_and_protection_management
  - cap.account_interest_and_fee_configuration_management
  - cap.deposit_account_product_management
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Checking Account Opening v1

## 1. Objective

Operationally execute the Accounts-domain sequence required to select a checking product, form the account contract, configure terms and features, set up overdraft protection, manage funding readiness, and activate the account for transactional use — once the Customer domain has established the customer relationship and completed due diligence gating.

## 2. Scope

This process covers:
- checking product selection and eligibility determination
- account application intake and contract formation
- party role assignment to the account
- terms, features, interest, and fee configuration
- overdraft protection election and setup
- funding readiness management and initial funding coordination
- account status activation and completion

This process does **not** own:
- customer relationship establishment or onboarding orchestration (`domain.customer`)
- identity proofing or due diligence execution (`domain.identity_access`, `domain.risk_fraud`)
- payment execution for initial funding (`domain.payments`)
- GL posting or balance computation (`domain.ledger_core`)
- channel presentation logic (`domain.channels_experience`)

## 3. Trigger

The Customer domain hands off an approved, verified customer context for checking account creation, or a known existing customer initiates a new checking account opening through a supported channel.

## 4. Entry Conditions

- A verified customer context exists (new-to-bank establishment complete, or existing customer authenticated).
- The selected checking product is available and the customer meets preliminary eligibility criteria.
- Product catalog, terms, and disclosure packages are current.
- Account-opening pathway is enabled for the selected channel.
- Customer due diligence gating is satisfied or waived per policy.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.checking_account_opening.product_selection_and_eligibility` | Product Selection and Eligibility | `domain.accounts` |
| `stage.checking_account_opening.application_and_contract_formation` | Application and Contract Formation | `domain.accounts` with `domain.customer` participation |
| `stage.checking_account_opening.party_role_establishment` | Party Role Establishment | `domain.accounts` |
| `stage.checking_account_opening.terms_and_feature_configuration` | Terms and Feature Configuration | `domain.accounts` |
| `stage.checking_account_opening.overdraft_protection_setup` | Overdraft Protection Setup | `domain.accounts` |
| `stage.checking_account_opening.funding_readiness_and_initial_funding` | Funding Readiness and Initial Funding | `domain.accounts` with `domain.payments` participation |
| `stage.checking_account_opening.account_activation_and_completion` | Account Activation and Completion | `domain.accounts` with `domain.customer` participation |

## 6. Stage-by-Stage Flow

### Stage: `stage.checking_account_opening.product_selection_and_eligibility`

**Description:** The institution evaluates the customer's checking product selection against the product catalog and eligibility rules to confirm the product is available and the customer qualifies.

**Capabilities activated:** `cap.deposit_account_product_management`, `cap.account_opening`

**Systems involved:** `sys.core_account_opening_platform`, `sys.product_configuration_engine`

**Decision points:**
- Is the selected checking product active and available in the customer's jurisdiction and channel?
- Does the customer meet the product's eligibility criteria (age, residency, relationship type)?
- Are there any product-level restrictions or capacity limits?

**Risks:** `risk.ineligible_product_selection`, `risk.product_catalog_staleness`

**Controls:** `ctrl.product_eligibility_validation`, `ctrl.product_catalog_currency_check`

**Evidence generated:** Product selection record, eligibility determination outcome, product version and channel attribution

**Handoff:** If eligible, proceed to application and contract formation. If ineligible, return to product selection or terminate.

### Stage: `stage.checking_account_opening.application_and_contract_formation`

**Description:** The institution assembles the checking account application, captures product-specific contractual elements, delivers required disclosures, and records consents and attestations.

**Capabilities activated:** `cap.account_application_and_contract_formation`, `cap.account_opening`

**Systems involved:** `sys.core_account_opening_platform`, `sys.disclosure_presentment_service`, `sys.consent_registry`

**Decision points:**
- Is the application payload complete and valid?
- Were all required checking-product disclosures presented in the correct version?
- Were required attestations and consents captured?

**Risks:** `risk.incomplete_contract_formation`, `risk.disclosure_not_delivered_or_not_provable`, `risk.invalid_or_missing_consent`

**Controls:** `ctrl.contract_completeness_gate`, `ctrl.disclosure_version_control`, `ctrl.consent_evidence_and_audit_trail`

**Evidence generated:** Account application record, contract formation record, disclosure acceptance records, consent records with timestamp/channel/purpose

**Handoff:** If contract formation is complete, proceed to party role establishment. If incomplete, hold in pending state or return to applicant.

### Stage: `stage.checking_account_opening.party_role_establishment`

**Description:** The institution assigns party roles to the checking account — primary owner, joint owner, authorized signer, beneficiary, or other roles as applicable.

**Capabilities activated:** `cap.account_party_role_management`

**Systems involved:** `sys.core_account_opening_platform`, `sys.customer_master`

**Decision points:**
- What party roles are required for this account type?
- Are all required parties identified and verified?
- Are role assignments consistent with product rules and regulatory requirements?
- Is this a single-owner or joint account?

**Risks:** `risk.incorrect_party_role_assignment`, `risk.missing_required_party`, `risk.unauthorized_role_assignment`

**Controls:** `ctrl.party_role_authorization_validation`, `ctrl.role_assignment_product_rule_check`, `ctrl.role_assignment_audit_trail`

**Evidence generated:** Party role assignment records, role type and effective date, authorization evidence for each role assignment

**Handoff:** If party roles are established, proceed to terms and feature configuration. If roles are incomplete (e.g., joint owner verification pending), hold or route to exception handling.

### Stage: `stage.checking_account_opening.terms_and_feature_configuration`

**Description:** The institution applies the specific terms, features, interest parameters, and fee schedules to the checking account based on the selected product and any customer-specific adjustments.

**Capabilities activated:** `cap.account_terms_and_feature_management`, `cap.account_interest_and_fee_configuration_management`

**Systems involved:** `sys.product_configuration_engine`, `sys.core_account_opening_platform`

**Decision points:**
- What rate tier applies to this customer/product combination?
- What fee schedule is applicable?
- Are any promotional or exception terms being applied?
- Are transaction limits and feature flags correctly configured?

**Risks:** `risk.misconfigured_terms_or_features`, `risk.incorrect_rate_or_fee_schedule`, `risk.promotional_terms_not_properly_bounded`

**Controls:** `ctrl.terms_configuration_product_catalog_validation`, `ctrl.rate_and_fee_schedule_validation`, `ctrl.promotional_terms_expiration_control`

**Evidence generated:** Terms configuration record, applied rate tier and fee schedule, feature flag settings, promotional terms documentation if applicable

**Handoff:** If terms and features are configured, proceed to overdraft protection setup. If configuration issues exist, route to exception handling.

### Stage: `stage.checking_account_opening.overdraft_protection_setup`

**Description:** The institution captures the customer's overdraft protection election, configures the appropriate overdraft settings, and links protection accounts if applicable.

**Capabilities activated:** `cap.account_overdraft_and_protection_management`

**Systems involved:** `sys.overdraft_management_module`, `sys.core_account_opening_platform`, `sys.disclosure_presentment_service`

**Decision points:**
- Does the customer elect overdraft protection?
- What type of overdraft protection is selected (linked account, line of credit, standard)?
- Are required Regulation E disclosures and opt-in captured?
- Is the linked protection account valid and eligible?

**Risks:** `risk.overdraft_misconfiguration`, `risk.missing_reg_e_opt_in`, `risk.invalid_protection_linkage`

**Controls:** `ctrl.overdraft_election_disclosure_control`, `ctrl.reg_e_opt_in_evidence_capture`, `ctrl.protection_linkage_validation`

**Evidence generated:** Overdraft election record, protection type and linked account reference, Regulation E disclosure and opt-in evidence, configuration confirmation

**Handoff:** If overdraft setup is complete or deferred, proceed to funding readiness. If deferred, flag for post-opening follow-up.

### Stage: `stage.checking_account_opening.funding_readiness_and_initial_funding`

**Description:** The institution validates funding prerequisites, enables the account to receive initial funding, and coordinates the initial deposit if required or elected.

**Capabilities activated:** `cap.account_funding_readiness_management`, `cap.account_status_and_restriction_management`

**Systems involved:** `sys.funding_service`, `sys.core_account_opening_platform`

**Participating domains:** `domain.payments`

**Decision points:**
- Are all funding prerequisites satisfied (contract formed, roles assigned, terms configured)?
- Is initial funding required by product policy, or optional?
- Did the funding instruction succeed?
- Can the account remain in a pending-funding state if funding is delayed?

**Risks:** `risk.premature_funding_before_prerequisites`, `risk.funding_failure`, `risk.account_status_mismatch_between_created_and_funded`

**Controls:** `ctrl.funding_precondition_gate`, `ctrl.funding_instruction_authorization`, `ctrl.funding_success_acknowledgement_and_reconciliation`

**Evidence generated:** Funding readiness confirmation, funding instruction record, payment authorization status, funding completion or failure event

**Handoff:** On successful or policy-acceptable funding status, proceed to activation and completion.

### Stage: `stage.checking_account_opening.account_activation_and_completion`

**Description:** The institution transitions the checking account to an active state and confirms completion of the account-opening process, coordinating with the Customer domain for lifecycle state updates.

**Capabilities activated:** `cap.account_status_and_restriction_management`, `cap.account_opening`

**Systems involved:** `sys.core_account_opening_platform`, `sys.lifecycle_rules_engine`, `sys.customer_master`

**Participating domains:** `domain.customer`

**Decision points:**
- Are all prior stages completed or appropriately waived?
- Is the account eligible for active status?
- Should any post-open tasks or restrictions remain?
- Does the Customer domain need to update the customer lifecycle state?

**Risks:** `risk.activation_before_controls_satisfied`, `risk.premature_completion`, `risk.customer_lifecycle_state_mismatch`

**Controls:** `ctrl.activation_dependency_matrix`, `ctrl.status_transition_validation`, `ctrl.completion_checklist`

**Evidence generated:** Account activation record, status transition event, completion marker, customer lifecycle update reference

**Handoff:** Process complete; the account enters servicing, transactional use, and ongoing maintenance context.

## 7. Decision Points

Key decisions across the process:
- Product eligible vs ineligible
- Contract formation complete vs incomplete
- Party roles sufficient vs insufficient
- Terms and features valid vs misconfigured
- Overdraft elected vs deferred vs declined
- Funding successful vs pending vs failed
- Activation eligible vs held/restricted

## 8. Alternate Outcomes

- Existing customer opens an additional checking account
- Joint account opening with multiple party role assignments
- Overdraft protection deferred to post-opening
- Account opened without immediate funding per product policy
- Promotional or exception terms applied
- Application saved and resumed from incomplete state

## 9. Risks (process-level)

- Product opened for ineligible customer
- Contract formation incomplete or incorrectly assembled
- Party roles misassigned or incomplete for joint accounts
- Terms, rates, or fees misconfigured against product catalog
- Overdraft protection misconfigured or missing required disclosures
- Funding attempted before all prerequisites are met
- Account activated before all control gates are satisfied
- Cross-domain state desynchronization between Accounts and Customer lifecycle

## 10. Controls (process-level)

- Product eligibility validation against catalog and customer attributes
- Contract completeness and disclosure evidence controls
- Party role authorization and product-rule validation
- Terms configuration validated against product catalog
- Overdraft election disclosure and Regulation E controls
- Funding precondition gating
- Activation dependency matrix enforcing stage completion
- Cross-domain lifecycle state reconciliation

## 11. Evidence / Audit Considerations

This process generates a dense audit trail:
- product selection and eligibility determination
- contract formation and disclosure evidence
- party role assignment and authorization records
- terms, rate, and fee configuration records
- overdraft election and Regulation E evidence
- funding readiness and initial funding events
- account activation and status transition records
- completion marker and lifecycle coordination events

## 12. Systems Involved (summary)

- core account-opening platform
- product configuration engine
- disclosure/e-sign services
- consent registry
- customer master / CIF
- overdraft management module
- funding/payment services
- lifecycle/state rules engine

## 13. Provider Participation

Representative providers often adjacent to this process:
- Fiserv, Jack Henry, FIS, Temenos, Thought Machine, Mambu, Finxact, Q2, Alkami, Narmi

## 14. Open Questions

- How much of the contract formation stage should be shared with the savings account opening process versus kept checking-specific?
- Should overdraft protection setup be modeled as a separate post-opening process when deferred?
- How should joint-account party role establishment interact with the Customer domain's party resolution process?
- At what point should digital access provisioning be represented — during this process or in a separate Identity & Access process?
- Should promotional and exception terms be governed by a separate approval workflow within this process?

## 15. Provenance

This process represents the Accounts-domain operational spine for checking account opening. It assumes the Customer domain has already established the customer relationship and completed due diligence gating. It exercises the full set of Accounts-domain capabilities specific to checking products — contract formation, terms and feature configuration, overdraft protection, funding readiness, and activation — and demonstrates the Accounts domain's ownership boundary for deposit product opening distinct from the Customer domain's onboarding orchestration process.
