---
id: cap.credential_and_authenticator_management
type: capability
name: Credential and Authenticator Management
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Credential and Authenticator Management

## Definition

Credential and Authenticator Management is the enduring business ability to issue, bind, maintain, rotate, recover, suspend, and revoke the credentials and authenticators that allow an access identity to prove its claim at authentication time. This includes passwords, passkeys, hardware tokens, software tokens, biometric enrollments, and any other factor used for authentication. It governs the full lifecycle of these artifacts from enrollment through revocation.

## Business Outcome

Ensure that every access identity has appropriately strong, well-governed credentials and authenticators that support the institution's authentication assurance requirements, can be recovered safely when lost, and are revoked promptly when compromised or no longer needed.

## Scope

### Included

- Enrolling credentials and authenticators for access identities
- Binding authenticators to the correct access identity with appropriate verification
- Resetting credentials through governed recovery flows
- Replacing lost or compromised authenticators with step-up verification
- Rotating credentials and tokens on policy-defined schedules
- Suspending and revoking credentials and authenticators
- Registering passkeys and managing FIDO-based authenticators
- Notifying parties of credential and authenticator changes

### Excluded

- Runtime authentication using these credentials (cap.authentication)
- Access identity creation and lifecycle (cap.access_identity_establishment)
- Identity proofing (cap.identity_proofing_execution)
- Step-up and adaptive authentication decisions (cap.step_up_and_adaptive_authentication)
- Entitlement and authorization (cap.entitlement_and_authorization_policy_management)

## Dependencies

### Upstream

- cap.access_identity_establishment supplies the access identity to which credentials are bound
- cap.identity_proofing_execution may be required for initial enrollment or recovery re-proofing
- cap.step_up_and_adaptive_authentication may trigger factor replacement step-up requirements
- domain.orchestration_control may coordinate enrollment and recovery flows

### Downstream

- cap.authentication consumes credentials and authenticators at login time
- cap.step_up_and_adaptive_authentication consumes authenticator inventory for challenge selection
- cap.session_and_device_trust_management is affected by authenticator revocation
- cap.identity_recovery_and_reset_management depends on recovery method availability

## Value Streams

- vs.authenticator_lifecycle
- vs.identity_assurance
- vs.account_recovery
- vs.phishing_resistant_authentication

## Journeys

- journey.enroll_multi_factor_authentication
- journey.enroll_passkey
- journey.reset_credential
- journey.replace_lost_authenticator
- journey.recover_account_access

## Processes

- proc.authenticator_enrollment
- proc.credential_reset
- proc.factor_replacement_and_rebinding
- proc.authenticator_revocation
- proc.passkey_registration

## Risks

- Weak authenticators are allowed where stronger factors should be required
- Factor replacement flow is exploited for account takeover
- Revocation of compromised authenticators is delayed
- Credential reset controls are insufficient to prevent unauthorized resets
- Recovery methods become stale or unverified over time

## Controls

- ctrl.authenticator_policy_by_risk_tier
- ctrl.strong_factor_enrollment_controls
- ctrl.factor_replacement_step_up_controls
- ctrl.authenticator_change_notifications
- ctrl.authenticator_revocation_sla
- ctrl.recovery_method_governance

## Systems and Providers

### Systems

- Authenticator enrollment service
- MFA platform
- Passkey registration service
- Credential store
- Recovery workflow service

### Providers

- Okta
- Duo
- Ping Identity
- Microsoft Entra
- Yubico

## Open Questions

- Should passkey management be modeled as a distinct sub-capability given its unique lifecycle characteristics?
- How should biometric enrollment governance differ from other authenticator types?
- What is the right boundary between credential recovery here and the broader identity recovery capability?

## Provenance

Strongly supported by NIST SP 800-63B which defines authenticator lifecycle requirements including binding, renewal, loss, and revocation. CISA phishing-resistant MFA guidance reinforces the move toward stronger authenticator types. FIDO Alliance passkey specifications provide additional context for modern authenticator management.
