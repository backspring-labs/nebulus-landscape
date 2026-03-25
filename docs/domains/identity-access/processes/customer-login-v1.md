---
id: proc.customer_login_v1
type: process
name: Customer Login v1
journey_id: journey.authenticate_customer_login
capability_ids:
  - cap.authentication
  - cap.credential_and_authenticator_management
  - cap.session_and_device_trust_management
  - cap.step_up_and_adaptive_authentication
  - cap.access_policy_decisioning
  - cap.authorization_and_entitlements
  - cap.access_event_monitoring_and_anomaly_response
status: draft
source_refs:
  - src.nist_sp800_63b4_authentication_lifecycle
  - src.ffiec_authentication_access_2021
last_reviewed: 2026-03-25
confidence: high
---

# Customer Login v1

## 1. Objective

Operationally execute the controlled sequence required to authenticate a customer, evaluate session and device trust context, apply adaptive assurance policy, establish a governed session, and release runtime permissions for digital banking access.

## 2. Scope

This process covers:
- login request intake and identifier resolution
- primary credential or authenticator validation
- session and device context evaluation
- adaptive assurance escalation decisioning
- step-up challenge execution when required
- session establishment with trust and lifetime bindings
- permission evaluation and entitlement release

This process does **not** own:
- customer onboarding or identity proofing (`domain.customer`, `domain.identity_access` proofing capabilities)
- credential enrollment or lifecycle management outside the login context (`cap.credential_and_authenticator_management` — enrollment flows)
- fraud or AML transaction monitoring (`domain.risk_fraud`)
- channel presentation or UX orchestration (`domain.channels_experience`)

## 3. Trigger

A customer initiates a login action through a supported digital entry point (web, mobile app, or federated identity provider redirect).

## 4. Entry Conditions

- The customer holds an active identity record and at least one valid credential or authenticator.
- Authentication policies and adaptive assurance rules are published and available to the policy decision point.
- The channel or application is registered and authorized to accept login requests.
- Session and device trust evaluation services are available.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.customer_login_v1.login_request_intake` | Login Request Intake | `domain.identity_access` |
| `stage.customer_login_v1.primary_authentication_validation` | Primary Authentication Validation | `domain.identity_access` |
| `stage.customer_login_v1.session_device_context_evaluation` | Session and Device Context Evaluation | `domain.identity_access` |
| `stage.customer_login_v1.assurance_escalation_decision` | Assurance Escalation Decision | `domain.identity_access` |
| `stage.customer_login_v1.step_up_challenge_execution` | Step-Up Challenge Execution | `domain.identity_access` |
| `stage.customer_login_v1.session_establishment` | Session Establishment | `domain.identity_access` |
| `stage.customer_login_v1.permission_evaluation_and_release` | Permission Evaluation and Release | `domain.identity_access` |

## 6. Stage-by-Stage Flow

### Stage: `stage.customer_login_v1.login_request_intake`

**Description:** The institution receives the login request, captures the initial identifier (username, email, device token, or federated assertion), and resolves the customer identity record to determine which authentication path applies.

**Capabilities activated:** `cap.authentication`, `cap.session_and_device_trust_management`

**Systems involved:** `sys.digital_login_service`, `sys.device_trust_service`

**Decision points:**
- Is the identifier resolvable to an active customer identity?
- Is the account in a locked, suspended, or restricted state?
- Does the channel/application qualify for this authentication path?

**Risks:** `risk.credential_based_unauthorized_login`

**Controls:** `ctrl.centralized_login_policy`

**Evidence generated:** Login request receipt, identifier resolution outcome, channel and device metadata capture, request timestamp

**Handoff:** If the identifier resolves to a valid, non-locked identity, route to primary authentication validation. If locked or unresolvable, route to error or recovery path.

### Stage: `stage.customer_login_v1.primary_authentication_validation`

**Description:** The authentication service retrieves enrolled credential metadata and validates the presented credential (password, biometric, passkey, or OTP) against the stored reference.

**Capabilities activated:** `cap.authentication`, `cap.credential_and_authenticator_management`

**Systems involved:** `sys.authenticator_validation_service`, `sys.digital_login_service`

**Decision points:**
- Does the presented credential match the enrolled reference?
- Has the credential expired or been revoked?
- Has the maximum attempt threshold been reached?

**Risks:** `risk.credential_based_unauthorized_login`

**Controls:** `ctrl.centralized_login_policy`

**Evidence generated:** Credential validation result, authenticator type used, attempt count, validation timestamp

**Handoff:** If credential validation succeeds, proceed to session/device context evaluation. If failed, increment lockout counter and return error or route to recovery.

### Stage: `stage.customer_login_v1.session_device_context_evaluation`

**Description:** The session and device trust services evaluate contextual signals — device fingerprint, geolocation, IP reputation, login velocity, behavioral patterns, and prior session history — to produce a trust and risk context for the authentication event.

**Capabilities activated:** `cap.session_and_device_trust_management`, `cap.access_event_monitoring_and_anomaly_response`

**Systems involved:** `sys.device_trust_service`, `sys.session_service`

**Decision points:**
- Is the device recognized and trusted?
- Are there anomalous signals (new geography, impossible travel, velocity spikes)?
- What is the composite trust score for this session context?

**Risks:** `risk.overtrusted_device_session_context`

**Controls:** `ctrl.device_session_trust_evaluation`

**Evidence generated:** Device trust evaluation result, contextual signal summary, composite trust/risk score, anomaly flags

**Handoff:** Pass the trust/risk context to the assurance escalation decision stage.

### Stage: `stage.customer_login_v1.assurance_escalation_decision`

**Description:** The policy decision point evaluates whether the current authentication assurance level, combined with the session/device context, meets the required threshold — or whether step-up authentication is required.

**Capabilities activated:** `cap.access_policy_decisioning`, `cap.step_up_and_adaptive_authentication`

**Systems involved:** `sys.policy_decision_point`

**Decision points:**
- Does the current assurance level meet the policy threshold for the requested access?
- Do contextual risk signals require escalation regardless of credential strength?
- Which step-up challenge type is appropriate for the risk level?

**Risks:** `risk.inconsistent_adaptive_assurance`

**Controls:** `ctrl.adaptive_assurance_escalation`

**Evidence generated:** Policy evaluation decision, assurance level determination, escalation trigger reason (if applicable), policy version reference

**Handoff:** If assurance is sufficient, skip to session establishment. If escalation is required, route to step-up challenge execution.

### Stage: `stage.customer_login_v1.step_up_challenge_execution`

**Description:** When the assurance escalation decision requires it, the step-up challenge service presents and validates an additional authentication factor (SMS OTP, push notification, biometric, hardware token, or out-of-band verification).

**Capabilities activated:** `cap.step_up_and_adaptive_authentication`, `cap.credential_and_authenticator_management`

**Systems involved:** `sys.step_up_challenge_service`, `sys.authenticator_validation_service`

**Decision points:**
- Did the customer complete the step-up challenge successfully?
- Has the challenge timed out or been abandoned?
- Has the maximum step-up attempt threshold been reached?

**Risks:** `risk.inconsistent_adaptive_assurance`, `risk.credential_based_unauthorized_login`

**Controls:** `ctrl.adaptive_assurance_escalation`, `ctrl.centralized_login_policy`

**Evidence generated:** Step-up challenge type, challenge delivery confirmation, challenge response result, completion timestamp

**Handoff:** If the step-up challenge succeeds, proceed to session establishment. If failed or abandoned, terminate the login or route to recovery.

### Stage: `stage.customer_login_v1.session_establishment`

**Description:** Upon successful authentication (with or without step-up), the session service creates a governed session bound to the customer identity, assurance level, device context, and policy-determined lifetime and scope.

**Capabilities activated:** `cap.session_and_device_trust_management`, `cap.access_event_monitoring_and_anomaly_response`

**Systems involved:** `sys.session_service`, `sys.device_trust_service`

**Decision points:**
- What session lifetime, scope, and trust level are appropriate based on the authentication outcome?
- Should existing sessions on other devices be retained, demoted, or terminated?
- Is session binding (device, IP, fingerprint) required?

**Risks:** `risk.overtrusted_device_session_context`, `risk.over_entitled_authenticated_session`

**Controls:** `ctrl.session_binding_and_rotation`, `ctrl.device_session_trust_evaluation`

**Evidence generated:** Session token issuance record, session metadata (lifetime, scope, trust level, binding), device association record, prior session disposition

**Handoff:** Pass the established session context to permission evaluation and release.

### Stage: `stage.customer_login_v1.permission_evaluation_and_release`

**Description:** The authorization service resolves the customer's entitlements, roles, and permitted actions within the scope of the established session, and releases runtime permissions to the requesting channel or application.

**Capabilities activated:** `cap.authorization_and_entitlements`, `cap.access_policy_decisioning`, `cap.access_event_monitoring_and_anomaly_response`

**Systems involved:** `sys.entitlement_registry`, `sys.policy_decision_point`, `sys.session_service`

**Decision points:**
- What entitlements and roles apply to this customer in this session context?
- Are there any conditional or restricted permissions based on assurance level or device trust?
- Should any permissions be deferred until a sensitive-action step-up is triggered?

**Risks:** `risk.over_entitled_authenticated_session`, `risk.insufficient_access_telemetry`

**Controls:** `ctrl.centralized_authorization_policy`, `ctrl.identity_access_telemetry_coverage`

**Evidence generated:** Entitlement evaluation result, permission set released, conditional or deferred permissions, authorization policy version reference

**Handoff:** Process complete; the customer enters an authenticated, permission-scoped session. Ongoing session monitoring begins.

## 7. Decision Points

Key decisions across the process:
- Identifier resolvable vs unresolvable or locked
- Credential validation success vs failure
- Device/session context trusted vs anomalous
- Assurance sufficient vs step-up required
- Step-up challenge success vs failure/abandonment
- Session scope and lifetime determination
- Permission set evaluation and conditional release

## 8. Alternate Outcomes

- Step-up authentication required due to elevated risk signals or new device
- Federated login redirects through an external identity provider before returning to the institution's authorization layer
- Login blocked due to account lockout after repeated credential failures
- Session demoted or restricted due to low device trust score
- Deferred permissions requiring sensitive-action step-up later in the session

## 9. Risks (process-level)

- Credential stuffing or brute-force attacks against the login entry point
- Over-trusted device or session context allowing inappropriate access
- Inconsistent adaptive assurance decisions across channels or policies
- Over-entitled sessions granting permissions beyond what the assurance level warrants
- Insufficient telemetry making post-incident investigation difficult
- Session fixation or hijacking after establishment

## 10. Controls (process-level)

- Centralized login policy governing all channels and entry points
- Device and session trust evaluation at every login
- Adaptive assurance escalation based on published risk policy
- Session binding, rotation, and lifetime governance
- Centralized authorization policy for entitlement release
- End-to-end identity and access telemetry coverage

## 11. Evidence / Audit Considerations

This process generates a high-density audit trail:
- login request receipt and identifier resolution
- credential validation result and authenticator type
- device trust evaluation and contextual signals
- policy evaluation and assurance escalation decision
- step-up challenge delivery and result (when applicable)
- session establishment metadata and binding
- entitlement evaluation and permission release
- ongoing session monitoring events

## 12. Systems Involved (summary)

- digital login service
- authenticator validation service
- device trust service
- policy decision point
- step-up challenge service
- session service
- entitlement registry

## 13. Provider Participation

Representative providers often adjacent to this process:
- Ping Identity, Okta, ForgeRock, Microsoft Entra ID, Duo Security, Transmit Security, Iovation (TransUnion), BioCatch

## 14. Open Questions

- Should federated login be modeled as a separate process variant or a stage branch within this process?
- How should session demotion and re-escalation be modeled when device trust degrades mid-session?
- Should the permission evaluation stage be decomposed further to separate role resolution from entitlement release?

## 15. Provenance

This process represents the most frequently executed Identity & Access domain process. It is the operational spine that connects credential validation, adaptive assurance, session governance, and runtime authorization into a single controlled sequence. Every authenticated digital interaction depends on the integrity of this process.
