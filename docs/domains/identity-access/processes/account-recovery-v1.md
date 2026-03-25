---
id: proc.account_recovery_v1
type: process
name: Account Recovery v1
journey_id: journey.recover_account_access
capability_ids:
  - cap.identity_recovery_and_reset_management
  - cap.credential_and_authenticator_management
  - cap.step_up_and_adaptive_authentication
  - cap.identity_proofing_execution
  - cap.session_and_device_trust_management
  - cap.access_event_monitoring_and_anomaly_response
  - cap.access_audit_and_attestation
status: draft
source_refs:
  - src.nist_sp800_63b4_authentication_lifecycle
  - src.ffiec_authentication_access_2021
last_reviewed: 2026-03-25
confidence: high
---

# Account Recovery v1

## 1. Objective

Operationally execute the controlled sequence required to restore account access after lockout, credential loss, lost authenticator, or suspected compromise — while enforcing assurance re-establishment, prior trust cleanup, and post-recovery monitoring proportional to the recovery scenario's risk.

## 2. Scope

This process covers:
- recovery request intake and initial context capture
- recovery scenario triage and risk classification
- reproofing and assurance re-establishment for sensitive recovery paths
- credential reset or authenticator rebinding
- cleanup of prior trust state (sessions, tokens, devices)
- reauthentication after recovery to confirm restored access
- recovery completion, evidence capture, and post-recovery monitoring

This process does **not** own:
- initial identity proofing or onboarding (`domain.customer`, `domain.identity_access` proofing capabilities)
- fraud investigation or case management (`domain.risk_fraud`)
- customer communication or notification delivery (`domain.channels_experience`)
- contact center agent authentication or authorization for support-assisted flows

## 3. Trigger

A customer or authorized support agent initiates a recovery action due to credential lockout, forgotten password, lost authenticator, device loss, or suspected account compromise.

## 4. Entry Conditions

- The customer identity record exists and is not permanently closed or terminated.
- At least one recovery path is configured and available for the customer's account type and risk tier.
- Recovery policy rules and reproofing requirements are published and available.
- The recovery workflow service and downstream credential/session services are operational.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.account_recovery_v1.recovery_intake` | Recovery Intake | `domain.identity_access` |
| `stage.account_recovery_v1.recovery_scenario_triage` | Recovery Scenario Triage | `domain.identity_access` |
| `stage.account_recovery_v1.reproof_and_assurance_reestablishment` | Reproof and Assurance Re-establishment | `domain.identity_access` |
| `stage.account_recovery_v1.reset_or_rebind_access_artifacts` | Reset or Rebind Access Artifacts | `domain.identity_access` |
| `stage.account_recovery_v1.cleanup_prior_trust_state` | Cleanup Prior Trust State | `domain.identity_access` |
| `stage.account_recovery_v1.reauthentication_after_recovery` | Reauthentication After Recovery | `domain.identity_access` |
| `stage.account_recovery_v1.recovery_completion_and_monitoring` | Recovery Completion and Monitoring | `domain.identity_access` |

## 6. Stage-by-Stage Flow

### Stage: `stage.account_recovery_v1.recovery_intake`

**Description:** The institution receives the recovery request, captures the initial context (channel, claimed identity, recovery reason), and validates that the customer identity exists and is eligible for recovery.

**Capabilities activated:** `cap.identity_recovery_and_reset_management`

**Systems involved:** `sys.recovery_workflow_service`

**Decision points:**
- Is the claimed identity resolvable to an existing customer record?
- Is the account in a state that permits recovery (not permanently closed or terminated)?
- Which recovery channel is in use (self-service, support-assisted, branch)?

**Risks:** `risk.weak_recovery_path`

**Controls:** `ctrl.recovery_assurance_by_risk_tier`

**Evidence generated:** Recovery request receipt, claimed identity resolution result, recovery channel and reason code, intake timestamp

**Handoff:** If the identity resolves and the account is eligible, route to recovery scenario triage. If ineligible, return error or route to support escalation.

### Stage: `stage.account_recovery_v1.recovery_scenario_triage`

**Description:** The recovery workflow classifies the recovery scenario by type and risk tier — distinguishing between simple password reset, authenticator replacement, device loss with active sessions, and suspected compromise — and determines the required assurance path.

**Capabilities activated:** `cap.identity_recovery_and_reset_management`, `cap.access_event_monitoring_and_anomaly_response`

**Systems involved:** `sys.recovery_workflow_service`

**Decision points:**
- What is the recovery scenario type (password reset, authenticator loss, device loss, compromise)?
- What risk tier does this scenario fall into?
- Does the scenario require reproofing, or is a lighter-weight recovery path sufficient?
- Is there evidence of active compromise requiring immediate session/token revocation?

**Risks:** `risk.inconsistent_recovery_treatment`, `risk.support_assisted_recovery_bypass`

**Controls:** `ctrl.recovery_assurance_by_risk_tier`

**Evidence generated:** Recovery scenario classification, risk tier assignment, required assurance path determination, compromise indicator flags

**Handoff:** If reproofing is required, route to reproof and assurance re-establishment. If a lighter-weight path is sufficient, route directly to reset/rebind. If compromise is indicated, trigger immediate session cleanup in parallel.

### Stage: `stage.account_recovery_v1.reproof_and_assurance_reestablishment`

**Description:** For sensitive recovery scenarios, the institution re-establishes identity assurance through reproofing — which may include knowledge-based verification, document verification, biometric reverification, or in-person identity confirmation proportional to the scenario risk.

**Capabilities activated:** `cap.identity_proofing_execution`, `cap.step_up_and_adaptive_authentication`, `cap.identity_recovery_and_reset_management`

**Systems involved:** `sys.identity_reverification_service`, `sys.recovery_workflow_service`

**Decision points:**
- Which reproofing methods are available and appropriate for this scenario?
- Did the reproofing evidence meet the required assurance threshold?
- Should manual review be triggered for ambiguous reproofing results?

**Risks:** `risk.weak_recovery_path`, `risk.weak_recovery_evidence`

**Controls:** `ctrl.reproofing_requirements_for_sensitive_recovery`

**Evidence generated:** Reproofing method used, verification result, assurance level re-established, manual review flag (if applicable), reproofing timestamp

**Handoff:** If assurance is re-established, proceed to reset/rebind access artifacts. If reproofing fails, route to manual review, escalation, or denial.

### Stage: `stage.account_recovery_v1.reset_or_rebind_access_artifacts`

**Description:** The credential or authenticator that was lost, compromised, or locked is reset, replaced, or rebound. This includes password reset, passkey re-enrollment, MFA device rebinding, or recovery code rotation.

**Capabilities activated:** `cap.credential_and_authenticator_management`, `cap.identity_recovery_and_reset_management`

**Systems involved:** `sys.credential_reset_service`, `sys.recovery_workflow_service`

**Decision points:**
- Which credential or authenticator needs to be reset or replaced?
- Are there dependent artifacts that must be rotated simultaneously (e.g., recovery codes after authenticator rebind)?
- Was the reset/rebind operation confirmed successfully?

**Risks:** `risk.weak_recovery_path`, `risk.support_assisted_recovery_bypass`

**Controls:** `ctrl.factor_replacement_step_up_controls`

**Evidence generated:** Credential/authenticator reset confirmation, artifact type replaced, dependent artifact rotation record, reset timestamp

**Handoff:** Proceed to cleanup of prior trust state.

### Stage: `stage.account_recovery_v1.cleanup_prior_trust_state`

**Description:** All prior trust state that may be compromised or stale is cleaned up — including active sessions on other devices, refresh tokens, API tokens, delegated access grants, and device trust bindings associated with the lost or compromised factor.

**Capabilities activated:** `cap.session_and_device_trust_management`, `cap.credential_and_authenticator_management`

**Systems involved:** `sys.global_session_token_termination_service`, `sys.session_service`

**Decision points:**
- Which sessions, tokens, and device bindings are associated with the compromised or replaced artifact?
- Should all sessions be terminated or only those on unrecognized devices?
- Are there delegated or third-party access tokens that must be revoked?

**Risks:** `risk.persistent_prior_trust_state`

**Controls:** `ctrl.post_recovery_session_cleanup`

**Evidence generated:** Session termination record, token revocation record, device binding cleanup record, scope of cleanup performed

**Handoff:** Proceed to reauthentication after recovery.

### Stage: `stage.account_recovery_v1.reauthentication_after_recovery`

**Description:** The customer authenticates using the newly reset or rebound credential/authenticator to confirm that the recovery was successful and that access has been properly restored.

**Capabilities activated:** `cap.authentication`, `cap.session_and_device_trust_management`

**Systems involved:** `sys.session_service`, `sys.recovery_workflow_service`

**Decision points:**
- Did the customer successfully authenticate with the new credential/authenticator?
- Should any elevated monitoring or restrictions apply to the first post-recovery session?

**Risks:** `risk.weak_recovery_path`

**Controls:** `ctrl.recovery_assurance_by_risk_tier`

**Evidence generated:** Post-recovery authentication result, session establishment record, elevated monitoring flag (if applicable)

**Handoff:** If reauthentication succeeds, proceed to recovery completion and monitoring. If failed, route back to reset/rebind or escalation.

### Stage: `stage.account_recovery_v1.recovery_completion_and_monitoring`

**Description:** The recovery workflow is closed, evidence is captured for audit, and post-recovery monitoring rules are activated to detect any anomalous activity in the period following recovery.

**Capabilities activated:** `cap.access_event_monitoring_and_anomaly_response`, `cap.access_audit_and_attestation`, `cap.identity_recovery_and_reset_management`

**Systems involved:** `sys.access_governance_evidence_repository`, `sys.recovery_workflow_service`

**Decision points:**
- Is the recovery case complete with all required evidence captured?
- What post-recovery monitoring duration and sensitivity level should apply?
- Should the customer be notified of the completed recovery through an out-of-band channel?

**Risks:** `risk.weak_recovery_evidence`

**Controls:** `ctrl.recovery_event_audit_trail`, `ctrl.post_recovery_monitoring_controls`

**Evidence generated:** Recovery case closure record, full recovery evidence package, post-recovery monitoring activation record, customer notification record

**Handoff:** Process complete; the customer re-enters normal authenticated access. Post-recovery monitoring runs for the configured observation period.

## 7. Decision Points

Key decisions across the process:
- Identity resolvable and eligible for recovery vs ineligible
- Recovery scenario type and risk tier classification
- Reproofing required vs lighter-weight recovery path
- Reproofing evidence sufficient vs insufficient
- Credential/authenticator reset success vs failure
- Scope of prior trust state cleanup
- Post-recovery reauthentication success vs failure
- Post-recovery monitoring duration and sensitivity

## 8. Alternate Outcomes

- Simple password reset without reproofing for low-risk scenarios
- Support-assisted recovery with manual identity verification
- Recovery denied due to failed reproofing
- Compromise-triggered recovery with immediate session and token revocation
- Recovery paused pending manual review of ambiguous reproofing evidence
- Branch-based recovery requiring in-person identity confirmation

## 9. Risks (process-level)

- Weak recovery paths that allow an attacker to take over an account
- Support-assisted recovery bypassing self-service assurance controls
- Persistent prior trust state (sessions, tokens) surviving recovery and remaining usable by an attacker
- Inconsistent recovery treatment across channels or scenarios
- Weak or incomplete evidence making post-incident investigation difficult
- Social engineering of support staff during assisted recovery flows

## 10. Controls (process-level)

- Recovery assurance requirements scaled by risk tier
- Reproofing requirements for sensitive recovery scenarios
- Step-up controls for factor replacement
- Mandatory post-recovery session and token cleanup
- Recovery event audit trail with full evidence capture
- Post-recovery monitoring with elevated sensitivity

## 11. Evidence / Audit Considerations

This process generates a high-density audit trail:
- recovery request receipt and identity resolution
- recovery scenario classification and risk tier assignment
- reproofing method, result, and assurance level (when applicable)
- credential/authenticator reset confirmation and artifact type
- session, token, and device binding cleanup records
- post-recovery reauthentication result
- recovery case closure and full evidence package
- post-recovery monitoring activation

## 12. Systems Involved (summary)

- recovery workflow service
- identity reverification service
- credential reset service
- global session/token termination service
- session service
- access governance evidence repository

## 13. Provider Participation

Representative providers often adjacent to this process:
- Ping Identity, Okta, ForgeRock, Microsoft Entra ID, Transmit Security, Prove, Socure, LexisNexis Risk Solutions

## 14. Open Questions

- Should support-assisted recovery be modeled as a separate process variant with distinct controls?
- How should compromise-triggered recovery interact with the fraud domain's incident management process?
- Should reproofing method selection be governed by a separate policy artifact or embedded in the recovery workflow?
- How should recovery of delegated or proxy access be handled when the primary account holder recovers?

## 15. Provenance

This process is the most risk-sensitive Identity & Access domain process because a compromised recovery path is functionally equivalent to a full account takeover. The assurance re-establishment, prior trust cleanup, and post-recovery monitoring stages are what differentiate a governed recovery from a simple password reset.
