---
id: journey.deprovision_access_after_customer_exit
type: journey
name: Deprovision Access After Customer Exit
domain_ids:
  - domain.identity_access
  - domain.customer
  - domain.accounts
  - domain.servicing
capability_ids:
  - cap.post_exit_access_governance
  - cap.access_provisioning_and_deprovisioning
  - cap.session_and_device_trust_management
  - cap.credential_and_authenticator_management
  - cap.delegated_and_proxy_access_management
  - cap.consented_access_token_management
  - cap.access_audit_and_attestation
value_stream_id: vs.customer_exit_control
process_id: proc.deprovision_access_after_customer_exit_v1
status: draft
source_refs:
  - src.cisa_icam_reference_architecture
  - src.ffiec_authentication_access_2021
last_reviewed: 2026-03-25
confidence: high
---

# Deprovision Access After Customer Exit

## 1. Objective

Remove or constrain all access entitlements, credentials, sessions, delegated access relationships, and consented third-party tokens associated with a customer whose relationship with the institution has ended or become restricted, ensuring no residual access pathways remain active beyond what is required by policy or regulation.

## 2. Primary Actor

Institution operations — this journey is system- or operations-initiated in response to a customer exit or restriction event from the Customer domain, not directly initiated by the exiting customer.

## 3. Trigger

A customer lifecycle state change event is received indicating that the customer relationship has exited (account closure, relationship termination) or become restricted (suspension, regulatory hold) and access deprovisioning is required.

## 4. Preconditions

- The customer exit or restriction event has been confirmed by the Customer domain.
- The customer's access inventory (credentials, sessions, entitlements, delegated relationships, third-party tokens) is resolvable from the identity and access records.
- Deprovisioning policies — including retention requirements, grace periods, and exception handling rules — are published and available.
- Downstream systems and third-party token providers are reachable for revocation.

## 5. Happy Path

### Stage 1 — Exit event reception and access inventory resolution
The Identity & Access domain receives the customer exit or restriction event. The system resolves the complete access inventory for the customer: active sessions, credentials, authenticators, entitlements, delegated access grants, and consented third-party access tokens.

**Capabilities activated:** `cap.post_exit_access_governance`, `cap.access_provisioning_and_deprovisioning`

### Stage 2 — Active session termination
All active sessions for the customer are immediately terminated across all channels and devices. Device trust bindings are revoked.

**Capabilities activated:** `cap.session_and_device_trust_management`

### Stage 3 — Credential and authenticator revocation
All credentials (passwords, passkeys, security keys) and enrolled authenticators (devices, biometric templates) are revoked or disabled. Credential metadata is retained per audit policy.

**Capabilities activated:** `cap.credential_and_authenticator_management`

### Stage 4 — Delegated and proxy access cleanup
Any delegated access relationships where the exiting customer is either grantor or grantee are identified and resolved. Delegate sessions are terminated. Proxy access grants are revoked or reassigned per policy.

**Capabilities activated:** `cap.delegated_and_proxy_access_management`

### Stage 5 — Third-party consented access token revocation
All consented access tokens (OAuth tokens, open banking consents, API keys) granted to or by the customer are revoked. Third-party providers are notified where applicable.

**Capabilities activated:** `cap.consented_access_token_management`

### Stage 6 — Entitlement removal and access record finalization
Remaining entitlements and role assignments are removed. The customer's access record is transitioned to a post-exit state with appropriate retention metadata. A comprehensive audit record of all deprovisioning actions is finalized.

**Capabilities activated:** `cap.access_provisioning_and_deprovisioning`, `cap.access_audit_and_attestation`, `cap.post_exit_access_governance`

## 6. Alternate Paths

- Restricted (not fully exited) customer receives constrained access rather than full revocation — read-only access to statements, limited self-service.
- Regulatory or legal hold requires certain access records to be preserved but all active access to be disabled.
- Grace period policy allows temporary limited access for a defined period after exit (e.g., statement download window).
- Customer has active joint or co-owner relationships that require selective deprovisioning rather than full cleanup.
- Third-party token revocation fails for an external provider, requiring retry or exception handling.
- Customer reinstatement occurs during the deprovisioning window, requiring process reversal.

## 7. Failure / Exception Paths

- Access inventory resolution is incomplete — orphaned credentials or tokens are discovered after initial deprovisioning.
- Session termination fails for a specific channel or device, requiring manual intervention.
- Third-party token provider is unreachable, leaving consented access active beyond intended revocation time.
- Delegated access relationships span multiple customers, creating complex dependency resolution.
- Audit record generation fails, creating a compliance gap in the deprovisioning evidence chain.
- Post-exit access review discovers residual active pathways that were missed during automated deprovisioning.

## 8. Related Domains

- `domain.identity_access` — owns the deprovisioning lifecycle, credential revocation, and access cleanup
- `domain.customer` — provides the exit or restriction trigger event and customer lifecycle state
- `domain.accounts` — provides account closure context that may influence deprovisioning scope and timing
- `domain.servicing` — may own case management for exception handling and manual deprovisioning tasks

## 9. Related Capabilities

- `cap.post_exit_access_governance`
- `cap.access_provisioning_and_deprovisioning`
- `cap.session_and_device_trust_management`
- `cap.credential_and_authenticator_management`
- `cap.delegated_and_proxy_access_management`
- `cap.consented_access_token_management`
- `cap.access_audit_and_attestation`

## 10. Value Stream Context

This journey is the primary realization of `vs.customer_exit_control` from the Identity & Access perspective. It ensures that the institution's access perimeter contracts cleanly when a customer relationship ends, preventing residual access that could become a security or compliance liability. It is tightly coupled with the Customer domain's exit journey but is owned by Identity & Access because the access cleanup is an I&A-domain concern.

## 11. Supporting Process Candidates

- `proc.deprovision_access_after_customer_exit_v1`
- `proc.exit_event_reception`
- `proc.access_inventory_resolution`
- `proc.session_termination_sweep`
- `proc.credential_revocation_sweep`
- `proc.delegated_access_resolution`
- `proc.third_party_token_revocation`
- `proc.entitlement_removal`
- `proc.post_exit_access_record_finalization`
- `proc.deprovisioning_exception_handling`

## 12. Key Risks and Controls Encountered

### Key risks
- Residual active access after customer exit creates security exposure
- Incomplete access inventory misses orphaned credentials or tokens
- Third-party token revocation delays leave consented access active
- Delegated access complexity leads to partial cleanup
- Grace period abuse allows unauthorized activity during transition
- Audit trail gaps create compliance exposure during examinations

### Key controls
- Comprehensive access inventory resolution before deprovisioning begins
- Automated session termination across all channels
- Credential and authenticator revocation with confirmation
- Third-party token revocation with retry and escalation
- Delegated access dependency resolution rules
- Post-deprovisioning access review and sweep
- Grace period activity monitoring and restriction enforcement
- Complete audit trail with evidence of each deprovisioning action
- Periodic orphaned access detection and cleanup

## 13. Typical Systems and Providers Involved

### Typical systems
- Identity governance and administration (IGA) platform
- Session management service
- Credential management service
- OAuth / consent management platform
- Open banking directory and API gateway
- Case management system (for exceptions)
- Audit and compliance reporting platform
- Event bus / integration layer (for exit event reception)

### Representative providers
- SailPoint
- Saviynt
- Ping Identity
- ForgeRock
- Okta
- Axway / Kong (API gateway and token management)
- Pega / Salesforce (case management)

## 14. Open Questions

- How should deprovisioning handle customers with active open banking consents that have independent regulatory timelines?
- Should the grace period access model be standardized across all exit types or vary by exit reason?
- Where does data retention policy enforcement (access logs, credential history) sit relative to the deprovisioning journey?
- How should reinstatement interact with deprovisioning — is it a reversal of this journey or a new provisioning journey?
- Should deprovisioning for deceased customers follow a distinct journey given the different evidence and authorization requirements?

## 15. Provenance

This journey is grounded in CISA ICAM Reference Architecture requirements for access lifecycle management and deprovisioning, FFIEC Authentication and Access guidance (2021) on access termination controls, and observed patterns from regulatory examination findings where residual access after customer exit is a recurring compliance gap. The journey represents the Identity & Access domain's response to the Customer domain's exit event and ensures clean access perimeter contraction.
