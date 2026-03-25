---
id: cap.post_exit_access_governance
type: capability
name: Post-Exit Access Governance
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Post-Exit Access Governance

## Definition

Post-Exit Access Governance is the enduring business ability to adjust or revoke access identities, sessions, authenticators, and entitlements when a customer or user relationship exits or is restricted. It encompasses the orchestration of access cleanup triggered by relationship termination, including global session and token revocation, authenticator and identity termination, delegated access revocation, and the governance of post-exit exceptions and reinstatement requests.

## Business Outcome

Ensure that when a customer or user relationship ends, all associated access rights, sessions, tokens, authenticators, and delegations are revoked completely and promptly, with auditable evidence of cleanup completion. Minimize the risk of persistent access after exit, govern any time-bound exceptions, and ensure that reinstatement after exit follows a controlled revalidation process.

## Scope

### Included

- Post-exit access cleanup orchestration triggered by relationship termination events
- Global session and token revocation across all channels and services
- Authenticator and identity termination or suspension
- Delegated and third-party data access revocation
- Post-exit exception handling with time-bound governance
- Reinstatement after exit with revalidation and re-approval
- Evidence capture and audit trail for all post-exit access actions
- Cleanup completion tracking and reconciliation

### Excluded

- Customer exit decision-making and relationship closure (domain.customer_lifecycle)
- Standard access provisioning and deprovisioning outside of exit context (cap.access_provisioning_and_deprovisioning)
- Records retention and data disposition after exit (domain.records_information_management)
- Account recovery for active customers (cap.identity_recovery_and_reset_management)
- General access attestation outside of exit context (cap.access_audit_and_attestation)

## Dependencies

### Upstream

- domain.customer_lifecycle supplies the exit trigger events that initiate post-exit access governance
- cap.access_provisioning_and_deprovisioning supplies the access grants that must be revoked
- cap.session_and_device_trust_management supplies active session and token state for revocation
- cap.credential_and_authenticator_management supplies authenticator state for termination

### Downstream

- cap.access_audit_and_attestation receives post-exit cleanup evidence for audit
- domain.records_information_management receives signals to initiate data retention and disposition
- cap.access_event_monitoring_and_anomaly_response monitors for post-exit access attempts
- domain.compliance_regulatory_affairs consumes exit governance evidence for regulatory examination

## Value Streams

- vs.customer_exit_control
- vs.access_identity_lifecycle
- vs.access_governance
- vs.records_governance

## Journeys

- journey.exit_customer_relationship
- journey.deprovision_access_after_customer_exit
- journey.institution_initiated_customer_offboarding
- journey.revoke_delegated_access
- journey.revoke_third_party_data_access

## Processes

- proc.post_exit_access_cleanup
- proc.global_session_and_token_revocation
- proc.authenticator_and_identity_termination
- proc.post_exit_exception_handling
- proc.reinstatement_after_exit_review

## Risks

- Access persists after relationship exit due to incomplete or delayed cleanup
- Sessions, tokens, or delegations survive exit processing, allowing continued access
- Post-exit cleanup is inconsistent across systems, leaving orphaned access in some services
- Post-exit exceptions are ungoverned, creating indefinite access beyond the exit event
- Reinstatement after exit bypasses proper revalidation, restoring access without adequate review

## Controls

- ctrl.exit_triggered_access_cleanup_rules
- ctrl.global_session_token_and_authenticator_revocation
- ctrl.post_exit_cleanup_completion_tracking
- ctrl.time_bound_post_exit_exceptions
- ctrl.post_exit_evidence_and_audit_trail
- ctrl.reinstatement_revalidation_controls

## Systems and Providers

### Systems

- Post-exit cleanup orchestrator
- Deprovisioning and revocation service
- Global session and token termination service
- Access cleanup evidence dashboard
- Reinstatement review workflow

### Providers

- SailPoint
- Okta
- Microsoft Entra
- CyberArk
- Ping Identity

## Open Questions

- How should post-exit access governance handle joint accounts or shared access where only one party exits?
- What is the appropriate time window for completing post-exit access cleanup across all systems?
- Should post-exit exceptions be modeled as a distinct governance workflow given their unique risk profile?
- How should post-exit access governance coordinate with data retention and disposition requirements?
- What monitoring should remain active after exit to detect attempted reuse of revoked credentials or tokens?

## Provenance

Strongly supported by CISA ICAM Reference Architecture which identifies access deprovisioning and revocation as core lifecycle governance functions. FFIEC Authentication and Access guidance requires institutions to revoke access promptly upon relationship termination and to maintain evidence of deprovisioning actions. Post-exit access governance is a critical control expectation for regulatory examination, particularly for customer-facing financial services.
