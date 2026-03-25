---
id: cap.access_identity_establishment
type: capability
name: Access Identity Establishment
domain_id: domain.identity_access
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Access Identity Establishment

## Definition

Access Identity Establishment is the enduring business ability to create, bind, maintain, and govern the access identities, identifiers, and trust anchors that allow a verified party to authenticate and interact with the institution's systems and services. It covers the full lifecycle of the access identity from creation through suspension, reinstatement, and deprovisioning. It is distinct from the business customer record and from the proofing that precedes it.

## Business Outcome

Ensure that every party who needs to authenticate has a well-governed, uniquely identifiable access identity that is correctly linked to its verified source, properly state-managed, and cleanly deprovisioned when no longer needed.

## Scope

### Included

- Creating access identities after successful identity proofing or equivalent trust establishment
- Assigning unique identifiers and binding them to verified party context
- Linking access identities to customer records, employee records, or partner records as appropriate
- Managing access identity state transitions such as active, suspended, reinstated, and deprovisioned
- Merging or relinking access identities when duplicates are discovered or relationships change
- Recording all identity state changes with audit context

### Excluded

- Identity proofing and verification (cap.identity_proofing_execution)
- Credential and authenticator issuance (cap.credential_and_authenticator_management)
- Runtime authentication (cap.authentication)
- Customer record creation and maintenance (domain.customer)
- Entitlement and authorization policy (cap.entitlement_and_authorization_policy_management)

## Dependencies

### Upstream

- cap.identity_proofing_execution supplies proofing results and confidence
- domain.customer supplies customer context for linking
- domain.hr_workforce supplies employee context for workforce identities
- domain.orchestration_control may coordinate provisioning flows

### Downstream

- cap.credential_and_authenticator_management binds credentials to the access identity
- cap.authentication uses the access identity at login
- cap.entitlement_and_authorization_policy_management assigns permissions to the access identity
- cap.session_and_device_trust_management establishes sessions for the access identity
- domain.customer and domain.accounts depend on active access identities for service delivery

## Value Streams

- vs.identity_assurance
- vs.access_identity_lifecycle
- vs.trusted_customer_relationship
- vs.employee_access_enablement

## Journeys

- journey.verify_identity_for_onboarding
- journey.create_customer_access_identity
- journey.employee_access_request_and_approval
- journey.reinstate_access_identity
- journey.deprovision_access_after_customer_exit

## Processes

- proc.access_identity_creation
- proc.identity_linking_and_binding
- proc.access_identity_status_change
- proc.access_identity_reinstatement
- proc.access_identity_merge_or_relink

## Risks

- Access identity is misbound to the wrong verified party
- Duplicate access identities are created for the same party
- Identity linkage to customer or employee record is broken or stale
- Access identity remains active after the party has exited the relationship
- Identity state changes are not audited or governed

## Controls

- ctrl.access_identity_creation_preconditions
- ctrl.unique_access_identity_policy
- ctrl.identity_linking_governance
- ctrl.access_identity_state_model
- ctrl.access_identity_audit_trail
- ctrl.suspension_and_reinstatement_controls

## Systems and Providers

### Systems

- CIAM platform
- Identity directory
- Customer identity store
- Workforce identity store
- Access identity lifecycle service

### Providers

- Okta
- Auth0
- Ping Identity
- Microsoft Entra
- ForgeRock

## Open Questions

- Should workforce and customer access identity establishment be separate capabilities or remain unified?
- How should partner and third-party access identities be governed relative to customer and employee identities?
- What is the right boundary between access identity deprovisioning here and broader offboarding orchestration?

## Provenance

Strongly supported by NIST SP 800-63 digital identity guidelines which define identity lifecycle management as a core function. FFIEC guidance reinforces the need for governed identity lifecycle practices in financial institutions.
