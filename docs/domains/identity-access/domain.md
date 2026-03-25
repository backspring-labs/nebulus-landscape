---
id: domain.identity_access
type: domain
name: Identity & Access
status: draft
source_refs:
  - src.domain_customer_boundary_v1
last_reviewed: 2026-03-25
confidence: high
---

# Identity & Access

## 1. Definition

The Identity & Access domain is the durable business area responsible for establishing, verifying, securing, and governing digital identity and runtime access trust for parties interacting with the institution. It owns the mechanisms and policies used to determine who a party is in an access context, how that identity is verified or re-verified, what credentials and authenticators are bound to that identity, what permissions or entitlements are granted, what session trust is active at a given moment, and how access is monitored, challenged, revoked, or adjusted over time.

In practical terms, this domain answers four persistent questions:

1. **Who is attempting to act?**
2. **Can we verify that identity to the required level of confidence?**
3. **What are they allowed to do right now?**
4. **How do we preserve, elevate, or revoke trust over the life of the interaction?**

This domain therefore owns the institution's **trust and verification layer** for digital and assisted access. It includes identity proofing execution, credential lifecycle, authentication, authorization, session/device trust, delegated access, and access governance. It does **not** own the business customer record, customer-relationship lifecycle, or product/account contracts. Instead, it provides the trust services those neighboring domains depend on.

A critical boundary established by Customer research is preserved here:

- **Business identity** — the institution's durable customer/party record — belongs to `domain.customer`
- **Access identity** — credentials, authenticators, sessions, authorizations, entitlements, and delegated access — belongs to `domain.identity_access`

This distinction matters because institutions frequently blur customer record ownership and access-control ownership in the same platform, but they remain conceptually and operationally different concerns.

## 2. Business Purpose

Enable the institution to establish and maintain appropriate trust in digital and assisted interactions by proving identity when required, authenticating access attempts, authorizing permitted actions, and continuously governing trust over time.

Without a coherent Identity & Access domain:

- digital channels cannot safely determine whether the person logging in is the legitimate party,
- sensitive customer changes cannot be protected with step-up or risk-based challenge,
- customer onboarding and product opening cannot rely on trustworthy identity proofing results,
- delegated and employee/admin actions become difficult to govern,
- session and device trust erode over time, increasing fraud and operational risk,
- downstream domains are forced to build ad hoc trust logic rather than consuming a shared control layer.

In banking and fintech, Identity & Access is not just a login function. It is the operational trust substrate that protects customer interactions, employee and partner access, sensitive maintenance workflows, and high-risk actions such as onboarding, profile changes, consent changes, wire setup, credential resets, and post-exit access removal.

## 3. Scope

### Includes

The Identity & Access domain includes capabilities required to:

- perform identity proofing and verification execution for prospective or existing parties,
- establish and maintain access identities distinct from business/customer identities,
- issue, bind, rotate, recover, suspend, and revoke credentials and authenticators such as passwords, passkeys, tokens, or MFA factors,
- authenticate parties at login and at sensitive in-session decision points,
- maintain device, channel, and session trust posture,
- manage authorization, permissions, roles, and entitlements,
- support delegated, proxy, or authorized-party access where access is permitted,
- enforce step-up or adaptive authentication for higher-risk actions,
- monitor authentication and access anomalies and trigger trust responses or escalation,
- maintain access governance, auditability, revocation, and post-exit deprovisioning,
- support federation or external identity trust relationships where the institution accepts or brokers them.

### Excludes

- **Business customer record creation, maintenance, and lifecycle** → `domain.customer`
  - Customer owns the business relationship and canonical customer record.
- **Customer due diligence coordination** → `domain.customer`
  - Customer coordinates due diligence collection and process progression; Identity & Access owns proofing/verification execution only.
- **Account opening, account maintenance, account closure** → `domain.accounts`
  - Identity & Access may gate access to those actions but does not own product/account contracts.
- **Fraud, AML, sanctions, and broader risk decisioning** → `domain.risk_fraud`
  - Identity & Access may emit signals or consume risk scores, but does not own those decision domains.
- **Workflow engine management and generic policy-orchestration tooling** → `domain.orchestration_control`
  - Identity & Access owns trust policies and access decisions; not generic orchestration infrastructure.
- **Channel UX and screen presentation** → `domain.channels_experience`
  - Identity & Access defines trust requirements; channels present the user experience.
- **Case management, complaints, dispute handling** → `domain.servicing`
  - Identity & Access may trigger or inform cases but does not own them.
- **Customer analytics, rewards, and loyalty treatment** → `domain.data_rewards`

### Boundary recommendations

- **Customer identity vs access identity**: The customer record is a business object in `domain.customer`; the access identity is a runtime trust object in `domain.identity_access`.
- **Proofing execution vs due diligence coordination**: Identity proofing execution belongs here; orchestration of customer-evidence collection remains in `domain.customer`.
- **Authentication vs authorization**: Authentication confirms the acting identity or trust state; authorization determines allowed actions once identity is sufficiently trusted.
- **Session trust vs fraud decisioning**: Session/device trust belongs here; broader behavioral or financial fraud outcomes belong in `domain.risk_fraud`.
- **Delegated access vs relationship ownership**: The right to act for another party in an access context belongs here; the durable business relationship between those parties belongs in `domain.customer`.

## 4. Included Capabilities

The following capabilities are proposed as the durable capability set for `domain.identity_access`.

- `cap.identity_proofing_execution` — Perform identity proofing and verification checks to establish confidence that a party is who they claim to be in an access or onboarding context.
- `cap.access_identity_establishment` — Create and maintain access identities, identifiers, and trust anchors used for authentication and authorization.
- `cap.credential_and_authenticator_management` — Issue, bind, maintain, rotate, recover, suspend, and revoke credentials and authenticators such as passwords, passkeys, tokens, or MFA factors.
- `cap.authentication` — Authenticate parties at login or re-entry points using required credentials, factors, or trust assertions.
- `cap.step_up_and_adaptive_authentication` — Apply additional or adaptive authentication when action sensitivity or trust degradation requires stronger assurance.
- `cap.session_and_device_trust_management` — Establish, monitor, refresh, and revoke session and device trust over time.
- `cap.authorization_and_entitlements` — Determine what authenticated parties are permitted to do based on roles, permissions, policies, and contextual constraints.
- `cap.access_policy_decisioning` — Evaluate access-control policies and context-specific trust rules for runtime allow/deny/challenge outcomes.
- `cap.delegated_and_proxy_access_management` — Manage authorized-party, delegated, proxy, or representative access relationships in an access-control context.
- `cap.access_provisioning_and_deprovisioning` — Provision, update, suspend, and remove access rights, identities, roles, and authenticators across target systems.
- `cap.federation_and_external_identity_trust` — Manage trust relationships with external identity providers, federation brokers, or partner-issued assertions.
- `cap.identity_recovery_and_reset_management` — Recover account access, reset credentials, and re-establish trust after credential loss, lockout, or compromise.
- `cap.access_event_monitoring_and_anomaly_response` — Monitor authentication, session, and access events for anomalies and trigger trust responses or escalation.
- `cap.access_audit_and_attestation` — Maintain access logs, evidentiary records, attestation support, and review artifacts for governance and audit.
- `cap.privileged_access_governance` — Govern high-risk administrative, operational, or privileged access distinct from standard end-user access.
- `cap.consented_access_token_management` — Issue, maintain, and revoke tokens or access artifacts used for customer-authorized third-party data access where applicable.
- `cap.post_exit_access_governance` — Adjust or revoke access identities, sessions, authenticators, and entitlements when a customer or user relationship exits or is restricted.

### Capability notes

A few boundaries are likely to be blurry:

- `cap.identity_proofing_execution` vs `cap.access_identity_establishment`: proofing verifies the claimant; access identity establishment creates and governs the runtime identity object that will later authenticate.
- `cap.authentication` vs `cap.step_up_and_adaptive_authentication`: base authentication establishes access; step-up/adaptive authentication applies when more assurance is needed.
- `cap.authorization_and_entitlements` vs `cap.access_policy_decisioning`: entitlements define granted access rights; policy decisioning determines runtime allow/deny/challenge based on context.
- `cap.delegated_and_proxy_access_management` may partially overlap with customer relationship or legal authorization models. Those relationships belong in `domain.customer`; the usable access rights derived from them belong here.
- `cap.consented_access_token_management` may partially overlap with API/security platforms or data-sharing regimes. It belongs here if the model wants customer-authorized access artifacts treated as access-control objects rather than merely integration artifacts.

## 5. Excluded Capabilities

- `cap.customer_identity_establishment` → `domain.customer`
  - Establishes the institution's canonical customer identity record at the business relationship level.
- `cap.customer_profile_management` → `domain.customer`
  - Maintains the business customer profile, not runtime access identity.
- `cap.customer_due_diligence_coordination` → `domain.customer`
  - Coordinates collection and completion of due diligence requirements but does not own proofing execution.
- `cap.account_opening` / `cap.account_maintenance` / `cap.account_closure` → `domain.accounts`
  - Product/account contract lifecycle, not trust/access control.
- `cap.fraud_decisioning`, `cap.sanctions_screening`, `cap.kyc_aml_decisioning` → `domain.risk_fraud`
  - Decision engines external to identity proofing and access trust.
- `cap.workflow_engine_management` / `cap.policy_engine_execution` → `domain.orchestration_control`
  - Generic workflow/policy tooling rather than trust-domain policy semantics.
- `cap.channel_session_experience` / `cap.digital_login_screen_management` → `domain.channels_experience`
  - Presentation-layer concerns.
- `cap.case_management` / `cap.dispute_handling` → `domain.servicing`
  - Case and dispute ownership remains elsewhere.
- `cap.customer_analytics` / `cap.rewards_management` → `domain.data_rewards`

## 6. Upstream Dependencies

The Identity & Access domain typically depends on:

- `domain.customer`
  - provides the business customer/party context and relationship state that may anchor access identity creation or access adjustments.
- `domain.channels_experience`
  - provides the digital, branch, or assisted access entry points through which authentication and challenge flows occur.
- `domain.orchestration_control`
  - may provide workflow sequencing, policy routing, and exception orchestration for complex trust flows.
- external identity data sources and evidence utilities
  - document verification, phone/email reputation, address intelligence, device intelligence, or identity data references where integrated.
- enterprise directory or workforce identity sources
  - where employee, admin, or partner identities feed privileged or operational access contexts.

## 7. Downstream Dependencies

The Identity & Access domain commonly provides trust decisions, session states, or access context to:

- `domain.customer`
  - onboarding, customer change, consent change, and contact maintenance depend on proofing/authentication outcomes.
- `domain.accounts`
  - account opening and sensitive account maintenance depend on authenticated and authorized access.
- `domain.payments`
  - high-risk money movement often requires stronger authentication or access policy decisions.
- `domain.servicing`
  - assisted servicing actions depend on agent entitlements and customer authentication.
- `domain.risk_fraud`
  - consumes authentication anomalies, device/session signals, or proofing outcomes as inputs to broader risk models.
- `domain.data_rewards`
  - may depend on trusted identity or authorization state for certain data-sharing or experience flows.
- `domain.channels_experience`
  - consumes authentication state, challenge requirements, session trust, and access outcomes to shape user experience.

## 8. Related Journeys

Journeys that belong to or materially pass through `domain.identity_access` include:

- `journey.authenticate_customer_login`
- `journey.enroll_multi_factor_authentication`
- `journey.recover_account_access`
- `journey.reset_credential`
- `journey.verify_identity_for_onboarding`
- `journey.perform_step_up_for_sensitive_change`
- `journey.authorize_privileged_admin_action`
- `journey.grant_delegated_access`
- `journey.revoke_delegated_access`
- `journey.customer_authorizes_third_party_data_access`
- `journey.revoke_third_party_data_access`
- `journey.deprovision_access_after_customer_exit`
- `journey.employee_access_request_and_approval`
- `journey.review_access_attestation`

Some of these journeys will later span multiple domains. They are listed here because Identity & Access is either the primary business anchor or a mandatory participating domain.

## 9. Typical Risks and Controls

### Typical Risks

- Identity proofing is weak, bypassed, or inconsistently applied.
- Unauthorized users gain access through credential compromise or recovery weakness.
- Step-up authentication is missing for sensitive actions.
- Sessions remain trusted too long or across inappropriate contexts.
- Entitlements are overbroad, stale, or misapplied.
- Delegated/proxy access persists beyond authorized scope.
- Privileged access is insufficiently governed or monitored.
- Access is not removed promptly after exit, restriction, or role change.
- Federated or third-party identity assertions are trusted beyond appropriate assurance.
- Access logs or audit trails are incomplete for investigation or attestation.

### Typical Controls

- Proofing policy with required assurance levels by use case.
- Credential strength, authenticator binding, and recovery controls.
- MFA and step-up enforcement for higher-risk actions.
- Session timeout, device trust expiration, and re-authentication requirements.
- Role/entitlement governance with least-privilege principles.
- Runtime policy decision controls for contextual allow/deny/challenge.
- Delegated-access evidence, scope limitation, and revocation controls.
- Privileged-access approval, segmentation, monitoring, and attestation.
- Joiner/mover/leaver-style provisioning and deprovisioning controls.
- Access audit logging, review, and attestation.

## 10. Typical Systems and Providers

These are representative patterns, not prescribed architecture.

### Typical system categories

- customer identity and access management platform
- workforce identity / directory platform
- authentication and MFA service
- identity proofing / verification orchestration service
- authorization / policy decision point
- session and device trust service
- privileged access management platform
- access provisioning / identity governance platform
- token / consented-access authorization service
- audit / access logging / attestation repository

### Representative providers often seen around this domain

- `prov.okta` — workforce and customer identity / federation / access management adjacency
- `prov.microsoft_entra` — workforce identity, federation, directory, access governance adjacency
- `prov.ping_identity` — federation, authentication, access policy, directory adjacency
- `prov.auth0` — customer identity and authentication adjacency
- `prov.sailpoint` — identity governance and provisioning adjacency
- `prov.cyberark` — privileged access governance adjacency
- `prov.duo` — MFA and device/access trust adjacency
- `prov.socure` — identity proofing adjacency
- `prov.alloy` — proofing/orchestration adjacency
- `prov.trulioo` — identity verification adjacency
- `prov.lexisnexis_risk_solutions` — proofing and identity intelligence adjacency
- internal directory, IAM, and access policy stacks

Provider placement should remain careful: many vendors span Customer, Identity & Access, Risk & Fraud, and Orchestration concerns simultaneously. Providers should enrich the model, not redefine domain ownership.

## 11. Open Questions

- Should `cap.identity_proofing_execution` remain a single capability, or eventually split into consumer proofing, business-party proofing, and recovery re-proofing as the repo deepens?
- How strongly should workforce/admin identity and customer identity be modeled together inside this domain versus as separate subdomains?
- Should `cap.access_policy_decisioning` remain separate from `cap.authorization_and_entitlements`, or merge if the repo stays more business-oriented than runtime-oriented?
- How first-class should `cap.consented_access_token_management` become if open banking / third-party access expands elsewhere in the landscape?
- Where should the boundary sit between delegated access governed here and durable authorized-party relationship constructs modeled in `domain.customer`?
- Should post-exit deprovisioning remain a distinct capability, or stay a specialization under provisioning/deprovisioning once neighboring domains are more fully modeled?

## 12. Provenance

This artifact is derived from the already-established Customer domain research, which repeatedly clarified that `domain.customer` owns the business relationship and canonical customer record, while `domain.identity_access` owns credentials, authentication, authorization, session trust, and identity proofing execution. This domain definition resolves those Customer boundary questions by formalizing Identity & Access as the institution's trust and verification layer: the place where proofing is executed, runtime access is controlled, and trust is maintained or revoked over time.
