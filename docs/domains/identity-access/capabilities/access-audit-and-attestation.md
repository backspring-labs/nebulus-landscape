---
id: cap.access_audit_and_attestation
type: capability
name: Access Audit and Attestation
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Access Audit and Attestation

## Definition

Access Audit and Attestation is the enduring business ability to maintain access logs, evidentiary records, attestation support, and review artifacts that demonstrate who has access to what, on what basis, and whether that access has been reviewed and affirmed as appropriate. It encompasses the compilation of access evidence, the orchestration of periodic and event-driven access reviews, the tracking of attestation remediation, and the production of audit-ready evidence for internal governance, regulatory examination, and external audit.

## Business Outcome

Provide demonstrable, auditable evidence that access rights are appropriate, reviewed, and governed according to institutional policy and regulatory requirements. Ensure that access attestation is substantive rather than perfunctory, that exceptions are recertified on a governed schedule, and that audit evidence is traceable to the approval basis for each access grant.

## Scope

### Included

- Periodic access review and attestation campaigns for standard and exception access
- Audit evidence compilation linking access grants to approval basis and policy
- Exception access recertification on a governed schedule
- Attestation remediation tracking to ensure review findings are acted upon
- Privileged access evidence review and compilation
- Access governance evidence repository management
- Audit reporting and dashboard support for governance and examination readiness

### Excluded

- Access provisioning and deprovisioning execution (cap.access_provisioning_and_deprovisioning)
- Entitlement policy definition and management (cap.entitlement_and_authorization_policy_management)
- Privileged access request and approval workflows (cap.privileged_access_governance)
- Real-time access monitoring and anomaly detection (cap.access_event_monitoring_and_anomaly_response)
- General regulatory compliance management (domain.compliance_regulatory_affairs)

## Dependencies

### Upstream

- cap.access_provisioning_and_deprovisioning supplies the access grants and revocations that form the evidence base
- cap.authorization_and_entitlements supplies entitlement assignments and policy context
- cap.privileged_access_governance supplies privileged access records and session evidence
- cap.access_event_monitoring_and_anomaly_response supplies access event logs

### Downstream

- domain.compliance_regulatory_affairs consumes audit evidence for regulatory examination
- cap.access_provisioning_and_deprovisioning receives remediation directives from attestation reviews
- Internal audit and external auditors consume access attestation artifacts
- Risk management consumes access review findings for risk assessment

## Value Streams

- vs.access_governance
- vs.audit_and_attestation_support
- vs.runtime_access_control
- vs.privileged_access_control

## Journeys

- journey.review_access_attestation
- journey.authorize_privileged_admin_action
- journey.deprovision_access_after_customer_exit
- journey.grant_role_or_permission
- journey.revoke_delegated_access

## Processes

- proc.access_review_and_attestation
- proc.audit_evidence_compilation
- proc.exception_access_recertification
- proc.attestation_remediation_tracking
- proc.privileged_access_evidence_review

## Risks

- Access evidence is incomplete or fragmented, making it impossible to demonstrate compliance
- Access reviews are performed superficially without meaningful evaluation of appropriateness
- Exception access is not recertified on schedule, allowing ungoverned access to persist
- Access approvals are untraceable to the original business justification or policy basis
- Sensitive access evidence is exposed to unauthorized parties, creating security or privacy risk

## Controls

- ctrl.access_evidence_retention_requirements
- ctrl.periodic_access_attestation_controls
- ctrl.exception_access_recertification
- ctrl.evidence_traceability_to_approval_basis
- ctrl.attestation_remediation_tracking
- ctrl.evidence_store_access_controls

## Systems and Providers

### Systems

- Access governance evidence repository
- Attestation and certification platform
- Access review dashboard
- Audit reporting service
- Privileged access evidence store

### Providers

- SailPoint
- CyberArk
- Microsoft Entra
- Okta
- Splunk

## Open Questions

- How should access attestation campaigns be scoped to avoid review fatigue while maintaining meaningful coverage?
- What is the right level of automation for attestation evidence compilation versus manual review?
- Should privileged access attestation follow a different cadence or depth than standard access attestation?
- How should access audit evidence retention policies balance long-term audit needs with data minimization?
- What role should continuous access monitoring play in supplementing periodic attestation reviews?

## Provenance

Strongly supported by CISA ICAM Reference Architecture which identifies access certification and recertification as core governance functions. FFIEC Authentication and Access guidance requires institutions to maintain auditable records of access decisions and to conduct periodic reviews. Access attestation is a foundational control expectation across all major regulatory frameworks for financial institutions.
