---
id: proc.customer_exit_v1
type: process
name: Customer Exit v1
journey_id: journey.exit_customer_relationship
capability_ids:
  - cap.customer_lifecycle_state_management
  - cap.customer_exit_management
  - cap.customer_record_retention_and_archival
  - cap.customer_change_management
  - cap.customer_data_issue_remediation
  - cap.customer_relationship_management
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: medium
---

# Customer Exit v1

## 1. Objective

Operationally terminate a customer relationship in a controlled way by reviewing dependencies, governing lifecycle transitions, coordinating post-exit treatment, and triggering record-governance obligations.

## 2. Scope

This process covers:
- exit initiation
- dependency and obligation review
- lifecycle-state transition
- exit approval/execution
- post-exit treatment
- retention/archival triggering

It does not cover:
- detailed account closure execution
- complaint/dispute resolution
- enterprise legal-hold processes outside customer context

## 3. Trigger

A voluntary customer closure request or an institution-initiated exit event starts the process.

## 4. Entry Conditions

- Customer relationship exists.
- Exit criteria and reason codes are defined.
- Dependency review across products and servicing is possible.
- Required evidence and approvals can be captured.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.customer_exit.exit_initiation` | Exit Initiation | `domain.customer` |
| `stage.customer_exit.dependency_review` | Dependency Review | `domain.customer` with `domain.accounts`, `domain.servicing`, `domain.risk_fraud` participation |
| `stage.customer_exit.lifecycle_transition` | Lifecycle Transition | `domain.customer` |
| `stage.customer_exit.exit_approval_and_execution` | Exit Approval and Execution | `domain.customer` |
| `stage.customer_exit.post_exit_treatment` | Post-Exit Treatment | `domain.customer` with `domain.identity_access` and `domain.channels_experience` participation |
| `stage.customer_exit.retention_and_archival_triggering` | Retention and Archival Triggering | `domain.customer` |

## 6. Stage-by-Stage Flow

### Stage: `stage.customer_exit.exit_initiation`

**Description:** Capture the intent to end the customer relationship and the initiating reason.

**Capabilities activated:** `cap.customer_exit_management`

**Systems involved:** `sys.offboarding_workbench`, `sys.customer_master`, `sys.crm_platform`

**Decision points:** Voluntary vs institution-initiated, eligible for exit review vs immediate hold, straightforward vs escalated case

**Risks:** Incorrect initiation reason, exit initiated on wrong customer, insufficient initiating evidence

**Controls:** Reason code requirements, exit initiation audit, customer identity confirmation for request-driven exits

**Evidence generated:** Exit request/trigger record, initiating reason, initiating party context

**Handoff:** Move to dependency review

### Stage: `stage.customer_exit.dependency_review`

**Description:** Review whether open products, active cases, residual obligations, or policy conditions block or complicate exit.

**Capabilities activated:** `cap.customer_exit_management`, `cap.customer_relationship_management`

**Participating domains:** `domain.accounts`, `domain.servicing`, `domain.risk_fraud`

**Systems involved:** `sys.customer_master`, `sys.offboarding_workbench`, `sys.crm_platform`, `sys.status_dashboard`

**Decision points:** Are accounts still open? Are disputes, complaints, or cases unresolved? Are restrictions or investigations present? Is exit allowed now, blocked, or conditional?

**Risks:** Unresolved dependency at exit, exit while servicing obligations remain, dependency state mismatch across systems

**Controls:** Pre-exit dependency checklist, dependency attestation by participating domains, cross-system status review

**Evidence generated:** Dependency review record, unresolved-obligation list, block/conditional-clearance decision

**Handoff:** If clear, move to lifecycle transition; otherwise hold/escalate.

### Stage: `stage.customer_exit.lifecycle_transition`

**Description:** Apply the appropriate interim or final lifecycle status based on the dependency review and exit path.

**Capabilities activated:** `cap.customer_lifecycle_state_management`, `cap.customer_change_management`

**Systems involved:** `sys.customer_master`, `sys.lifecycle_rules_engine`, `sys.status_dashboard`

**Decision points:** Exited vs pending-exit vs restricted, reversible vs final status, effective now vs future-dated status

**Risks:** Inaccurate customer status, status transition without adequate basis, inconsistent lifecycle treatment across domains

**Controls:** Allowed transition matrix, state change authorization, reason code requirements, state change audit trail

**Evidence generated:** Lifecycle status change, effective date, approving authority

**Handoff:** Move to exit approval and execution

### Stage: `stage.customer_exit.exit_approval_and_execution`

**Description:** Approve and execute the customer-level exit decision.

**Capabilities activated:** `cap.customer_exit_management`

**Systems involved:** `sys.offboarding_workbench`, `sys.audit_event_log`, `sys.crm_platform`

**Decision points:** Approved vs rejected vs held, reversible vs irreversible, special handling required for institution-initiated exit?

**Risks:** Inadequate exit evidence, unfair institution-initiated offboarding, execution before dependencies truly resolved

**Controls:** Customer exit approval workflow, exit evidence audit trail, escalation/second-review for higher-risk exit types

**Evidence generated:** Final exit decision, approval record, execution timestamp

**Handoff:** Move to post-exit treatment

### Stage: `stage.customer_exit.post_exit_treatment`

**Description:** Adjust customer-facing posture and access-related treatment after exit.

**Capabilities activated:** `cap.customer_change_management`, `cap.customer_exit_management`

**Participating domains:** `domain.identity_access`, `domain.channels_experience`

**Systems involved:** `sys.customer_master`, `sys.status_dashboard`, `sys.records_trigger_service`

**Decision points:** What access changes are needed? What communications are still permitted or required? What relationship treatment flags must be suppressed?

**Risks:** Inconsistent post-exit treatment, access remains open incorrectly, customer still treated as active in channel or engagement systems

**Controls:** Post-exit access controls, post-exit communication controls, suppression and routing updates

**Evidence generated:** Treatment-change events, access-adjustment references, communication posture update

**Handoff:** Move to retention and archival triggering

### Stage: `stage.customer_exit.retention_and_archival_triggering`

**Description:** Trigger and record the retention, archival, suppression, and disposition posture required after customer exit.

**Capabilities activated:** `cap.customer_record_retention_and_archival`

**Systems involved:** `sys.records_management_repository`, `sys.archive_store`, `sys.legal_hold_registry`, `sys.audit_retrieval_portal`

**Decision points:** What record classes are now archival-eligible? Do any legal holds override disposition? What retrieval posture must be preserved?

**Risks:** Missed archival or legal hold, premature destruction, inaccessible evidence after exit

**Controls:** Retention schedule assignment, legal-hold override control, archival/disposition audit logging, record classification mapping

**Evidence generated:** Retention trigger record, archive eligibility metadata, hold/disposition status

**Handoff:** Process complete; post-exit retrieval remains possible under governance rules.

## 7. Decision Points

- Voluntary vs institution-initiated exit
- Dependency clear vs blocked
- Pending-exit vs exited vs restricted lifecycle state
- Approve vs hold vs reject
- Post-exit treatment scope
- Archive now vs retain live due to hold/obligation

## 8. Alternate Outcomes

- Voluntary customer exit
- Institution-initiated offboarding
- Exit blocked by unresolved accounts/cases
- Pending-exit state while dependencies clear
- Exit reversed or rescinded
- Archival delayed due to legal hold

## 9. Risks (process-level)

- Customer relationship exited before dependencies resolve
- Insufficient evidence for involuntary exit
- Inconsistent treatment after exit
- Retention/archival obligations missed
- Lifecycle status drift

## 10. Controls (process-level)

- Dependency review checklist
- Lifecycle transition governance
- Exit approval workflow
- Post-exit treatment controls
- Retention and archival controls

## 11. Evidence / Audit Considerations

Most important audit artifacts:
- exit trigger and reason
- dependency-review record
- lifecycle transition evidence
- approval and execution record
- post-exit treatment changes
- retention/archive trigger records

## 12. Systems Involved (summary)

- offboarding workbench, customer master, CRM/case system, lifecycle rules engine, records management repository, archive and legal-hold systems

## 13. Provider Participation

Representative providers: Salesforce, ServiceNow, Pega, Appian, Fiserv, OpenText

## 14. Open Questions

- Should institution-initiated offboarding be split into a separate process later?
- What minimum dependency-clearing standard should be required before final exit?
- How much of access treatment should be modeled here versus in an Identity & Access process artifact?

## 15. Provenance

This process formalizes the end-of-life operational pattern for the customer relationship and keeps it separate from account closure and servicing case handling.
