---
id: proc.account_closure_v1
type: process
name: Account Closure v1
journey_id: journey.close_account
capability_ids:
  - cap.account_closure_management
  - cap.account_status_and_restriction_management
  - cap.account_hold_and_constraint_management
  - cap.account_interest_and_fee_configuration_management
  - cap.account_record_retention_and_archival
  - cap.account_party_role_management
  - cap.account_exception_and_remediation_management
  - cap.customer_lifecycle_state_management
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Closure v1

## 1. Objective

Operationally execute the controlled sequence required to close a deposit account by resolving outstanding obligations, clearing holds and constraints, finalizing interest and fees, disbursing remaining funds, deactivating party roles, transitioning the account to a terminal closed state, and triggering retention and archival obligations.

## 2. Scope

This process covers:
- closure request intake and eligibility assessment
- hold and constraint resolution
- final interest accrual and fee assessment
- remaining balance disbursement
- party role and authorization deactivation
- account status transition to closed
- record retention, archival, and customer lifecycle update

This process does **not** own:
- customer relationship exit orchestration (`domain.customer`)
- payment execution for disbursement (`domain.payments`)
- GL posting for final interest and fee entries (`domain.ledger_core`)
- fraud or risk decisioning that may trigger closure (`domain.risk_fraud`)
- channel presentation of closure flows (`domain.channels_experience`)

## 3. Trigger

A customer requests account closure through a supported channel, or the institution initiates closure for cause (risk decision, prolonged dormancy, regulatory action, or escheatment progression).

## 4. Entry Conditions

- The account exists and is in a state eligible for closure (active, dormant, or restricted — not already closed or in a terminal state).
- The requesting party has authority to close the account.
- Closure policies, fee schedules, and retention rules are available.
- Disbursement mechanisms are available if a remaining balance exists.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.account_closure.closure_request_and_eligibility` | Closure Request and Eligibility | `domain.accounts` |
| `stage.account_closure.hold_and_constraint_resolution` | Hold and Constraint Resolution | `domain.accounts` |
| `stage.account_closure.interest_accrual_and_fee_finalization` | Interest Accrual and Fee Finalization | `domain.accounts` with `domain.ledger_core` participation |
| `stage.account_closure.balance_disbursement` | Balance Disbursement | `domain.accounts` with `domain.payments` participation |
| `stage.account_closure.party_role_deactivation` | Party Role Deactivation | `domain.accounts` |
| `stage.account_closure.status_transition_to_closed` | Status Transition to Closed | `domain.accounts` |
| `stage.account_closure.retention_archival_and_lifecycle_update` | Retention, Archival, and Lifecycle Update | `domain.accounts` with `domain.customer` participation |

## 6. Stage-by-Stage Flow

### Stage: `stage.account_closure.closure_request_and_eligibility`

**Description:** The closure request is received and the account is evaluated for closure eligibility, identifying any blocking conditions or outstanding obligations.

**Capabilities activated:** `cap.account_closure_management`, `cap.account_status_and_restriction_management`

**Systems involved:** `sys.core_banking_platform`, `sys.case_management_system`

**Decision points:**
- Is the account in a state eligible for closure?
- Does the requesting party have closure authority?
- Are there pending transactions, outstanding obligations, or blocking conditions?
- Is this a voluntary customer closure or an institution-initiated closure?

**Risks:** `risk.unauthorized_closure_request`, `risk.closure_of_account_with_active_obligations`, `risk.incorrect_closure_reason_code`

**Controls:** `ctrl.closure_authority_verification`, `ctrl.closure_eligibility_status_check`, `ctrl.closure_reason_code_requirements`

**Evidence generated:** Closure request record, eligibility assessment outcome, requesting party identity and authority verification, closure reason and channel attribution

**Handoff:** If eligible, proceed to hold and constraint resolution. If blocked, route to exception handling or hold.

### Stage: `stage.account_closure.hold_and_constraint_resolution`

**Description:** All holds, liens, garnishments, and constraints on the account are reviewed and resolved or cleared before closure can proceed.

**Capabilities activated:** `cap.account_hold_and_constraint_management`, `cap.account_exception_and_remediation_management`

**Systems involved:** `sys.hold_and_lien_management_system`, `sys.core_banking_platform`, `sys.case_management_system`

**Decision points:**
- Are there active holds that must be resolved before closure?
- Are legal or regulatory constraints present that block closure?
- Can holds be released, or must they be resolved through external processes?
- Are there exceptions requiring manual remediation?

**Risks:** `risk.closure_with_unresolved_holds`, `risk.legal_hold_override`, `risk.unresolved_garnishment_or_levy`

**Controls:** `ctrl.hold_and_constraint_clearance_gate`, `ctrl.legal_hold_override_prevention`, `ctrl.exception_remediation_workflow`

**Evidence generated:** Hold inventory and resolution status, constraint clearance records, exception remediation records, hold release authorization

**Handoff:** If all holds and constraints are resolved or cleared, proceed to interest and fee finalization. If unresolvable, route to exception handling or block closure.

### Stage: `stage.account_closure.interest_accrual_and_fee_finalization`

**Description:** Accrued interest is calculated through the closure date and any final fees or early-termination charges are assessed per product terms.

**Capabilities activated:** `cap.account_interest_and_fee_configuration_management`

**Systems involved:** `sys.core_banking_platform`

**Participating domains:** `domain.ledger_core`

**Decision points:**
- What is the final interest accrual through the closure date?
- Are there closing fees or early-termination charges per product terms?
- Is the final balance positive, zero, or negative after interest and fees?

**Risks:** `risk.incorrect_final_interest_calculation`, `risk.missed_closing_fee`, `risk.negative_balance_after_finalization`

**Controls:** `ctrl.final_interest_calculation_validation`, `ctrl.fee_schedule_application_check`, `ctrl.final_balance_reconciliation`

**Evidence generated:** Final interest accrual record, closing fee assessment record, finalized account balance, calculation audit trail

**Handoff:** If balance is finalized, proceed to disbursement. If negative balance, route to collection or charge-off coordination.

### Stage: `stage.account_closure.balance_disbursement`

**Description:** The remaining balance is disbursed to the customer through an approved method (transfer, check, cash, or other approved channel).

**Capabilities activated:** `cap.account_closure_management`

**Systems involved:** `sys.payment_disbursement_service`, `sys.core_banking_platform`

**Participating domains:** `domain.payments`

**Decision points:**
- What disbursement method is selected or required?
- Is the disbursement recipient verified?
- Did the disbursement instruction succeed?
- Is a zero-balance confirmation available?

**Risks:** `risk.disbursement_to_incorrect_party`, `risk.disbursement_method_failure`, `risk.balance_not_zeroed`

**Controls:** `ctrl.disbursement_confirmation_and_zero_balance_validation`, `ctrl.disbursement_recipient_verification`, `ctrl.disbursement_method_authorization`

**Evidence generated:** Disbursement instruction record, payment authorization and completion status, zero-balance confirmation, disbursement method and recipient documentation

**Handoff:** On successful disbursement and zero-balance confirmation, proceed to party role deactivation. If disbursement fails, route to exception handling.

### Stage: `stage.account_closure.party_role_deactivation`

**Description:** Account-level party roles, signatory authorities, and transactional authorizations are deactivated.

**Capabilities activated:** `cap.account_party_role_management`

**Systems involved:** `sys.core_banking_platform`, `sys.customer_master`

**Decision points:**
- What party roles are currently active on the account?
- Are all roles being deactivated, or are some being preserved for record purposes?
- Should Identity & Access be notified to revoke account-specific access?

**Risks:** `risk.party_role_not_deactivated`, `risk.access_remains_after_closure`, `risk.incomplete_role_deactivation_on_joint_accounts`

**Controls:** `ctrl.party_role_deactivation_checklist`, `ctrl.deactivation_audit_trail`, `ctrl.access_revocation_notification`

**Evidence generated:** Party role deactivation records, deactivation effective date, access revocation references

**Handoff:** If all roles are deactivated, proceed to status transition. If deactivation is incomplete, route to exception handling.

### Stage: `stage.account_closure.status_transition_to_closed`

**Description:** The account is formally transitioned to a terminal closed state, and no further transactions are permitted.

**Capabilities activated:** `cap.account_status_and_restriction_management`, `cap.account_closure_management`

**Systems involved:** `sys.core_banking_platform`

**Decision points:**
- Are all prerequisite stages completed?
- Is the account at zero balance with all holds cleared?
- Should the closure be effective immediately or future-dated?

**Risks:** `risk.premature_closure_before_settlement`, `risk.status_transition_without_prerequisites`, `risk.transaction_posted_after_closure`

**Controls:** `ctrl.closure_prerequisite_completion_check`, `ctrl.status_transition_authorization`, `ctrl.post_closure_transaction_block`

**Evidence generated:** Account status transition record, closure effective date, closure reason code, final status confirmation

**Handoff:** If closed, proceed to retention, archival, and lifecycle update.

### Stage: `stage.account_closure.retention_archival_and_lifecycle_update`

**Description:** Record retention policies are applied, archival is initiated, and the Customer domain is notified to evaluate customer lifecycle state changes if this was the last active account.

**Capabilities activated:** `cap.account_record_retention_and_archival`, `cap.customer_lifecycle_state_management`

**Systems involved:** `sys.record_retention_and_archival_platform`, `sys.customer_master`

**Participating domains:** `domain.customer`

**Decision points:**
- What retention schedule applies to this account's records based on product type and regulatory requirements?
- Are there legal holds that override standard retention and disposition?
- Was this the customer's last active account, requiring customer lifecycle evaluation?

**Risks:** `risk.incomplete_retention_or_archival`, `risk.premature_record_destruction`, `risk.customer_lifecycle_not_updated_on_last_account`

**Controls:** `ctrl.retention_policy_application_validation`, `ctrl.legal_hold_override_check`, `ctrl.customer_lifecycle_evaluation_trigger`

**Evidence generated:** Retention schedule assignment, archival initiation record, legal hold status, customer lifecycle evaluation trigger event

**Handoff:** Process complete; post-closure record access remains available under governance rules.

## 7. Decision Points

Key decisions across the process:
- Closure eligible vs blocked
- Holds resolved vs unresolvable
- Final balance positive vs zero vs negative
- Disbursement successful vs failed
- Party roles fully deactivated vs incomplete
- Closure immediate vs future-dated
- Last account closed vs other accounts remain

## 8. Alternate Outcomes

- Institution-initiated closure for cause (risk, dormancy, escheatment)
- Closure with pending items requiring a closure-pending state
- Closure with negative balance requiring collection or charge-off
- Partial closure of joint account where one party exits but account remains
- Closure deferred due to active legal holds
- Closure reversed or rescinded before reaching terminal state

## 9. Risks (process-level)

- Account closed with unresolved legal holds or liens
- Disbursement to incorrect party or via incorrect method
- Premature closure before pending transactions settle
- Loss of required records due to incomplete retention
- Unauthorized closure request
- Customer lifecycle not updated when last account closes
- Negative balance not properly collected or written off

## 10. Controls (process-level)

- Closure authority verification
- Hold and constraint clearance gate
- Pending transaction settlement check
- Disbursement confirmation and zero-balance validation
- Party role deactivation checklist
- Retention policy application validation
- Customer lifecycle evaluation trigger on last-account closure
- Post-closure transaction block

## 11. Evidence / Audit Considerations

This process generates a comprehensive audit trail:
- closure request and eligibility assessment
- hold and constraint resolution records
- final interest and fee calculation evidence
- disbursement instructions and payment confirmations
- party role deactivation records
- account status transition events
- retention schedule assignment and archival initiation
- customer lifecycle evaluation trigger

## 12. Systems Involved (summary)

- core banking platform (account management module)
- hold and lien management system
- payment/disbursement service
- record retention and archival platform
- customer master / CIF
- case management system (for exception handling)

## 13. Provider Participation

Representative providers often adjacent to this process:
- Fiserv, Jack Henry, Temenos, FIS, Iron Mountain (archival)

## 14. Open Questions

- Should institution-initiated closure and customer-initiated closure be modeled as separate process variants?
- How should closure interact with active auto-pay or recurring payment instructions linked to the account?
- What is the appropriate handling of in-flight ACH or wire transactions at the time of closure?
- Should negative-balance closure (requiring charge-off) be a separate process or a variant path within this process?
- How should partial closure of joint accounts be handled — as a closure variant or a party-role-change process?

## 15. Provenance

This process represents the Accounts-domain operational spine for deposit account closure. It exercises the full set of closure-related capabilities — hold resolution, interest and fee finalization, disbursement coordination, party role deactivation, status transition, and retention/archival. It pairs with the checking and savings account opening processes to form a complete lifecycle arc from opening through closure, and coordinates with the Customer domain for relationship-level lifecycle updates when the last active account is closed.
