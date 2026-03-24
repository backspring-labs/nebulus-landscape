---
id: journey.exit_customer_relationship
type: journey
name: Exit Customer Relationship
domain_ids:
  - domain.customer
  - domain.accounts
  - domain.servicing
  - domain.identity_access
  - domain.channels_experience
  - domain.orchestration_control
  - domain.risk_fraud
capability_ids:
  - cap.customer_lifecycle_state_management
  - cap.customer_exit_management
  - cap.customer_record_retention_and_archival
  - cap.customer_change_management
  - cap.customer_data_issue_remediation
  - cap.customer_relationship_management
value_stream_id: vs.customer_lifecycle_governance
process_id: proc.customer_exit_v1
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: medium
---

# Exit Customer Relationship

## 1. Objective

Terminate or close the customer relationship in a controlled way that coordinates product/account dependencies, post-exit obligations, lifecycle-state changes, and record-retention triggers.

## 2. Primary Actor

Primary actor varies by path:
- Customer for voluntary exit
- Institution operations/compliance for institution-initiated exit

## 3. Trigger

A customer requests closure of the relationship, or the institution determines that the relationship should be terminated or wound down.

## 4. Preconditions

- The customer relationship exists.
- Exit criteria or policy triggers are defined.
- The institution can review dependent accounts, unresolved cases, obligations, and restrictions.
- Exit reason and evidence standards are defined.

## 5. Happy Path

### Stage 1 — Exit initiation
The customer or institution initiates the exit path and records the reason and intent.

**Capabilities activated:** `cap.customer_exit_management`

### Stage 2 — Dependency review
The institution checks for open accounts, unresolved obligations, pending disputes, residual servicing items, or other dependencies.

**Capabilities activated:** `cap.customer_exit_management`, `cap.customer_relationship_management`

**Participating neighbor domains:** `domain.accounts`, `domain.servicing`, `domain.risk_fraud`

### Stage 3 — Lifecycle status transition
The institution changes the customer lifecycle state to an exited, restricted, or pending-exit condition as appropriate.

**Capabilities activated:** `cap.customer_lifecycle_state_management`, `cap.customer_change_management`

### Stage 4 — Execute relationship exit
The institution completes the customer-level exit decision and applies customer-level post-exit handling rules.

**Capabilities activated:** `cap.customer_exit_management`

### Stage 5 — Post-exit controls and record governance
Access, communications, suppression, retention, and archival triggers are applied as needed.

**Capabilities activated:** `cap.customer_record_retention_and_archival`, `cap.customer_change_management`

**Participating neighbor domains:** `domain.identity_access`, `domain.channels_experience`

### Stage 6 — Closure and retrieval posture
The institution completes the exit workflow and preserves a retrievable evidentiary trail for later servicing, audit, or compliance needs.

**Capabilities activated:** `cap.customer_record_retention_and_archival`

## 6. Alternate Paths

- Voluntary exit after closing the final product
- Institution-initiated offboarding
- Exit is paused because dependent products remain open
- Exit is reversed or rescinded
- Dormant customer is not exited but reactivated instead

## 7. Failure / Exception Paths

- Customer exit attempted while products or obligations remain unresolved
- Active cases or disputes prevent completion
- Exit reason or evidence is insufficient
- Access or communication posture remains inconsistent after exit
- Retention/archival triggers are missed
- Conflicting downstream states require remediation

## 8. Related Domains

- `domain.customer` — owns lifecycle state, exit governance, and customer-level record outcome
- `domain.accounts` — owns account closure and final product dependency resolution
- `domain.servicing` — owns disputes, complaints, and residual servicing obligations
- `domain.identity_access` — adjusts access where appropriate
- `domain.channels_experience` — updates customer-facing posture/messages
- `domain.orchestration_control` — may coordinate complex offboarding workflows
- `domain.risk_fraud` — may supply exit triggers, restrictions, or required review context

## 9. Related Capabilities

- `cap.customer_lifecycle_state_management`
- `cap.customer_exit_management`
- `cap.customer_record_retention_and_archival`
- `cap.customer_change_management`
- `cap.customer_data_issue_remediation`
- `cap.customer_relationship_management`

## 10. Value Stream Context

This journey primarily sits in `vs.customer_lifecycle_governance`, with ties to records governance and post-relationship servicing posture.

## 11. Supporting Process Candidates

- `proc.customer_exit_v1`
- `proc.customer_exit_initiation`
- `proc.pre_exit_dependency_review`
- `proc.customer_exit_approval_and_execution`
- `proc.post_exit_obligation_management`
- `proc.customer_record_archival`

## 12. Key Risks and Controls Encountered

### Key risks
- Customer exited too early while dependencies remain
- Inconsistent post-exit treatment across systems
- Inadequate evidence for institution-initiated exit
- Fairness or consumer-harm concerns in involuntary offboarding
- Missed archival or suppression obligations

### Key controls
- Exit criteria and reason code standards
- Dependency checklist across products and servicing
- Lifecycle transition authorization
- Post-exit communication/access controls
- Exit evidence audit trail
- Retention and archival controls

## 13. Typical Systems and Providers Involved

### Typical systems
- Customer master
- Offboarding workbench
- CRM/case system
- Status dashboard
- Records trigger service
- Archive/evidence store

### Representative providers
- Salesforce
- ServiceNow
- Pega
- Appian
- Fiserv
- OpenText

## 14. Open Questions

- At what exact point should customer exit be considered complete if some post-exit obligations remain?
- Should voluntary and institution-initiated exit become separate canonical journeys later?
- How much access treatment belongs in this journey versus a dedicated Identity & Access offboarding journey?

## 15. Provenance

This journey closes the lifecycle arc that begins with onboarding. It exists to make customer-level relationship termination explicit and distinct from mere account closure.
