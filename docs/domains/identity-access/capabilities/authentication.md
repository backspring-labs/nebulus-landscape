---
id: cap.authentication
type: capability
name: Authentication
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Authentication

## Definition

Authentication is the enduring business ability to verify, at login or re-entry time, that a party presenting credentials or trust assertions is the party to whom the corresponding access identity was issued. It encompasses all forms of initial authentication including single-factor, multi-factor, and federated assertion-based authentication. It is the runtime trust gate that converts credential presentation into an authenticated session.

## Business Outcome

Ensure that only verified parties gain authenticated access to the institution's systems and services, at a level of assurance appropriate to the channel and context, while minimizing friction for legitimate users and blocking unauthorized access attempts.

## Scope

### Included

- Authenticating parties at login using credentials and authenticators bound to their access identity
- Enforcing multi-factor authentication where required by policy
- Handling authentication failures including lockout and retry management
- Consuming federated identity assertions from trusted identity providers
- Logging authentication events for audit and anomaly detection
- Supporting authentication across channels including web, mobile, and API

### Excluded

- Credential and authenticator issuance and lifecycle (cap.credential_and_authenticator_management)
- Step-up and adaptive authentication after initial login (cap.step_up_and_adaptive_authentication)
- Session management after authentication succeeds (cap.session_and_device_trust_management)
- Identity proofing (cap.identity_proofing_execution)
- Access identity lifecycle (cap.access_identity_establishment)
- Entitlement and authorization decisions (cap.entitlement_and_authorization_policy_management)

## Dependencies

### Upstream

- cap.access_identity_establishment supplies the access identity being authenticated
- cap.credential_and_authenticator_management supplies the credentials and authenticators used
- Federation partners supply identity assertions for federated authentication
- domain.channels_experience supplies the login surfaces

### Downstream

- cap.session_and_device_trust_management establishes sessions upon successful authentication
- cap.step_up_and_adaptive_authentication may augment authentication for sensitive actions
- cap.access_activity_monitoring_and_anomaly_detection consumes authentication events
- All downstream domain capabilities depend on authenticated access

## Value Streams

- vs.runtime_access_trust
- vs.phishing_resistant_authentication
- vs.customer_digital_access
- vs.employee_access_enablement

## Journeys

- journey.authenticate_customer_login
- journey.employee_login
- journey.partner_portal_login
- journey.reauthenticate_after_timeout
- journey.authenticate_federated_user

## Processes

- proc.login_authentication
- proc.multi_factor_authentication
- proc.authentication_failure_handling
- proc.lockout_and_retry_management
- proc.federated_assertion_authentication

## Risks

- Compromised credentials are used to authenticate successfully
- Phishing or credential stuffing attacks succeed against authentication controls
- Authentication strength is insufficient for the access context
- Lockout and retry controls are too weak or too aggressive
- Federated assertions are overtrusted without adequate validation

## Controls

- ctrl.authentication_policy_by_assurance_level
- ctrl.multi_factor_enforcement
- ctrl.failed_authentication_rate_limits
- ctrl.lockout_and_retry_controls
- ctrl.phishing_resistant_authentication_controls
- ctrl.authentication_event_logging

## Systems and Providers

### Systems

- Authentication service
- MFA verifier
- Federation assertion consumer
- Login risk signal service
- Authentication event log

### Providers

- Okta
- Auth0
- Ping Identity
- Microsoft Entra
- Duo

## Open Questions

- Should federated authentication be modeled as a distinct sub-capability given its unique trust model?
- How should API authentication (machine-to-machine) be scoped relative to user authentication?
- What is the right level of integration between authentication event logging and broader security monitoring?

## Provenance

Strongly supported by NIST SP 800-63B which defines authentication assurance levels and authenticator requirements. FFIEC guidance reinforces risk-appropriate authentication for financial institution access. CISA phishing-resistant MFA guidance supports the move toward stronger authentication methods.
