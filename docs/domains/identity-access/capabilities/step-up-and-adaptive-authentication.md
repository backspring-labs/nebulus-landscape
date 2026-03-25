---
id: cap.step_up_and_adaptive_authentication
type: capability
name: Step-Up and Adaptive Authentication
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Step-Up and Adaptive Authentication

## Definition

Step-Up and Adaptive Authentication is the enduring business ability to apply additional or adaptive authentication challenges when the sensitivity of a requested action, a change in risk signals, or degradation of session trust requires stronger assurance than what was established at initial login. It is the capability that dynamically elevates trust in an already-authenticated session rather than performing initial authentication.

## Business Outcome

Ensure that high-risk actions and sensitive changes are protected by authentication assurance proportional to their risk, while avoiding unnecessary friction for routine actions, and that the institution can respond dynamically to changing trust signals during an active session.

## Scope

### Included

- Evaluating whether a requested action requires step-up authentication based on policy
- Executing adaptive authentication challenges based on risk signals, anomaly detection, or trust degradation
- Requiring reauthentication for high-risk actions such as payment approvals, contact information changes, or authenticator replacement
- Handling challenge failures including escalation and denial
- Recording assurance elevation events for audit
- Maintaining consistency of step-up rules across channels

### Excluded

- Initial login authentication (cap.authentication)
- Credential and authenticator management (cap.credential_and_authenticator_management)
- Session lifecycle management (cap.session_and_device_trust_management)
- Fraud detection and investigation (domain.risk_fraud)
- Authorization and entitlement decisions (cap.entitlement_and_authorization_policy_management)

## Dependencies

### Upstream

- cap.authentication establishes the initial authentication context
- cap.session_and_device_trust_management supplies session and device trust state
- cap.credential_and_authenticator_management supplies the authenticator inventory available for challenge
- Risk signal sources supply anomaly and context signals
- domain.orchestration_control may coordinate step-up flows

### Downstream

- cap.session_and_device_trust_management updates session trust after successful step-up
- Domain capabilities that trigger sensitive actions consume step-up assurance
- cap.access_activity_monitoring_and_anomaly_detection consumes step-up events
- domain.payments, domain.customer, and domain.accounts are common consumers of step-up protection

## Value Streams

- vs.runtime_access_trust
- vs.high_risk_action_protection
- vs.phishing_resistant_authentication
- vs.customer_record_maintenance

## Journeys

- journey.perform_step_up_for_sensitive_change
- journey.authorize_high_risk_payment_action
- journey.replace_lost_authenticator
- journey.update_contact_information
- journey.manage_data_sharing_consent

## Processes

- proc.step_up_challenge_evaluation
- proc.adaptive_authentication_execution
- proc.high_risk_action_reauthentication
- proc.challenge_failure_and_escalation
- proc.assurance_elevation_recording

## Risks

- High-risk actions proceed without appropriate step-up authentication
- Step-up logic is inconsistent across channels or action types
- Excessive step-up friction degrades the user experience for legitimate users
- Weak factors are accepted for sensitive action step-up
- Anomaly-based adaptive triggers are bypassed or miscalibrated

## Controls

- ctrl.step_up_policy_by_action_risk
- ctrl.adaptive_signal_threshold_governance
- ctrl.high_assurance_factor_requirements
- ctrl.challenge_failure_escalation
- ctrl.step_up_event_logging
- ctrl.channel_consistency_for_step_up_rules

## Systems and Providers

### Systems

- Adaptive authentication engine
- Risk signal aggregator
- Challenge orchestration service
- MFA verifier
- Trust event log

### Providers

- Okta
- Ping Identity
- Duo
- Microsoft Entra
- Auth0

## Open Questions

- Should adaptive authentication based on anomaly signals be modeled separately from policy-driven step-up for sensitive actions?
- How should step-up requirements be coordinated with transaction-level authorization controls in domain.payments?
- What is the right balance between step-up frequency and session trust duration?

## Provenance

Strongly supported by FFIEC authentication and access guidance which emphasizes risk-appropriate authentication including step-up for high-risk transactions. NIST SP 800-63B supports the concept of assurance level elevation. CISA phishing-resistant MFA guidance reinforces the use of strong factors for sensitive operations.
