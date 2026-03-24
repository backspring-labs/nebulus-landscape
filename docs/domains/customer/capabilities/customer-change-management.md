---
id: cap.customer_change_management
type: capability
name: Customer Change Management
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Customer Change Management

## 1. Definition

Customer Change Management is the enduring business ability to govern how customer-record changes are initiated, validated, approved, evidenced, applied, and communicated across profile, contact, preference, consent, relationship, and lifecycle data.

## 2. Business Outcome

Ensure customer data changes occur through controlled, traceable, policy-aligned mechanisms that reduce fraud exposure, prevent unauthorized alteration, and preserve a reliable history of who changed what, why, and under what authority.

## 3. Scope

### Included activities
- Defining change classes and sensitivity levels
- Governing change-request initiation, evidence, and approval paths
- Managing effective dating, future dating, and historical supersession
- Requiring step-up review or dual control for sensitive changes
- Managing documentation and evidence for change requests
- Governing propagation of approved changes to downstream systems
- Managing change reason codes and source provenance
- Coordinating customer-notification requirements tied to changes
- Supporting assisted, digital, and exception-driven update patterns

### Excluded activities
- The steady-state ownership of data domains themselves (`cap.customer_profile_management`, `cap.customer_contact_management`, `cap.customer_consent_management`, etc.)
- Operational cleanup of broken data (`cap.customer_data_issue_remediation`)
- Identity authentication and runtime access challenge execution (`domain.identity_access`)
- Fraud decisioning (`domain.risk_fraud`)
- Generic orchestration engine ownership (`domain.orchestration_control`)

## 4. Upstream Dependencies
- `cap.customer_profile_management`
- `cap.customer_contact_management`
- `cap.customer_preference_management`
- `cap.customer_consent_management`
- `cap.customer_relationship_management`
- `cap.customer_lifecycle_state_management`
- `domain.identity_access` for authentication or step-up signals where required

## 5. Downstream Dependencies
- All customer-record maintenance capabilities that consume governed change outcomes
- `cap.customer_data_issue_remediation` for failed or disputed changes
- `domain.servicing` for customer-facing resolution flows
- `domain.accounts` and `domain.payments` when customer changes must propagate downstream
- `domain.risk_fraud` when certain change events are risk-significant

## 6. Related Value Streams
- `vs.customer_record_maintenance`
- `vs.operational_control`
- `vs.customer_lifecycle_governance`
- `vs.privacy_and_permissions_governance`

## 7. Related Journeys
- `journey.update_customer_profile`
- `journey.update_contact_information`
- `journey.change_legal_name`
- `journey.revoke_data_sharing_consent`
- `journey.assign_relationship_owner`

## 8. Likely Processes
- `proc.customer_change_request_intake`
- `proc.sensitive_change_review_and_approval`
- `proc.effective_dated_change_application`
- `proc.change_propagation_and_confirmation`
- `proc.change_dispute_handling`

## 9. Risks
- Unauthorized or fraudulent changes are applied
- Sensitive changes bypass proper approval
- Downstream systems are not updated consistently
- Historical record is lost or overwritten
- Customers are not notified of material changes when they should be

## 10. Controls
- `ctrl.customer_change_taxonomy`
- `ctrl.sensitive_change_step_up_controls`
- `ctrl.maker_checker_for_high_risk_changes`
- `ctrl.effective_dating_and_history_preservation`
- `ctrl.change_propagation_reconciliation`
- `ctrl.customer_change_audit_trail`

## 11. Typical Systems/Providers

### Typical systems
- Customer master
- CRM platform
- Change-governance rules engine
- Audit/event store
- Case and exception workbench

### Typical providers
- Salesforce
- ServiceNow
- Pega
- Appian
- Informatica MDM

## 12. Open Questions
- Should customer change governance remain centralized here, or eventually specialize into separate policy layers for profile/contact/consent?
- Which changes should be governed as customer-domain changes versus domain-local overrides in downstream systems?
- How should disputed changes be partitioned between change management and remediation?

## 13. Provenance

This capability is the governance layer over the Batch 2 customer-record cluster. Batch 2 established profile, contact, preference, consent, disclosure, and relationship management as distinct data-owning or context-owning capabilities; this capability governs how those records change over time.
