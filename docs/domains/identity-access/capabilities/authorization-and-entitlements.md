---
id: cap.authorization_and_entitlements
type: capability
name: Authorization and Entitlements
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Authorization and Entitlements

## Definition

Authorization and Entitlements is the enduring business ability to define, assign, maintain, and govern what authenticated parties are permitted to do across channels, applications, services, and protected resources. It establishes the permission model: roles, privileges, scopes, actions, constraints, and access bundles granted to access identities or classes of actors. It is not the runtime decision itself; it is the governed model of what rights exist and who holds them.

## Business Outcome

Provide a durable, governable permission structure so the institution can grant the right level of access to the right parties, distinguish between standard and high-risk permissions, enforce least-privilege and separation-of-duties expectations, and align customer, employee, partner, and delegated access with policy and business need.

## Scope

### Included

- Defining permission models, roles, scopes, and entitlements
- Assigning entitlements to access identities, user classes, or delegated actors
- Maintaining access bundles and entitlement hierarchies
- Governing permission inheritance and exceptions
- Managing resource/action mappings
- Supporting contextual entitlement constraints where pre-modeled
- Governing privileged versus standard access classes
- Maintaining entitlement metadata, ownership, and review posture

### Excluded

- Runtime allow/deny/challenge evaluation (cap.access_policy_decisioning)
- Login-time authentication (cap.authentication)
- Proofing, credential, or session trust activities from Batch 1
- Business relationship ownership or customer-authorized relationship semantics (domain.customer)
- Generic workflow approval machinery (domain.orchestration_control)

## Dependencies

### Upstream

- cap.access_identity_establishment supplies the access identities that hold entitlements
- cap.authentication supplies the authenticated context under which entitlements are evaluated
- cap.session_and_device_trust_management supplies session and device trust signals
- domain.customer may provide relationship context
- Organizational role or administrative source systems supply role inputs

### Downstream

- cap.access_policy_decisioning consumes entitlements for runtime access decisions
- cap.access_provisioning_and_deprovisioning fulfills entitlement grants and removals
- cap.delegated_and_proxy_access_management consumes delegated entitlement definitions
- cap.privileged_access_governance (future batch) consumes privileged entitlement classifications
- All downstream domains relying on access rights

## Value Streams

- vs.access_governance
- vs.runtime_access_control
- vs.employee_access_enablement
- vs.delegated_access_enablement

## Journeys

- journey.grant_role_or_permission
- journey.review_access_attestation
- journey.authorize_privileged_admin_action
- journey.assign_delegated_access_rights
- journey.update_service_or_account_permissions

## Processes

- proc.entitlement_definition_and_maintenance
- proc.role_assignment
- proc.exception_entitlement_grant
- proc.entitlement_review_and_cleanup
- proc.permission_model_change_control

## Risks

- Overbroad permissions granted beyond business need
- Stale entitlements persist after role or relationship changes
- Entitlement models inconsistent across systems and applications
- High-risk access bundled with routine access through inheritance
- Exception grants ungoverned or lacking expiry
- Inheritance creates hidden excess privilege

## Controls

- ctrl.least_privilege_entitlement_design
- ctrl.separation_of_duties_rules
- ctrl.entitlement_owner_accountability
- ctrl.exception_access_approval_controls
- ctrl.periodic_entitlement_review
- ctrl.permission_model_change_governance

## Systems and Providers

### Systems

- Authorization administration service
- Entitlement registry
- Role/scope catalog
- Access governance platform
- Resource-permission mapping repository

### Providers

- Okta
- Microsoft Entra
- Ping Identity
- SailPoint
- CyberArk

## Open Questions

- Should customer-facing and workforce/admin entitlements later split into distinct sub-capabilities?
- How granular should scopes become before they warrant their own governance layer?
- Should inheritance be modeled as a first-class construct with its own lifecycle?

## Provenance

NIST ABAC guidance (SP 800-162) distinguishes policy inputs and runtime decisioning, supporting keeping entitlement definition separate from request-time decisions. ICAM and zero-trust patterns reinforce governed permission models as a foundation for access control.
