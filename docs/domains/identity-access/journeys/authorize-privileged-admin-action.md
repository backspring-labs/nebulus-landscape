---
id: journey.authorize_privileged_admin_action
type: journey
name: Authorize Privileged Admin Action
domain_ids:
  - domain.identity_access
  - domain.orchestration_control
  - domain.risk_fraud
  - domain.servicing
capability_ids:
  - cap.authentication
  - cap.step_up_and_adaptive_authentication
  - cap.authorization_and_entitlements
  - cap.access_policy_decisioning
  - cap.privileged_access_governance
  - cap.access_audit_and_attestation
  - cap.access_event_monitoring_and_anomaly_response
  - cap.session_and_device_trust_management
value_stream_id: vs.privileged_access_control
process_id: proc.privileged_admin_action_v1
status: draft
source_refs:
  - src.cisa_zero_trust_guidance
  - src.cisa_icam_reference_architecture
  - src.ffiec_authentication_access_2021
last_reviewed: 2026-03-25
confidence: high
---

# Authorize Privileged Admin Action

## 1. Objective

Ensure that an administrator or operator performing a high-risk privileged action does so under stronger identity assurance, explicit authorization, governance controls, and comprehensive evidence capture, consistent with least-privilege and zero-trust principles.

## 2. Primary Actor

An administrator, operator, or privileged user (internal or contracted) who holds elevated entitlements and needs to perform a high-impact action on institutional systems, configurations, or customer data.

## 3. Trigger

The administrator initiates a privileged action (system configuration change, bulk data operation, access policy modification, emergency break-glass access) that exceeds normal-session authorization thresholds.

## 4. Preconditions

- The administrator holds an active privileged identity with entitlements that include the requested action scope.
- The administrator has an active authenticated session at a baseline assurance level.
- Privileged access policies, approval workflows, and audit requirements are published and enforced.
- The target system or resource is registered in the privileged access governance inventory.

## 5. Happy Path

### Stage 1 — Privileged action request
The administrator initiates a high-risk action from a privileged console, admin workbench, or operational tool. The system identifies the action as privileged and routes it through the privileged access governance layer.

**Capabilities activated:** `cap.privileged_access_governance`, `cap.authorization_and_entitlements`

### Stage 2 — Step-up authentication
The system requires the administrator to re-authenticate at a higher assurance level appropriate to the action risk. This may include hardware token, biometric, or out-of-band verification.

**Capabilities activated:** `cap.authentication`, `cap.step_up_and_adaptive_authentication`, `cap.session_and_device_trust_management`

### Stage 3 — Policy evaluation and authorization
The access policy decision point evaluates the request against privileged access policies — role entitlements, time-of-day restrictions, segregation-of-duty rules, and risk context. If the action requires approval, a request is routed to an approver.

**Capabilities activated:** `cap.access_policy_decisioning`, `cap.authorization_and_entitlements`, `cap.privileged_access_governance`

### Stage 4 — Action execution under governance
Upon authorization (and approval if required), the action is executed within a governed, time-bounded privileged session. All actions within the session are recorded with full attribution and evidence.

**Capabilities activated:** `cap.privileged_access_governance`, `cap.session_and_device_trust_management`, `cap.access_audit_and_attestation`

### Stage 5 — Session closure and audit completion
The privileged session is closed upon action completion or timeout. The complete audit record — including who, what, when, why, and approval chain — is finalized and made available for attestation review.

**Capabilities activated:** `cap.access_audit_and_attestation`, `cap.access_event_monitoring_and_anomaly_response`

## 6. Alternate Paths

- Emergency break-glass access bypasses normal approval workflow but triggers immediate alert and mandatory post-action review.
- Dual-approval required for highest-risk actions (e.g., bulk customer data export, production configuration change).
- Time-limited just-in-time (JIT) access is provisioned for the specific action rather than persistent elevated entitlements.
- Action is delegated to a service account under the administrator's governance and attribution.
- Remote privileged access requires additional network-layer and device-trust verification.

## 7. Failure / Exception Paths

- Step-up authentication fails, blocking the privileged action.
- Policy evaluation denies the action due to insufficient entitlements or segregation-of-duty conflict.
- Approval request times out or is rejected by the approver.
- Privileged session exceeds its time boundary and is forcibly terminated.
- Anomaly detection flags unusual privileged activity (off-hours, unusual scope), triggering session suspension and investigation.
- Break-glass access is used but post-action review reveals unauthorized or inappropriate use.

## 8. Related Domains

- `domain.identity_access` — owns the privileged authentication, authorization, governance, and audit lifecycle
- `domain.orchestration_control` — may own workflow orchestration for approval chains and action execution
- `domain.risk_fraud` — provides risk signals and anomaly detection for privileged activity
- `domain.servicing` — may own the operational context and case management for privileged actions on customer data

## 9. Related Capabilities

- `cap.authentication`
- `cap.step_up_and_adaptive_authentication`
- `cap.authorization_and_entitlements`
- `cap.access_policy_decisioning`
- `cap.privileged_access_governance`
- `cap.access_audit_and_attestation`
- `cap.access_event_monitoring_and_anomaly_response`
- `cap.session_and_device_trust_management`

## 10. Value Stream Context

This journey is the primary realization of `vs.privileged_access_control`. It ensures that the institution's most sensitive operations are performed under appropriate governance, reducing insider threat risk and satisfying regulatory requirements for privileged access management. The journey bridges operational efficiency (administrators can act) with institutional control (actions are governed and evidenced).

## 11. Supporting Process Candidates

- `proc.privileged_admin_action_v1`
- `proc.privileged_action_request`
- `proc.step_up_reauthentication`
- `proc.privileged_policy_evaluation`
- `proc.approval_workflow_routing`
- `proc.governed_session_creation`
- `proc.privileged_action_execution`
- `proc.privileged_session_closure`
- `proc.break_glass_access`
- `proc.post_action_attestation_review`

## 12. Key Risks and Controls Encountered

### Key risks
- Privilege escalation through compromised administrator credentials
- Insider threat — authorized administrator performs unauthorized action
- Break-glass abuse circumvents normal governance
- Persistent over-provisioned entitlements violate least-privilege
- Audit gaps prevent post-incident reconstruction
- Segregation-of-duty violations allow single-actor high-impact changes

### Key controls
- Mandatory step-up authentication for privileged actions
- Just-in-time privilege provisioning with automatic expiry
- Segregation-of-duty enforcement at policy decision point
- Dual-approval for highest-risk action categories
- Time-bounded privileged sessions with forced closure
- Comprehensive session recording with full attribution
- Break-glass access alerting and mandatory post-action review
- Periodic privileged access attestation and recertification
- Anomaly detection on privileged activity patterns

## 13. Typical Systems and Providers Involved

### Typical systems
- Privileged access management (PAM) platform
- Identity governance and administration (IGA) platform
- Policy decision point (PDP) for privileged access
- Session recording and audit platform
- Approval workflow engine
- SIEM / anomaly detection platform
- Privileged access workstation or jump server

### Representative providers
- CyberArk
- BeyondTrust
- Delinea (Thycotic/Centrify)
- SailPoint
- Saviynt
- HashiCorp Vault (secrets and JIT access)
- Splunk / Microsoft Sentinel (monitoring)

## 14. Open Questions

- Should break-glass access be modeled as a distinct journey given its fundamentally different trust and governance model?
- How should privileged access governance interact with cloud-native IAM (AWS IAM, Azure RBAC) where the privilege boundary is at the cloud provider?
- Where does privileged access for automated service accounts (CI/CD pipelines, infrastructure-as-code) fit relative to this human-actor journey?
- Should the journey model distinguish between customer-data-affecting privileged actions and infrastructure-only privileged actions?

## 15. Provenance

This journey is grounded in CISA Zero Trust Maturity Model requirements for privileged access, CISA ICAM Reference Architecture guidance on privileged identity governance, and FFIEC Authentication and Access guidance (2021) on administrative access controls. Privileged access compromise is consistently identified as one of the highest-impact attack vectors in financial services, making this journey critical to institutional security posture.
