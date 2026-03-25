---
id: journey.recover_account_access
type: journey
name: Recover Account Access
domain_ids:
  - domain.identity_access
  - domain.channels_experience
  - domain.customer
  - domain.servicing
  - domain.risk_fraud
capability_ids:
  - cap.identity_recovery_and_reset_management
  - cap.identity_proofing_execution
  - cap.credential_and_authenticator_management
  - cap.authentication
  - cap.step_up_and_adaptive_authentication
  - cap.session_and_device_trust_management
  - cap.access_event_monitoring_and_anomaly_response
  - cap.access_audit_and_attestation
value_stream_id: vs.account_recovery
process_id: proc.account_recovery_v1
status: draft
source_refs:
  - src.nist_sp800_63b4_authentication_lifecycle
  - src.ffiec_authentication_access_2021
last_reviewed: 2026-03-25
confidence: high
---

# Recover Account Access

## 1. Objective

Restore a customer's access to digital banking after a credential loss, device loss, account lockout, or suspected compromise, while maintaining identity assurance and preventing unauthorized account takeover.

## 2. Primary Actor

An existing customer who is unable to authenticate through normal means due to forgotten credentials, lost or compromised device, account lockout, or suspected unauthorized access.

## 3. Trigger

The customer initiates a recovery flow from a supported entry point (forgot password link, locked-out help, lost device flow) or the institution initiates a forced recovery due to detected compromise.

## 4. Preconditions

- The customer has been previously onboarded and holds an active identity record.
- Recovery policies and identity re-proofing requirements are published and available.
- At least one recovery path (email, phone, knowledge-based, in-branch) is available for the customer.
- The account is not in a terminal lifecycle state (closed, permanently restricted).

## 5. Happy Path

### Stage 1 — Recovery initiation
The customer selects a recovery entry point (forgot password, locked out, lost device). The system identifies the customer identity and determines the applicable recovery policy based on the recovery reason and risk context.

**Capabilities activated:** `cap.identity_recovery_and_reset_management`, `cap.access_event_monitoring_and_anomaly_response`

### Stage 2 — Identity re-verification
The system challenges the customer to re-establish identity confidence through available recovery factors — secondary email, SMS OTP, security questions, or document-based re-proofing. The assurance level required is proportional to the risk of the recovery scenario.

**Capabilities activated:** `cap.identity_proofing_execution`, `cap.step_up_and_adaptive_authentication`, `cap.identity_recovery_and_reset_management`

### Stage 3 — Credential reset or re-enrollment
Upon successful re-verification, the customer is guided through credential reset (new password) or authenticator re-enrollment (new device binding, new passkey). Previous credentials or authenticators are revoked as appropriate.

**Capabilities activated:** `cap.credential_and_authenticator_management`, `cap.identity_recovery_and_reset_management`

### Stage 4 — Session establishment under recovery context
A new authenticated session is created with recovery-context metadata. The session may carry temporary restrictions (e.g., limited transaction authority for 24 hours) per recovery policy.

**Capabilities activated:** `cap.authentication`, `cap.session_and_device_trust_management`

### Stage 5 — Recovery completion and monitoring
The recovery event is recorded in the audit trail. Enhanced monitoring is applied to the account for a defined observation period following recovery.

**Capabilities activated:** `cap.access_audit_and_attestation`, `cap.access_event_monitoring_and_anomaly_response`

## 6. Alternate Paths

- In-branch recovery requires the customer to visit a branch with physical identification for higher-assurance re-proofing.
- Call-center assisted recovery routes through agent-assisted identity verification before enabling credential reset.
- Suspected compromise triggers institution-initiated forced recovery with all existing sessions and credentials revoked before re-enrollment.
- Multiple recovery factors are unavailable, requiring escalation to manual identity review.
- Customer requests recovery for a delegated or proxy access relationship.

## 7. Failure / Exception Paths

- Identity re-verification fails after maximum attempts, locking the recovery path and requiring manual intervention.
- Recovery request is flagged as potential social engineering or account takeover attempt, routing to fraud investigation.
- No viable recovery factors remain (email compromised, phone lost, no branch access), requiring exception handling.
- Account is under active fraud hold, blocking all recovery paths until investigation completes.
- Recovery completes but customer disputes that they initiated it, triggering a compromise investigation.

## 8. Related Domains

- `domain.identity_access` — owns the recovery lifecycle, credential reset, and re-proofing orchestration
- `domain.channels_experience` — owns the self-service and assisted recovery entry points and user experience
- `domain.customer` — provides canonical customer identity and lifecycle state
- `domain.servicing` — may own the case management and agent-assisted recovery workflow
- `domain.risk_fraud` — provides fraud signals and social engineering detection during recovery attempts

## 9. Related Capabilities

- `cap.identity_recovery_and_reset_management`
- `cap.identity_proofing_execution`
- `cap.credential_and_authenticator_management`
- `cap.authentication`
- `cap.step_up_and_adaptive_authentication`
- `cap.session_and_device_trust_management`
- `cap.access_event_monitoring_and_anomaly_response`
- `cap.access_audit_and_attestation`

## 10. Value Stream Context

This journey is the primary realization of `vs.account_recovery`. It protects the continuity of the customer's digital relationship with the institution. Poor recovery experiences drive customer attrition, while weak recovery controls are a primary vector for account takeover fraud.

## 11. Supporting Process Candidates

- `proc.account_recovery_v1`
- `proc.recovery_initiation`
- `proc.identity_reverification`
- `proc.credential_reset`
- `proc.authenticator_reenrollment`
- `proc.recovery_session_creation`
- `proc.post_recovery_monitoring`
- `proc.recovery_escalation`

## 12. Key Risks and Controls Encountered

### Key risks
- Account takeover through social engineering of recovery flow
- Weak recovery factors (SMS-only, knowledge-based questions) enable unauthorized reset
- Recovery bypasses normal authentication assurance levels
- Credential reset without proper revocation of old credentials
- Post-recovery monitoring gap allows immediate exploitation
- Recovery fatigue leads to customer abandonment

### Key controls
- Risk-proportional re-proofing requirements
- Recovery factor strength validation
- Immediate revocation of compromised credentials and sessions
- Post-recovery temporary access restrictions
- Enhanced monitoring window after recovery completion
- Recovery event audit trail with full evidence chain
- Velocity controls on recovery attempts per account
- Fraud signal integration during recovery flow

## 13. Typical Systems and Providers Involved

### Typical systems
- Identity recovery orchestration service
- Identity proofing and verification service
- Credential management service
- Session management service
- Case management system (for escalated recoveries)
- Fraud detection platform
- Audit and event logging platform

### Representative providers
- Ping Identity
- ForgeRock
- Okta
- Transmit Security
- Jumio / Onfido (document-based re-proofing)
- Pega / Salesforce (case management)
- BioCatch (behavioral analysis during recovery)

## 14. Open Questions

- Should institution-initiated forced recovery (compromise detected) be a separate journey given its different trigger and trust model?
- How should recovery interact with delegated access — if the primary holder recovers, what happens to delegate sessions?
- What is the minimum viable recovery path for customers with no digital recovery factors remaining?
- Should recovery assurance levels map directly to NIST SP 800-63 IAL/AAL levels or use an institution-specific scheme?

## 15. Provenance

This journey is grounded in NIST SP 800-63B requirements for credential recovery and re-proofing, FFIEC Authentication and Access guidance (2021) on account recovery controls, and observed patterns from account takeover incident analysis across the banking industry. Recovery is consistently identified as a high-risk, high-friction journey that bridges identity assurance and fraud prevention.
