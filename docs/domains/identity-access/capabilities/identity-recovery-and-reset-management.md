---
id: cap.identity_recovery_and_reset_management
type: capability
name: Identity Recovery and Reset Management
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Identity Recovery and Reset Management

## Definition

Identity Recovery and Reset Management is the enduring business ability to recover account access, reset credentials, and re-establish trust after credential loss, lockout, or compromise. It encompasses all forms of recovery including self-service credential resets, authenticator replacement, assisted identity review, lockout recovery, and compromise-driven recovery workflows. It ensures that legitimate parties can regain access without creating a weaker bypass path that undermines the assurance level established at enrollment and authentication.

## Business Outcome

Enable legitimate parties to regain access to their accounts and services after credential loss, lockout, or compromise, while minimizing the risk of account takeover through recovery channels. Recovery paths must maintain assurance levels proportionate to the sensitivity of the access being restored, and all recovery actions must be auditable.

## Scope

### Included

- Account recovery initiation and intake across self-service and assisted channels
- Credential reset and rebinding to verified access identities
- Authenticator replacement including factor re-enrollment and device rebinding
- Lockout recovery including time-based, challenge-based, and assisted unlock
- Identity re-proofing for high-risk recovery scenarios
- Compromise-driven recovery including forced credential rotation and session invalidation
- Temporary recovery states with time-bound elevated trust
- Evidence capture and audit trail for all recovery events

### Excluded

- Initial identity proofing and enrollment (cap.identity_proofing_execution)
- Ordinary authentication including MFA (cap.authentication)
- Customer lifecycle decisions such as account closure or suspension (cap.post_exit_access_governance)
- General fraud investigation and case management (domain.fraud_financial_crime)
- Channel UX design for recovery flows (domain.channels_experience)

## Dependencies

### Upstream

- cap.access_identity_establishment supplies the access identity to which recovery restores access
- cap.credential_and_authenticator_management supplies the credentials and authenticators being reset or replaced
- cap.identity_proofing_execution supplies re-proofing capabilities for high-risk recovery
- domain.channels_experience supplies the recovery flow surfaces

### Downstream

- cap.session_and_device_trust_management must invalidate existing sessions when recovery involves compromise response
- cap.access_event_monitoring_and_anomaly_response consumes recovery events for anomaly detection
- cap.access_audit_and_attestation receives recovery evidence for audit purposes
- cap.authentication resumes normal authentication after recovery completes

## Value Streams

- vs.account_recovery
- vs.identity_assurance
- vs.runtime_access_trust
- vs.customer_digital_access

## Journeys

- journey.recover_account_access
- journey.reset_credential
- journey.replace_lost_authenticator
- journey.reinstate_access_identity
- journey.perform_identity_reproof_for_high_risk_change

## Processes

- proc.account_recovery_intake
- proc.identity_reproof_for_recovery
- proc.credential_reset_and_rebind
- proc.lockout_recovery
- proc.compromise_response_recovery

## Risks

- Recovery paths offer weaker assurance than the original enrollment or authentication, creating a bypass
- Account takeover succeeds through social engineering or exploitation of recovery channels
- Recovery methods become stale or unreachable, leaving users unable to recover access
- Assisted recovery by agents or support staff operates without adequate governance or oversight
- Post-recovery state retains elevated trust or temporary permissions beyond the intended window

## Controls

- ctrl.recovery_assurance_by_risk_tier
- ctrl.reproofing_requirements_for_sensitive_recovery
- ctrl.factor_replacement_step_up_controls
- ctrl.recovery_event_audit_trail
- ctrl.recovery_method_governance
- ctrl.post_recovery_session_cleanup

## Systems and Providers

### Systems

- Recovery workflow service
- Credential reset service
- Assisted identity review workbench
- Recovery event log
- Trust restoration service

### Providers

- Okta
- Ping Identity
- Microsoft Entra
- Duo
- Socure

## Open Questions

- Should compromise-driven recovery be modeled as a distinct sub-capability given its unique trust and urgency requirements?
- How should recovery assurance levels align with the original enrollment assurance levels defined by NIST SP 800-63?
- What is the appropriate boundary between identity recovery and fraud case management when recovery is triggered by suspected compromise?
- How should assisted recovery by contact center agents be governed differently from self-service recovery?
- Should temporary recovery states have their own session trust model distinct from normal post-authentication sessions?

## Provenance

Strongly supported by NIST SP 800-63B Section 6 which addresses authentication lifecycle management including reauthentication and credential recovery requirements. FFIEC Authentication and Access guidance reinforces the need for risk-appropriate recovery controls that do not undermine the assurance level of the original authentication. Recovery is a well-established identity management function across all major ICAM frameworks.
