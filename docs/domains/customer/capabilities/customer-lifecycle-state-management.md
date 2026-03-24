---
id: cap.customer_lifecycle_state_management
type: capability
name: Customer Lifecycle State Management
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Customer Lifecycle State Management

## 1. Definition

Customer Lifecycle State Management is the enduring business ability to define, govern, and operationalize lifecycle states and state transitions for the customer relationship, from prospect through applicant, active, restricted, dormant, exited, and archived.

## 2. Business Outcome

Ensure the institution has a controlled, auditable state model for the customer relationship so channels, operations, servicing, compliance, and downstream product domains can act consistently based on the customer's current standing.

## 3. Scope

### Included activities
- Defining customer lifecycle states and sub-states
- Defining allowed state transitions and transition preconditions
- Recording lifecycle status, status reason, and effective date
- Managing state-triggered operational behaviors, restrictions, or routing flags
- Coordinating lifecycle changes arising from onboarding completion, inactivity, remediation, exit, or archival
- Maintaining customer-level restrictions distinct from account-level restrictions
- Tracking manual overrides and exception state changes
- Supporting lifecycle-triggered communications and review actions

### Excluded activities
- Product/account lifecycle and account closure (`domain.accounts`)
- Fraud/AML decision ownership for restricted or monitored status (`domain.risk_fraud`)
- Generic workflow engine management (`domain.orchestration_control`)
- Steady-state profile updates (`cap.customer_profile_management`)
- Detailed remediation operations (`cap.customer_data_issue_remediation`)
- Exit workflow execution after the decision to terminate (`cap.customer_exit_management`)

## 4. Upstream Dependencies
- `cap.customer_onboarding_orchestration` for onboarding progression signals
- `cap.customer_identity_establishment` for canonical customer creation
- `cap.customer_change_management` for governed state changes
- `cap.customer_data_issue_remediation` where unresolved issues block reactivation or activation
- `domain.risk_fraud` for decision outputs that may require restricted states
- `domain.accounts` for account relationship status that may influence customer state

## 5. Downstream Dependencies
- `cap.customer_exit_management`
- `cap.customer_record_retention_and_archival`
- `domain.servicing` for treatment and access routing
- `domain.channels_experience` for experience gating
- `domain.data_rewards` for segmentation and lifecycle analytics
- `domain.identity_access` where access provisioning or de-provisioning follows lifecycle state

## 6. Related Value Streams
- `vs.customer_lifecycle_governance`
- `vs.customer_record_maintenance`
- `vs.operational_control`
- `vs.relationship_management_and_service`

## 7. Related Journeys
- `journey.convert_prospect_to_customer`
- `journey.customer_onboarding`
- `journey.reactivate_dormant_customer`
- `journey.restrict_customer_relationship`
- `journey.exit_customer_relationship`

## 8. Likely Processes
- `proc.lifecycle_state_definition_and_governance`
- `proc.lifecycle_state_transition_management`
- `proc.customer_restriction_application`
- `proc.dormancy_and_reactivation_review`
- `proc.archival_eligibility_marking`

## 9. Risks
- Lifecycle status is inaccurate or stale
- Invalid state transitions bypass required controls
- Customer is treated as active when restricted or exited
- State changes are made manually without evidence
- Different systems hold conflicting lifecycle states

## 10. Controls
- `ctrl.customer_state_model_governance`
- `ctrl.allowed_transition_matrix`
- `ctrl.state_change_authorization`
- `ctrl.lifecycle_reason_code_requirements`
- `ctrl.cross_system_state_reconciliation`
- `ctrl.state_change_audit_trail`

## 11. Typical Systems/Providers

### Typical systems
- Customer master
- Lifecycle/rules engine
- CRM platform
- Operations workbench
- Restriction and status dashboard

### Typical providers
- Salesforce
- Pega
- Appian
- ServiceNow
- Fiserv

## 12. Open Questions
- Should "restricted" be modeled as a state, a state overlay, or both?
- How much lifecycle-state logic should sit in the customer master versus orchestration layers?
- When account relationships diverge, what is the exact precedence between account status and customer status?

## 13. Provenance

This capability formalizes the lifecycle-state recommendation already surfaced in the Customer domain research: lifecycle state should remain a first-class capability rather than being buried inside profile management.
