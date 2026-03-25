---
id: cap.access_policy_decisioning
type: capability
name: Access Policy Decisioning
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Access Policy Decisioning

## Definition

Access Policy Decisioning is the enduring business ability to evaluate access requests at runtime against authorization policies, contextual signals, and entitlements to render allow, deny, or step-up decisions. It is the real-time decision engine that interprets the permission model, ingests contextual signals such as device posture, location, risk score, and time of access, and produces an access verdict for each request. It does not define entitlements; it consumes them.

## Business Outcome

Ensure that every access request is evaluated against current policy and context so that authorized access is granted without unnecessary friction, unauthorized access is blocked, and high-risk requests trigger appropriate step-up or denial, supporting zero-trust and adaptive access objectives.

## Scope

### Included

- Evaluating access requests against authorization policies at runtime
- Ingesting contextual signals including device, location, risk, and behavioral signals
- Rendering allow, deny, or step-up decisions
- Enforcing default-deny posture where no explicit grant exists
- Logging access decisions for audit and analytics
- Supporting policy-as-code evaluation models
- Handling policy exceptions and overrides within governed boundaries

### Excluded

- Defining or maintaining entitlements and permission models (cap.authorization_and_entitlements)
- Authentication at login time (cap.authentication)
- Session establishment or management (cap.session_and_device_trust_management)
- Provisioning or deprovisioning of access rights (cap.access_provisioning_and_deprovisioning)
- Risk scoring model development (domain.risk_analytics)

## Dependencies

### Upstream

- cap.authorization_and_entitlements supplies the entitlement and permission model consumed at decision time
- cap.authentication supplies the authenticated identity context
- cap.session_and_device_trust_management supplies session and device trust signals
- Risk and fraud signal services supply contextual risk inputs

### Downstream

- Policy enforcement points across applications and APIs enforce rendered decisions
- cap.access_activity_monitoring_and_anomaly_detection consumes decision logs
- cap.step_up_and_adaptive_authentication may be triggered by step-up decisions
- All protected resources depend on access decisions

## Value Streams

- vs.runtime_access_control
- vs.zero_trust_access
- vs.adaptive_risk_response
- vs.customer_digital_access

## Journeys

- journey.access_protected_resource
- journey.step_up_for_sensitive_action
- journey.deny_unauthorized_access_attempt
- journey.evaluate_context_for_adaptive_access
- journey.enforce_conditional_access_policy

## Processes

- proc.policy_evaluation_at_request_time
- proc.contextual_signal_ingestion
- proc.step_up_trigger_evaluation
- proc.access_decision_logging
- proc.policy_exception_handling

## Risks

- Policy bypass or misconfiguration leads to unauthorized access
- Stale policies enforced after business rules change
- Insufficient contextual signal coverage weakens adaptive decisions
- Overly permissive default decisions when signals are unavailable
- Decision latency impacts system availability or user experience

## Controls

- ctrl.policy_as_code_governance
- ctrl.default_deny_enforcement
- ctrl.decision_audit_logging
- ctrl.policy_change_review_and_approval
- ctrl.contextual_signal_validation
- ctrl.step_up_authentication_trigger_rules

## Systems and Providers

### Systems

- Policy decision point
- Policy information point
- Policy enforcement point
- Access decision log
- Contextual signal aggregator

### Providers

- Okta
- Microsoft Entra
- Ping Identity
- Axiomatics
- Styra

## Open Questions

- How should policy versioning and rollback be governed?
- What is the acceptable latency threshold for runtime policy evaluation?
- Should machine-to-machine and user access decisions follow the same policy architecture?

## Provenance

NIST SP 800-162 defines the ABAC architecture with distinct policy decision, information, and enforcement points, directly supporting this capability's scope. NIST SP 800-207 zero-trust architecture reinforces per-request evaluation. CISA Zero Trust Maturity Model emphasizes continuous, context-aware access decisions.
