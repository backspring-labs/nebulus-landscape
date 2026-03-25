---
id: proc.deprovision_access_after_customer_exit_v1
type: process
name: Deprovision Access After Customer Exit v1
journey_id: journey.deprovision_access_after_customer_exit
capability_ids:
  - cap.access_provisioning_and_deprovisioning
  - cap.session_and_device_trust_management
  - cap.credential_and_authenticator_management
  - cap.consented_access_token_management
  - cap.delegated_and_proxy_access_management
  - cap.post_exit_access_governance
  - cap.access_audit_and_attestation
status: draft
source_refs:
  - src.cisa_icam_reference_architecture
  - src.ffiec_authentication_access_2021
last_reviewed: 2026-03-25
confidence: high
---

# Deprovision Access After Customer Exit v1

## 1. Objective

Operationally execute the controlled sequence required to clean up all customer access state — sessions, credentials, authenticators, tokens, delegated grants, and entitlements — after an authoritative customer exit or restriction trigger, while ensuring cross-system consistency, evidence capture, and governed exception handling.

## 2. Scope

This process covers:
- exit trigger intake and validation from an authoritative source
- cleanup scope determination across all access artifacts
- session and runtime state revocation (active sessions, refresh tokens, API tokens)
- access artifact cleanup (credentials, authenticators, entitlements, delegated grants, third-party tokens)
- cleanup confirmation and cross-system reconciliation
- evidence capture, exception handling, and closure

This process does **not** own:
- the customer exit decision itself (`domain.customer` — `proc.customer_exit_v1`)
- account closure, ledger finalization, or regulatory hold processing (`domain.accounts`)
- fraud-triggered access suspension (immediate response owned by `domain.risk_fraud`)
- channel deactivation or notification delivery (`domain.channels_experience`)

## 3. Trigger

An authoritative exit or restriction event is received from the customer lifecycle domain — such as voluntary account closure, involuntary termination, regulatory restriction, or death notification — indicating that the customer's access state must be cleaned up.

## 4. Entry Conditions

- An authoritative exit or restriction trigger has been issued by the customer lifecycle domain.
- The customer identity record exists and has not already been fully deprovisioned.
- The cleanup orchestrator and downstream revocation services are operational.
- Access artifact registries (credentials, tokens, sessions, entitlements) are queryable.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.deprovision_access_after_customer_exit_v1.exit_trigger_intake` | Exit Trigger Intake | `domain.identity_access` |
| `stage.deprovision_access_after_customer_exit_v1.cleanup_scope_determination` | Cleanup Scope Determination | `domain.identity_access` |
| `stage.deprovision_access_after_customer_exit_v1.session_and_runtime_state_revocation` | Session and Runtime State Revocation | `domain.identity_access` |
| `stage.deprovision_access_after_customer_exit_v1.access_artifact_cleanup` | Access Artifact Cleanup | `domain.identity_access` |
| `stage.deprovision_access_after_customer_exit_v1.cleanup_confirmation_and_reconciliation` | Cleanup Confirmation and Reconciliation | `domain.identity_access` |
| `stage.deprovision_access_after_customer_exit_v1.evidence_and_exception_closure` | Evidence and Exception Closure | `domain.identity_access` |

## 6. Stage-by-Stage Flow

### Stage: `stage.deprovision_access_after_customer_exit_v1.exit_trigger_intake`

**Description:** The identity and access domain receives the authoritative exit or restriction trigger, validates its source and authority, and confirms that the trigger applies to a known customer identity with active access state.

**Capabilities activated:** `cap.access_provisioning_and_deprovisioning`, `cap.post_exit_access_governance`

**Systems involved:** `sys.post_exit_cleanup_orchestrator`

**Decision points:**
- Is the trigger from an authoritative source (customer lifecycle domain)?
- Is the trigger type recognized (voluntary closure, involuntary termination, regulatory restriction, death notification)?
- Does the customer identity have active access state requiring cleanup?

**Risks:** `risk.persistent_access_after_customer_exit`

**Controls:** `ctrl.authoritative_exit_trigger_validation`

**Evidence generated:** Exit trigger receipt, source and authority validation result, trigger type classification, customer identity confirmation, intake timestamp

**Handoff:** If the trigger is valid and the customer has active access state, proceed to cleanup scope determination. If invalid or no active state, log and close.

### Stage: `stage.deprovision_access_after_customer_exit_v1.cleanup_scope_determination`

**Description:** The cleanup orchestrator inventories the full scope of the customer's access state across all systems — active sessions, credentials, authenticators, entitlements, delegated access grants, third-party tokens, and device bindings — to produce a comprehensive cleanup manifest.

**Capabilities activated:** `cap.access_provisioning_and_deprovisioning`, `cap.post_exit_access_governance`

**Systems involved:** `sys.post_exit_cleanup_orchestrator`, `sys.deprovisioning_and_revocation_service`

**Decision points:**
- What access artifacts exist across all registered systems?
- Are there delegated or proxy access grants that depend on this customer's identity?
- Are there third-party (open banking, API) tokens that must be revoked?
- Are there any regulatory hold or retention requirements that affect cleanup timing?

**Risks:** `risk.incomplete_runtime_or_delegated_cleanup`, `risk.cross_system_cleanup_inconsistency`

**Controls:** `ctrl.exit_triggered_access_cleanup_rules`

**Evidence generated:** Cleanup scope manifest, inventory of access artifacts by type and system, delegated/proxy access dependencies, regulatory hold flags, retention requirement notes

**Handoff:** With the cleanup manifest assembled, proceed to session and runtime state revocation (immediate priority) and access artifact cleanup (parallel or sequential depending on dependencies).

### Stage: `stage.deprovision_access_after_customer_exit_v1.session_and_runtime_state_revocation`

**Description:** All active runtime state is immediately revoked — including active sessions across all channels and devices, refresh tokens, API access tokens, and any real-time authorization grants — to prevent continued access from the moment of exit trigger processing.

**Capabilities activated:** `cap.session_and_device_trust_management`, `cap.consented_access_token_management`

**Systems involved:** `sys.global_session_token_termination_service`, `sys.third_party_token_registry`

**Decision points:**
- Have all active sessions been identified and terminated?
- Have all refresh tokens and API tokens been revoked?
- Have all third-party (open banking) access tokens been revoked?
- Are there any sessions or tokens that could not be terminated (e.g., offline tokens with long expiry)?

**Risks:** `risk.persistent_access_after_customer_exit`, `risk.incomplete_runtime_or_delegated_cleanup`

**Controls:** `ctrl.global_session_token_and_authenticator_revocation`

**Evidence generated:** Session termination confirmations by channel/device, token revocation confirmations by type, third-party token revocation confirmations, unterminated artifact exceptions (if any)

**Handoff:** Proceed to access artifact cleanup. Any unterminated runtime artifacts are flagged for exception handling.

### Stage: `stage.deprovision_access_after_customer_exit_v1.access_artifact_cleanup`

**Description:** All persistent access artifacts are deactivated, revoked, or marked for deletion — including credentials (passwords, passkeys), authenticator bindings (MFA devices, biometric enrollments), entitlement assignments, delegated access grants, and device trust bindings.

**Capabilities activated:** `cap.credential_and_authenticator_management`, `cap.access_provisioning_and_deprovisioning`, `cap.delegated_and_proxy_access_management`

**Systems involved:** `sys.deprovisioning_and_revocation_service`, `sys.authenticator_binding_registry`

**Decision points:**
- Have all credentials been deactivated or revoked?
- Have all authenticator bindings been removed?
- Have all entitlement assignments been removed?
- Have all delegated or proxy access grants been revoked?
- Are there any artifacts subject to retention requirements that cannot be immediately deleted?

**Risks:** `risk.incomplete_runtime_or_delegated_cleanup`, `risk.cross_system_cleanup_inconsistency`

**Controls:** `ctrl.exit_triggered_access_cleanup_rules`, `ctrl.global_session_token_and_authenticator_revocation`

**Evidence generated:** Credential deactivation confirmations, authenticator revocation confirmations, entitlement removal confirmations, delegated access revocation confirmations, retention-held artifact records

**Handoff:** Proceed to cleanup confirmation and reconciliation.

### Stage: `stage.deprovision_access_after_customer_exit_v1.cleanup_confirmation_and_reconciliation`

**Description:** The cleanup orchestrator reconciles the cleanup manifest against the actual cleanup results across all systems to confirm completeness, identify any discrepancies, and flag unresolved items for exception handling.

**Capabilities activated:** `cap.post_exit_access_governance`, `cap.access_audit_and_attestation`

**Systems involved:** `sys.post_exit_cleanup_orchestrator`, `sys.access_governance_evidence_repository`

**Decision points:**
- Does the cleanup result match the cleanup manifest across all artifact types and systems?
- Are there any discrepancies (artifacts not confirmed as cleaned up)?
- Do discrepancies require immediate escalation or can they be handled as time-bound exceptions?

**Risks:** `risk.cross_system_cleanup_inconsistency`, `risk.weak_post_exit_cleanup_evidence`

**Controls:** `ctrl.cross_system_cleanup_reconciliation`

**Evidence generated:** Reconciliation result (match/discrepancy), discrepancy details by system and artifact type, escalation flags, exception records

**Handoff:** If reconciliation is clean, proceed to evidence and exception closure. If discrepancies exist, route exceptions and proceed to closure with open items.

### Stage: `stage.deprovision_access_after_customer_exit_v1.evidence_and_exception_closure`

**Description:** The complete evidence package is assembled, any open exceptions are assigned time-bound resolution targets and owners, and the cleanup case is closed with an auditable record linking back to the original exit trigger.

**Capabilities activated:** `cap.access_audit_and_attestation`, `cap.post_exit_access_governance`

**Systems involved:** `sys.access_governance_evidence_repository`, `sys.post_exit_cleanup_orchestrator`

**Decision points:**
- Is the evidence package complete for the closed cleanup items?
- Are all open exceptions assigned time-bound resolution targets?
- Should the cleanup case be closed or held open pending exception resolution?
- Does the exit type require notification to any external party (e.g., regulatory report)?

**Risks:** `risk.weak_post_exit_cleanup_evidence`, `risk.ungoverned_post_exit_exception`

**Controls:** `ctrl.time_bound_post_exit_exceptions`, `ctrl.post_exit_evidence_and_audit_trail`

**Evidence generated:** Complete cleanup evidence package, exception assignment records with time-bound targets, case closure record, exit trigger-to-cleanup linkage, external notification record (if applicable)

**Handoff:** Process complete; evidence is stored and linked to the exit trigger. Open exceptions are tracked to resolution independently.

## 7. Decision Points

Key decisions across the process:
- Exit trigger source validation and authority confirmation
- Cleanup scope determination across all artifact types and systems
- Immediate runtime revocation vs scheduled artifact cleanup
- Delegated and third-party access identification and revocation
- Reconciliation match vs discrepancy handling
- Exception assignment with time-bound targets
- Regulatory hold or retention impact on cleanup timing

## 8. Alternate Outcomes

- Partial cleanup due to system unavailability with exceptions tracked to resolution
- Regulatory hold preventing immediate deletion of certain artifacts (marked for deferred cleanup)
- Delegated access grants requiring notification to dependent parties before revocation
- Third-party (open banking) token revocation requiring coordination with external providers
- Exception escalation for artifacts that cannot be confirmed as cleaned up
- Re-trigger of cleanup after initial exception resolution

## 9. Risks (process-level)

- Persistent access surviving after customer exit due to incomplete cleanup
- Incomplete cleanup of runtime state (sessions, tokens) or delegated/proxy grants
- Cross-system cleanup inconsistency leaving orphaned artifacts
- Weak or incomplete evidence making post-exit audit difficult
- Ungoverned exceptions persisting without time-bound resolution
- Delayed cleanup due to system unavailability or coordination gaps

## 10. Controls (process-level)

- Authoritative exit trigger validation before cleanup begins
- Exit-triggered cleanup rules covering all artifact types
- Global session, token, and authenticator revocation
- Cross-system cleanup reconciliation against manifest
- Time-bound exception governance with assigned owners
- Post-exit evidence and audit trail with trigger-to-cleanup linkage

## 11. Evidence / Audit Considerations

This process generates a high-density audit trail:
- exit trigger receipt and authority validation
- cleanup scope manifest and artifact inventory
- session and token termination confirmations
- credential, authenticator, and entitlement revocation confirmations
- delegated and third-party access revocation confirmations
- reconciliation result and discrepancy details
- exception assignment and resolution records
- complete evidence package with exit trigger linkage

## 12. Systems Involved (summary)

- post-exit cleanup orchestrator
- deprovisioning and revocation service
- global session/token termination service
- authenticator binding registry
- third-party token registry
- access governance evidence repository

## 13. Provider Participation

Representative providers often adjacent to this process:
- SailPoint, Saviynt, CyberArk, Okta, Ping Identity, ForgeRock, Microsoft Entra ID, Axway (API gateway/token management)

## 14. Open Questions

- Should the cleanup orchestrator operate as an event-driven subscriber to the customer lifecycle domain or as a polled reconciliation process?
- How should regulatory holds and retention requirements be modeled when they affect cleanup timing?
- Should third-party token revocation be modeled as a distinct sub-process given the external coordination complexity?
- How should this process interact with the customer exit process to ensure no race conditions between account closure and access cleanup?

## 15. Provenance

This process is the most cross-system Identity & Access domain process because it must reach into every system that holds access state for a customer and confirm that cleanup is complete. The reconciliation and exception governance stages are what differentiate a governed deprovisioning process from a simple account disable.
