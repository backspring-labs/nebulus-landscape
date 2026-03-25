---
id: cap.delegated_and_proxy_access_management
type: capability
name: Delegated and Proxy Access Management
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: medium
---

# Delegated and Proxy Access Management

## Definition

Delegated and Proxy Access Management is the enduring business ability to manage access arrangements where one party acts on behalf of another. This includes power of attorney, guardianship, corporate delegation, third-party agent access, and other fiduciary or authorized representative relationships. It governs how delegated authority is established, scoped, exercised, reviewed, and revoked, ensuring that proxy actors operate within defined boundaries and that actions taken on behalf of others are distinguishable and auditable.

## Business Outcome

Enable legitimate delegation of access authority while maintaining clear accountability, scope limitation, and auditability so the institution can serve customers and partners through authorized representatives without creating ungoverned or opaque access pathways.

## Scope

### Included

- Establishing delegated access authority based on verified legal or business authorization
- Assigning and scoping proxy access rights for delegated actors
- Maintaining a registry of active delegated access arrangements
- Governing delegation chains and transitive authority
- Ensuring proxy actors maintain distinct identity and audit trails
- Reviewing and expiring delegated access on schedule or trigger
- Revoking delegated access upon authority change or termination

### Excluded

- Defining the underlying entitlement or permission model (cap.authorization_and_entitlements)
- Authenticating the delegated actor at login (cap.authentication)
- Business relationship ownership or customer onboarding (domain.customer)
- Legal or compliance determination of delegation authority (domain.compliance)
- Runtime access decisions for delegated actors (cap.access_policy_decisioning)

## Dependencies

### Upstream

- cap.access_identity_establishment supplies the access identities for both delegator and delegate
- cap.authorization_and_entitlements supplies the entitlements that can be delegated
- cap.authentication authenticates the delegated actor
- domain.customer or legal/compliance sources provide delegation authority documentation

### Downstream

- cap.access_policy_decisioning evaluates access requests from delegated actors
- cap.access_provisioning_and_deprovisioning provisions delegated access rights
- cap.access_activity_monitoring_and_anomaly_detection monitors delegated actor behavior
- Audit and compliance functions consume delegation event trails

## Value Streams

- vs.delegated_access_enablement
- vs.access_governance
- vs.customer_digital_access
- vs.fiduciary_access_trust

## Journeys

- journey.establish_delegated_access_authority
- journey.act_on_behalf_of_another_party
- journey.revoke_delegated_access
- journey.review_delegated_access_scope
- journey.transition_delegated_access_authority

## Processes

- proc.delegated_authority_verification
- proc.proxy_access_scope_assignment
- proc.delegated_access_review
- proc.delegated_access_revocation
- proc.delegation_chain_governance

## Risks

- Unauthorized delegation of access rights without proper authority
- Overbroad delegated scope beyond what authority permits
- Stale delegated access persists after authority expires or is revoked
- Delegation chains become opaque and difficult to audit
- Proxy actor identity confused with the delegating party in audit trails

## Controls

- ctrl.delegated_authority_verification_controls
- ctrl.delegated_scope_limitation
- ctrl.delegated_access_periodic_review
- ctrl.delegation_event_audit_trail
- ctrl.proxy_actor_distinct_identity_controls
- ctrl.delegation_expiry_enforcement

## Systems and Providers

### Systems

- Delegated access registry
- Proxy identity service
- Delegation authority verifier
- Delegated access audit log
- Delegation scope catalog

### Providers

- Okta
- Microsoft Entra
- SailPoint
- Saviynt

## Open Questions

- How should delegation authority documents (e.g., power of attorney) be digitally verified and linked?
- Should transitive delegation (delegate of a delegate) be permitted and if so under what constraints?
- How should delegation interact with privileged access governance?

## Provenance

FFIEC authentication and access guidance recognizes the need for governed representative access in financial services. CISA ICAM reference architecture includes delegation as a component of identity governance. The complexity of fiduciary and representative access in financial institutions warrants a dedicated capability with medium confidence pending further validation of scope boundaries.
