---
id: cap.privileged_access_governance
type: capability
name: Privileged Access Governance
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Privileged Access Governance

## Definition

Privileged Access Governance is the enduring business ability to govern high-risk administrative, operational, or privileged access distinct from standard end-user access. It encompasses the request, approval, provisioning, monitoring, and recertification of privileged access including just-in-time access, break-glass access, privileged session management, and the governance controls that ensure privileged access is scoped, time-bound, monitored, and auditable.

## Business Outcome

Ensure that privileged access to critical systems, data, and administrative functions is explicitly requested, approved, scoped, time-bound, and monitored. Minimize standing privileged access, enforce strong assurance for privileged actions, and maintain auditable evidence of all privileged access activity to satisfy governance and regulatory requirements.

## Scope

### Included

- Privileged access request and approval workflows with business justification
- Just-in-time and time-bound privileged access provisioning
- Break-glass access execution with governance controls and post-use review
- Privileged session recording, monitoring, and review
- Privileged access recertification on a governed schedule
- Secret and credential vault management for privileged accounts
- Privileged access classification and tiering

### Excluded

- Standard end-user access provisioning and deprovisioning (cap.access_provisioning_and_deprovisioning)
- General authentication including MFA (cap.authentication)
- Access attestation for non-privileged access (cap.access_audit_and_attestation)
- Security operations incident response (domain.security_operations)
- Infrastructure and platform administration execution (domain.technology_operations)

## Dependencies

### Upstream

- cap.access_identity_establishment supplies the identities that receive privileged access grants
- cap.authentication supplies strong authentication for privileged access sessions
- cap.step_up_and_adaptive_authentication supplies step-up challenges for privileged actions
- cap.entitlement_and_authorization_policy_management supplies privileged access policies and classification rules

### Downstream

- cap.access_audit_and_attestation receives privileged access evidence for audit and attestation
- cap.access_event_monitoring_and_anomaly_response consumes privileged session events for anomaly detection
- domain.security_operations receives alerts from privileged session monitoring
- cap.access_provisioning_and_deprovisioning executes provisioning and revocation of privileged entitlements

## Value Streams

- vs.privileged_access_control
- vs.access_governance
- vs.high_risk_action_protection
- vs.zero_trust_access_evaluation

## Journeys

- journey.authorize_privileged_admin_action
- journey.request_break_glass_access
- journey.review_privileged_access_attestation
- journey.revoke_privileged_access
- journey.employee_access_request_and_approval

## Processes

- proc.privileged_access_request_and_approval
- proc.just_in_time_privileged_access
- proc.break_glass_access_execution
- proc.privileged_session_recording_and_review
- proc.privileged_access_recertification

## Risks

- Excessive privileged access accumulates beyond what is needed for current roles and responsibilities
- Standing administrative access is overused instead of just-in-time provisioning
- Privileged sessions are unmonitored, allowing unauthorized or harmful actions to go undetected
- Break-glass access bypasses governance controls without adequate post-use review
- Privileged access persists after role changes or departures, creating orphaned elevated access

## Controls

- ctrl.privileged_access_classification_rules
- ctrl.just_in_time_or_time_bound_privileged_access
- ctrl.break_glass_governance_controls
- ctrl.privileged_session_monitoring
- ctrl.strong_assurance_for_privileged_actions
- ctrl.privileged_access_recertification

## Systems and Providers

### Systems

- PAM platform
- Privileged session manager
- Secret vault
- Privileged access approval service
- Privileged activity evidence repository

### Providers

- CyberArk
- BeyondTrust
- Delinea
- Microsoft Entra
- Okta

## Open Questions

- How should privileged access governance for cloud-native infrastructure differ from traditional on-premises PAM?
- What is the right balance between just-in-time access and operational efficiency for roles that require frequent privileged access?
- Should break-glass access be modeled as a distinct sub-capability given its unique governance and audit requirements?
- How should privileged access governance extend to third-party vendors and managed service providers?
- What role should machine identity and service account governance play within privileged access governance?

## Provenance

Strongly supported by CISA ICAM Reference Architecture which identifies privileged access management as a distinct governance function requiring dedicated controls. CISA Zero Trust Maturity Model emphasizes least-privilege access and just-in-time provisioning as core zero-trust principles. Privileged access governance is a foundational expectation across financial regulatory frameworks including FFIEC, OCC, and Federal Reserve guidance.
