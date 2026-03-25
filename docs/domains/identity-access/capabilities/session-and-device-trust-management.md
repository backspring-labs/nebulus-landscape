---
id: cap.session_and_device_trust_management
type: capability
name: Session and Device Trust Management
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Session and Device Trust Management

## Definition

Session and Device Trust Management is the enduring business ability to establish, monitor, refresh, and revoke trust in authenticated sessions and the devices from which they originate. It governs the ongoing trust relationship between the institution and an authenticated party after initial authentication succeeds, including session timeouts, reauthentication triggers, device trust evaluation, and global session revocation.

## Business Outcome

Ensure that authenticated sessions remain trustworthy throughout their lifetime, that session trust degrades and is refreshed appropriately based on time and context, that device trust contributes meaningfully to access decisions, and that sessions can be revoked promptly and completely when required.

## Scope

### Included

- Establishing authenticated sessions after successful authentication
- Enforcing session timeout and inactivity policies
- Triggering reauthentication when session trust degrades or policy requires it
- Evaluating and maintaining device trust state
- Revoking individual or all sessions for an access identity
- Binding sessions to authenticated context to prevent hijacking
- Logging session lifecycle events for audit

### Excluded

- Initial authentication (cap.authentication)
- Step-up authentication decisions (cap.step_up_and_adaptive_authentication)
- Credential and authenticator management (cap.credential_and_authenticator_management)
- Network-level access control (domain.network_security)
- Endpoint management and device compliance (domain.endpoint_management)

## Dependencies

### Upstream

- cap.authentication establishes the authenticated context that initiates the session
- cap.step_up_and_adaptive_authentication may update session trust after assurance elevation
- Device posture signals from endpoint management or security domains
- domain.channels_experience supplies the session surfaces

### Downstream

- cap.step_up_and_adaptive_authentication consumes session trust state for challenge decisions
- All domain capabilities that require authenticated access depend on valid sessions
- cap.access_activity_monitoring_and_anomaly_detection consumes session events
- cap.access_identity_establishment is affected by global session revocation during deprovisioning

## Value Streams

- vs.runtime_access_trust
- vs.session_continuity_and_protection
- vs.customer_digital_access
- vs.high_risk_action_protection

## Journeys

- journey.authenticate_customer_login
- journey.reauthenticate_after_timeout
- journey.perform_step_up_for_sensitive_change
- journey.logout_all_sessions
- journey.deprovision_access_after_customer_exit

## Processes

- proc.session_establishment
- proc.session_timeout_and_termination
- proc.device_trust_evaluation
- proc.session_reauthentication
- proc.global_session_revocation

## Risks

- Sessions persist longer than appropriate for the access context
- Session tokens are hijacked or replayed
- Device trust state is overtrusted or not validated
- Session revocation is incomplete leaving orphaned sessions active
- Timeout policies are inconsistent across channels

## Controls

- ctrl.session_timeout_policy
- ctrl.reauthentication_requirements_by_assurance_level
- ctrl.device_trust_expiration_rules
- ctrl.global_session_revocation_controls
- ctrl.session_binding_and_integrity_controls
- ctrl.session_event_audit_logging

## Systems and Providers

### Systems

- Session management service
- Token store
- Trusted device registry
- Reauthentication trigger service
- Session event log

### Providers

- Okta
- Auth0
- Ping Identity
- Microsoft Entra
- Cloudflare

## Open Questions

- Should device trust management be modeled as a distinct capability given its unique lifecycle and signal sources?
- How should session trust interact with zero-trust architecture principles where trust is continuously evaluated?
- What is the right boundary between session revocation here and access deprovisioning in cap.access_identity_establishment?

## Provenance

Strongly supported by NIST SP 800-63B session management requirements which define session binding, timeout, and reauthentication practices. OWASP session management guidance provides additional technical context for secure session handling.
