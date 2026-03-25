---
id: cap.access_provisioning_and_deprovisioning
type: capability
name: Access Provisioning and Deprovisioning
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Access Provisioning and Deprovisioning

## Definition

Access Provisioning and Deprovisioning is the enduring business ability to grant, modify, and remove access rights across systems and applications in response to lifecycle events, access requests, and governance actions. It translates entitled access into actual system-level permissions and ensures that access is revoked promptly when no longer authorized. It is the fulfillment layer that connects the governed entitlement model to technical access reality.

## Business Outcome

Ensure that access rights are granted promptly when authorized, modified accurately when roles or relationships change, and revoked completely and timely when access is no longer warranted, minimizing orphan accounts, access drift, and the risk window created by delayed deprovisioning.

## Scope

### Included

- Processing access requests through approval workflows
- Automating provisioning fulfillment to target systems and applications
- Deprovisioning access in response to lifecycle events such as termination, role change, or relationship end
- Detecting and cleaning up orphan accounts
- Reconciling provisioned access against entitled access
- Supporting emergency access revocation
- Maintaining provisioning audit trails

### Excluded

- Defining the entitlement or permission model (cap.authorization_and_entitlements)
- Runtime access decisions (cap.access_policy_decisioning)
- Authentication or credential issuance (cap.authentication, cap.credential_and_authenticator_management)
- Identity proofing or access identity creation (cap.identity_proofing_execution, cap.access_identity_establishment)
- HR or organizational role management (domain.workforce)

## Dependencies

### Upstream

- cap.authorization_and_entitlements supplies the entitled access that provisioning fulfills
- cap.access_identity_establishment supplies the access identities to which access is provisioned
- HR and workforce lifecycle systems supply joiner, mover, and leaver events
- Access request and approval workflows supply approved provisioning requests

### Downstream

- Target systems and applications receive provisioned access
- cap.access_policy_decisioning evaluates access that has been provisioned
- cap.access_activity_monitoring_and_anomaly_detection monitors for provisioning anomalies
- Access governance and certification processes consume reconciliation data

## Value Streams

- vs.access_governance
- vs.employee_access_enablement
- vs.joiner_mover_leaver_lifecycle
- vs.customer_digital_access

## Journeys

- journey.provision_new_hire_access
- journey.modify_access_on_role_change
- journey.deprovision_access_on_termination
- journey.request_and_approve_application_access
- journey.emergency_access_revocation

## Processes

- proc.access_request_and_approval
- proc.automated_provisioning_fulfillment
- proc.deprovisioning_on_lifecycle_event
- proc.orphan_account_detection_and_cleanup
- proc.provisioning_reconciliation

## Risks

- Delayed deprovisioning leaves access active after authorization ends
- Orphan accounts persist without an associated active identity
- Provisioning drifts from entitled access over time
- Incomplete access revocation leaves residual permissions in some systems
- Unauthorized self-provisioning bypasses approval controls

## Controls

- ctrl.automated_deprovisioning_on_termination
- ctrl.provisioning_approval_workflow
- ctrl.orphan_account_detection_controls
- ctrl.provisioning_reconciliation_checks
- ctrl.emergency_revocation_procedures
- ctrl.provisioning_audit_trail

## Systems and Providers

### Systems

- Identity governance platform
- Provisioning connector framework
- Access request portal
- Deprovisioning orchestrator
- Provisioning reconciliation engine

### Providers

- SailPoint
- Saviynt
- Okta
- Microsoft Entra
- One Identity

## Open Questions

- What is the target SLA for deprovisioning upon termination across different system tiers?
- How should provisioning for non-human identities (service accounts, bots) be governed?
- Should provisioning reconciliation run continuously or on a scheduled basis?

## Provenance

NIST SP 800-53 AC family controls directly address provisioning and deprovisioning requirements. CISA ICAM reference architecture includes provisioning as a core identity governance function. FFIEC guidance emphasizes timely access revocation as a critical control for financial institutions.
