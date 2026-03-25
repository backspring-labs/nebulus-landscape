---
id: proc.privileged_admin_action_v1
type: process
name: Privileged Admin Action v1
journey_id: journey.authorize_privileged_admin_action
capability_ids:
  - cap.privileged_access_governance
  - cap.step_up_and_adaptive_authentication
  - cap.access_policy_decisioning
  - cap.authorization_and_entitlements
  - cap.access_event_monitoring_and_anomaly_response
  - cap.access_audit_and_attestation
status: draft
source_refs:
  - src.cisa_zero_trust_guidance
  - src.cisa_icam_reference_architecture
last_reviewed: 2026-03-25
confidence: high
---

# Privileged Admin Action v1

## 1. Objective

Operationally execute the controlled sequence required to intake, authorize, assure, execute, monitor, and evidence a high-risk privileged administrative action under stronger-than-normal policy, assurance, monitoring, and evidence controls.

## 2. Scope

This process covers:
- privileged action request intake and classification
- eligibility and scope determination (role, entitlement, time-bound access)
- strong assurance execution (step-up or re-authentication)
- runtime policy evaluation against privileged action rules
- privileged execution window with active monitoring
- evidence and review capture for audit and attestation

This process does **not** own:
- standard (non-privileged) administrative actions
- privileged access request/approval workflows that occur outside the execution window (e.g., long-lived PAM request queues)
- fraud or threat detection beyond access monitoring (`domain.risk_fraud`)
- infrastructure-level privileged access management (OS, database, network)
- break-glass incident response execution (`domain.risk_fraud` or operational resilience processes)

## 3. Trigger

An administrator or operator initiates a classified privileged action — such as modifying access policy, changing entitlement configurations, overriding controls, or accessing sensitive customer data — through an administrative interface or API.

## 4. Entry Conditions

- The requesting administrator holds an active identity with a role eligible for privileged access.
- Privileged access classification rules are published and available.
- The PAM platform and policy decision point are operational.
- Strong assurance challenge services are available.
- Privileged session monitoring is active and recording.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.privileged_admin_action_v1.request_intake` | Request Intake | `domain.identity_access` |
| `stage.privileged_admin_action_v1.eligibility_and_scope_determination` | Eligibility and Scope Determination | `domain.identity_access` |
| `stage.privileged_admin_action_v1.strong_assurance_execution` | Strong Assurance Execution | `domain.identity_access` |
| `stage.privileged_admin_action_v1.runtime_policy_evaluation` | Runtime Policy Evaluation | `domain.identity_access` |
| `stage.privileged_admin_action_v1.privileged_execution_window` | Privileged Execution Window | `domain.identity_access` |
| `stage.privileged_admin_action_v1.evidence_and_review_capture` | Evidence and Review Capture | `domain.identity_access` |

## 6. Stage-by-Stage Flow

### Stage: `stage.privileged_admin_action_v1.request_intake`

**Description:** The institution receives the privileged action request, captures the action type and target scope, and classifies the request against the privileged access classification rules to determine the required control path.

**Capabilities activated:** `cap.privileged_access_governance`

**Systems involved:** `sys.pam_platform`

**Decision points:**
- What is the action type and classification level?
- Is this action classified as privileged under current policy?
- Does the request include sufficient context (target, scope, justification) to proceed?

**Risks:** `risk.excessive_privileged_scope`

**Controls:** `ctrl.privileged_access_classification_rules`

**Evidence generated:** Privileged action request receipt, action classification result, target and scope metadata, requesting administrator identity, request timestamp

**Handoff:** If the action is classified as privileged and context is sufficient, route to eligibility and scope determination. If not classified as privileged, route to standard action path.

### Stage: `stage.privileged_admin_action_v1.eligibility_and_scope_determination`

**Description:** The PAM platform and authorization service evaluate whether the requesting administrator is eligible for the classified action — based on role, entitlement, time-bound access grants, and any required approval chain — and determine the permitted scope of the action.

**Capabilities activated:** `cap.privileged_access_governance`, `cap.authorization_and_entitlements`

**Systems involved:** `sys.pam_platform`, `sys.privileged_access_approval_service`

**Decision points:**
- Does the administrator hold a role with standing or just-in-time eligibility for this action?
- Is the action within a valid time-bound access window?
- Is peer or manager approval required and, if so, has it been obtained?
- What is the permitted scope (target systems, data, duration)?

**Risks:** `risk.excessive_privileged_scope`

**Controls:** `ctrl.just_in_time_or_time_bound_privileged_access`, `ctrl.privileged_access_classification_rules`

**Evidence generated:** Eligibility evaluation result, role and entitlement basis, approval chain status (if applicable), permitted scope determination, time-bound window validation

**Handoff:** If eligible and within scope, proceed to strong assurance execution. If ineligible or awaiting approval, hold or deny.

### Stage: `stage.privileged_admin_action_v1.strong_assurance_execution`

**Description:** The administrator is required to complete a stronger-than-normal authentication challenge — step-up MFA, re-authentication, hardware token validation, or biometric confirmation — to confirm that the person executing the privileged action is the authorized administrator.

**Capabilities activated:** `cap.step_up_and_adaptive_authentication`, `cap.privileged_access_governance`

**Systems involved:** `sys.step_up_challenge_service`, `sys.pam_platform`

**Decision points:**
- Which strong assurance method is required for this action classification?
- Did the administrator complete the challenge successfully?
- Has the challenge timed out or been abandoned?

**Risks:** `risk.insufficient_assurance_for_privileged_action`

**Controls:** `ctrl.strong_assurance_for_privileged_actions`

**Evidence generated:** Strong assurance challenge type, challenge delivery confirmation, challenge response result, assurance level achieved, completion timestamp

**Handoff:** If assurance is confirmed, proceed to runtime policy evaluation. If failed or abandoned, deny the action and log the failure.

### Stage: `stage.privileged_admin_action_v1.runtime_policy_evaluation`

**Description:** The policy decision point evaluates the complete request context — action classification, administrator identity, assurance level, scope, time window, and any environmental conditions — against published privileged action policy rules to produce a permit/deny decision.

**Capabilities activated:** `cap.access_policy_decisioning`, `cap.privileged_access_governance`

**Systems involved:** `sys.policy_decision_point`, `sys.pam_platform`

**Decision points:**
- Does the full request context satisfy all policy conditions?
- Are there any override or exception conditions in effect?
- Is a break-glass path being invoked, and if so, are the break-glass controls satisfied?

**Risks:** `risk.break_glass_control_bypass`

**Controls:** `ctrl.runtime_privileged_policy_rules`

**Evidence generated:** Policy evaluation decision (permit/deny), policy rule references, override or exception flags, break-glass invocation record (if applicable), policy version reference

**Handoff:** If policy permits, open the privileged execution window. If denied, terminate with denial record.

### Stage: `stage.privileged_admin_action_v1.privileged_execution_window`

**Description:** The privileged action is executed within a governed, time-bound, and actively monitored session window. The privileged session manager records all actions taken during the window and enforces scope and time constraints.

**Capabilities activated:** `cap.privileged_access_governance`, `cap.access_event_monitoring_and_anomaly_response`

**Systems involved:** `sys.privileged_session_manager`, `sys.pam_platform`

**Decision points:**
- Is the action remaining within the permitted scope?
- Has the time-bound window expired?
- Are there anomalous actions within the privileged session that should trigger alerts or termination?

**Risks:** `risk.unmonitored_privileged_execution`, `risk.excessive_privileged_scope`

**Controls:** `ctrl.privileged_session_monitoring`, `ctrl.just_in_time_or_time_bound_privileged_access`

**Evidence generated:** Privileged session recording, action-by-action audit log, scope boundary enforcement events, time-bound window enforcement, anomaly alerts (if triggered)

**Handoff:** When the action is complete or the time window expires, close the privileged session and proceed to evidence and review capture.

### Stage: `stage.privileged_admin_action_v1.evidence_and_review_capture`

**Description:** The complete evidence package for the privileged action is assembled, linked to the original request and approval basis, and stored for audit, attestation, and periodic review.

**Capabilities activated:** `cap.access_audit_and_attestation`, `cap.privileged_access_governance`

**Systems involved:** `sys.access_governance_evidence_repository`, `sys.pam_platform`

**Decision points:**
- Is the evidence package complete (request, approval, assurance, policy decision, session recording, outcome)?
- Does the action require mandatory post-action review?
- Should the evidence be flagged for the next periodic privileged access attestation cycle?

**Risks:** `risk.incomplete_privileged_evidence`

**Controls:** `ctrl.evidence_traceability_to_approval_basis`

**Evidence generated:** Complete evidence package (request, classification, eligibility, approval, assurance, policy decision, session recording, outcome), review flag, attestation cycle linkage

**Handoff:** Process complete; evidence is stored and linked. Post-action review or attestation cycle proceeds independently.

## 7. Decision Points

Key decisions across the process:
- Action classification as privileged vs standard
- Administrator eligibility and scope determination
- Approval chain completion (if required)
- Strong assurance challenge success vs failure
- Runtime policy permit vs deny
- Scope and time-bound enforcement during execution
- Break-glass invocation validation
- Evidence completeness and review flagging

## 8. Alternate Outcomes

- Action classified as non-privileged and routed to standard path
- Administrator ineligible or awaiting approval
- Strong assurance challenge failed or abandoned
- Runtime policy denial due to environmental or contextual conditions
- Break-glass override invoked with elevated evidence and review requirements
- Privileged session terminated due to scope violation or anomaly detection
- Post-action review triggers remediation or access revocation

## 9. Risks (process-level)

- Excessive privileged scope granted beyond operational need
- Insufficient assurance for the risk level of the privileged action
- Unmonitored privileged execution allowing undetectable changes
- Break-glass control bypass without proper evidence or review
- Incomplete evidence making post-incident attribution difficult
- Standing privileged access that is never reviewed or attested

## 10. Controls (process-level)

- Privileged access classification rules defining action tiers
- Just-in-time and time-bound access grants instead of standing privileges
- Strong assurance requirements for privileged action execution
- Runtime policy evaluation at the point of execution
- Active privileged session monitoring and recording
- Evidence traceability linking every action to its approval basis

## 11. Evidence / Audit Considerations

This process generates a high-density audit trail:
- privileged action request and classification
- eligibility evaluation and approval chain
- strong assurance challenge and result
- runtime policy evaluation decision
- privileged session recording (action-by-action)
- scope and time-bound enforcement events
- complete evidence package with approval basis linkage
- review and attestation cycle flags

## 12. Systems Involved (summary)

- PAM platform
- privileged access approval service
- policy decision point
- step-up challenge service
- privileged session manager
- access governance evidence repository

## 13. Provider Participation

Representative providers often adjacent to this process:
- CyberArk, BeyondTrust, Delinea, HashiCorp Vault, Saviynt, SailPoint, Ping Identity, Okta

## 14. Open Questions

- Should break-glass be modeled as a separate process variant with its own distinct controls and evidence requirements?
- How should privileged actions that span multiple systems or domains be coordinated and evidenced?
- Should the approval workflow stage be broken out as a distinct pre-process or remain embedded in eligibility determination?
- How should periodic privileged access attestation cycles reference the evidence generated by this process?

## 15. Provenance

This process is the highest-assurance Identity & Access domain process because it governs actions that can modify the security posture of the institution itself. The combination of classification, just-in-time access, strong assurance, runtime policy evaluation, active monitoring, and traceable evidence is what distinguishes governed privileged access from uncontrolled administrative power.
