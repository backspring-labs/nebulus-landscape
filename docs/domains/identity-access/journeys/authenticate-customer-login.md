---
id: journey.authenticate_customer_login
type: journey
name: Authenticate Customer Login
domain_ids:
  - domain.identity_access
  - domain.channels_experience
  - domain.customer
  - domain.risk_fraud
capability_ids:
  - cap.authentication
  - cap.credential_and_authenticator_management
  - cap.session_and_device_trust_management
  - cap.step_up_and_adaptive_authentication
  - cap.access_policy_decisioning
  - cap.authorization_and_entitlements
  - cap.access_event_monitoring_and_anomaly_response
value_stream_id: vs.customer_digital_access
process_id: proc.customer_login_v1
status: draft
source_refs:
  - src.nist_sp800_63b4_authentication_lifecycle
  - src.ffiec_authentication_access_2021
  - src.cisa_zero_trust_guidance
last_reviewed: 2026-03-25
confidence: high
---

# Authenticate Customer Login

## 1. Objective

Authenticate a customer to access digital banking services and establish a governed, runtime-trusted session that reflects the customer's identity assurance level, device trust posture, and entitlement scope.

## 2. Primary Actor

An existing customer of the institution who holds valid credentials and seeks to access digital banking through a supported channel.

## 3. Trigger

The customer initiates a login action through a supported entry point (web, mobile app, or federated identity provider redirect).

## 4. Preconditions

- The customer has been previously onboarded and holds an active identity record.
- At least one valid credential or authenticator is enrolled and active.
- Authentication policies and adaptive rules are published and available to the policy decision point.
- The channel or application is registered and authorized to accept login requests.

## 5. Happy Path

### Stage 1 — Login initiation
The customer presents themselves at a digital entry point (web login page, mobile app launch, or federated redirect). The channel captures the initial identifier (username, email, or device token) and forwards it to the authentication service.

**Capabilities activated:** `cap.authentication`, `cap.session_and_device_trust_management`

### Stage 2 — Credential verification
The authentication service resolves the customer identity, retrieves enrolled credential metadata, and verifies the presented credential (password, biometric, passkey, or OTP). Device trust signals and session context are evaluated alongside the credential.

**Capabilities activated:** `cap.authentication`, `cap.credential_and_authenticator_management`, `cap.session_and_device_trust_management`

### Stage 3 — Adaptive risk evaluation
The access policy decision point evaluates contextual signals — device fingerprint, geolocation, login velocity, behavioral patterns — against published policy. If risk is within acceptable thresholds, authentication proceeds. If elevated risk is detected, step-up authentication is triggered.

**Capabilities activated:** `cap.access_policy_decisioning`, `cap.step_up_and_adaptive_authentication`

### Stage 4 — Authorization and session establishment
Upon successful authentication, the authorization service resolves the customer's entitlements, roles, and permitted actions. A governed session is created with appropriate scope, lifetime, and trust level. Session metadata is recorded for audit and anomaly detection.

**Capabilities activated:** `cap.authorization_and_entitlements`, `cap.session_and_device_trust_management`, `cap.access_event_monitoring_and_anomaly_response`

### Stage 5 — Access granted
The customer is presented with their authenticated experience. The session is actively monitored for anomalous behavior throughout its lifetime.

**Capabilities activated:** `cap.access_event_monitoring_and_anomaly_response`

## 6. Alternate Paths

- Step-up authentication is required due to elevated risk signals, new device, or sensitive action context.
- Federated login redirects through an external identity provider before returning to the institution's authorization layer.
- Biometric or passkey authentication bypasses traditional password flow.
- Limited or read-only session is established when full assurance cannot be achieved but partial access is permitted by policy.
- Remember-device token allows reduced friction on subsequent logins from trusted devices.

## 7. Failure / Exception Paths

- Invalid credentials exceed retry threshold, triggering temporary lockout.
- Device trust evaluation fails (rooted device, unknown device, VPN anomaly), blocking session creation.
- Step-up authentication fails or times out, denying elevated access.
- Account is in a restricted lifecycle state (suspended, under review), preventing login.
- Anomaly detection flags the login attempt as potentially fraudulent, routing to fraud review.
- Session creation fails due to concurrent session policy limits.

## 8. Related Domains

- `domain.identity_access` — owns the authentication, authorization, and session lifecycle
- `domain.channels_experience` — owns the presentation layer and entry point interactions
- `domain.customer` — provides the canonical customer identity and lifecycle state
- `domain.risk_fraud` — provides fraud signals and risk scoring that influence adaptive authentication decisions

## 9. Related Capabilities

- `cap.authentication`
- `cap.credential_and_authenticator_management`
- `cap.session_and_device_trust_management`
- `cap.step_up_and_adaptive_authentication`
- `cap.access_policy_decisioning`
- `cap.authorization_and_entitlements`
- `cap.access_event_monitoring_and_anomaly_response`

## 10. Value Stream Context

This journey is the primary realization of `vs.customer_digital_access`. Every digital interaction depends on a successfully authenticated and authorized session. The quality, speed, and security of this journey directly governs customer experience and institutional risk posture.

## 11. Supporting Process Candidates

- `proc.customer_login_v1`
- `proc.credential_verification`
- `proc.adaptive_risk_evaluation`
- `proc.session_creation`
- `proc.step_up_challenge`
- `proc.entitlement_resolution`
- `proc.login_event_recording`

## 12. Key Risks and Controls Encountered

### Key risks
- Credential compromise allows unauthorized access
- Session hijacking or replay attacks
- Device trust bypass through emulation or rooting
- Brute force or credential stuffing attacks
- Overly permissive session scope grants excessive access
- Adaptive rules fail open under error conditions

### Key controls
- Multi-factor authentication enforcement
- Credential lockout and velocity controls
- Device trust attestation and binding
- Session scope limitation and lifetime enforcement
- Adaptive policy evaluation with fail-closed defaults
- Real-time anomaly detection and session revocation
- Comprehensive audit trail for all authentication events

## 13. Typical Systems and Providers Involved

### Typical systems
- Identity provider / authentication server
- Policy decision point (PDP) and policy enforcement point (PEP)
- Device trust and fingerprinting service
- Session management service
- Entitlement and role resolution service
- Event streaming and anomaly detection platform
- Audit log repository

### Representative providers
- Ping Identity
- ForgeRock
- Okta
- Microsoft Entra ID
- Transmit Security
- BioCatch (behavioral biometrics)
- Splunk / Elastic (event monitoring)

## 14. Open Questions

- Should passwordless-first authentication be modeled as a separate journey variant or a path within this journey?
- How should continuous authentication (re-evaluation during session) relate to the initial login journey?
- Where does consent capture (cookie consent, terms acceptance) fit if required at login time?
- Should federated login be a distinct journey given the different trust model and token exchange mechanics?

## 15. Provenance

This journey represents the most fundamental Identity & Access domain journey. It is referenced by virtually every other journey in the landscape as a prerequisite or embedded step. Grounded in NIST SP 800-63B (digital authentication), FFIEC Authentication and Access guidance (2021), and CISA Zero Trust maturity model requirements for identity-centric access control.
