---
id: cap.customer_exit_management
type: capability
name: Customer Exit Management
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: medium
---

# Customer Exit Management

## 1. Definition

Customer Exit Management is the enduring business ability to govern and execute customer-level relationship termination, closure, disengagement, or post-relationship handling once the institution determines the customer relationship should end or has effectively ended.

## 2. Business Outcome

Ensure the institution can close or terminate customer relationships in a controlled, evidence-backed way that coordinates customer treatment, downstream obligations, post-exit servicing, and archival triggers without confusing customer exit with account closure alone.

## 3. Scope

### Included activities
- Determining and recording customer-level exit status and reason
- Coordinating pre-exit checks and dependencies across accounts, servicing, compliance, and operations
- Managing voluntary and institution-initiated exit patterns
- Managing post-exit communication, servicing posture, and residual obligations
- Triggering archival, retention, and access changes after exit
- Coordinating exit where the customer has multiple products or relationships
- Managing reversals or rescissions where allowed
- Recording evidence and approvals for exit actions

### Excluded activities
- Closing or settling individual accounts (`domain.accounts`)
- Complaint, dispute, or litigation handling (`domain.servicing`)
- Fraud/AML decisioning that may motivate exit (`domain.risk_fraud`)
- Generic retention and archival execution (`cap.customer_record_retention_and_archival`)
- Identity-access deprovisioning mechanics (`domain.identity_access`)

## 4. Upstream Dependencies
- `cap.customer_lifecycle_state_management`
- `cap.customer_change_management`
- `cap.customer_record_retention_and_archival`
- `domain.accounts` for account closure/completion status
- `domain.servicing` for active cases or unresolved obligations
- `domain.risk_fraud` for restrictions or closure drivers
- `domain.identity_access` for access implications

## 5. Downstream Dependencies
- `cap.customer_record_retention_and_archival`
- `domain.servicing` for post-exit inquiries
- `domain.channels_experience` for access and messaging changes
- `domain.data_rewards` for suppression from ongoing engagement
- `domain.identity_access` where digital access must be adjusted
- Enterprise audit/compliance functions needing exit evidence

## 6. Related Value Streams
- `vs.customer_lifecycle_governance`
- `vs.relationship_management_and_service`
- `vs.records_governance`
- `vs.operational_control`

## 7. Related Journeys
- `journey.exit_customer_relationship`
- `journey.close_last_product_and_exit_customer`
- `journey.institution_initiated_customer_offboarding`
- `journey.reinstate_exited_customer`
- `journey.post_exit_record_retrieval`

## 8. Likely Processes
- `proc.customer_exit_initiation`
- `proc.pre_exit_dependency_review`
- `proc.customer_exit_approval_and_execution`
- `proc.post_exit_obligation_management`
- `proc.customer_exit_reversal`

## 9. Risks
- Customer is exited while dependent products or obligations remain unresolved
- Exit reason or evidence is inadequately recorded
- Post-exit access, communications, or servicing posture remain inconsistent
- Institution-initiated exit creates fairness or regulatory exposure
- Exit is confused with simple inactivity or dormancy

## 10. Controls
- `ctrl.customer_exit_criteria_and_reason_codes`
- `ctrl.pre_exit_dependency_checklist`
- `ctrl.customer_exit_approval_workflow`
- `ctrl.post_exit_access_and_communication_controls`
- `ctrl.exit_evidence_and_audit_trail`
- `ctrl.exit_reversal_governance`

## 11. Typical Systems/Providers

### Typical systems
- Customer master
- Offboarding workbench
- CRM/case platform
- Status dashboard
- Records trigger service

### Typical providers
- ServiceNow
- Salesforce
- Pega
- Appian
- Fiserv

## 12. Open Questions
- Should voluntary exit and institution-initiated offboarding eventually split if the repo models consumer-harm and fairness controls in greater depth?
- At what point does "customer exit" occur if the customer has open but inactive residual obligations?
- Should post-exit servicing be treated as part of exit management or handed off fully to servicing?

## 13. Provenance

This capability closes the lifecycle arc that begins in Batch 1 onboarding and is stabilized by Batch 2 record governance. It is distinct from account closure and from retention/archival, but tightly dependent on both.
