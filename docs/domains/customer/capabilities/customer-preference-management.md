---
id: cap.customer_preference_management
type: capability
name: Customer Preference Management
domain_id: domain.customer
status: draft
source_refs:
  - src.ftc_can_spam_guide
  - src.ecfr_tsr_16cfr310
  - src.ecfr_glba_safeguards_16cfr314
last_reviewed: 2026-03-24
confidence: medium
---

# Customer Preference Management

## 1. Definition

Customer Preference Management is the business ability to capture, maintain, and operationalize the customer's stated non-legal preferences for interaction with the institution (e.g., language, communication channels, notification types, contact times, accessibility needs).

It distinguishes "preferences" (customer choices for convenience and experience) from "consents" (legally meaningful permissions and opt-outs).

## 2. Business Outcome
- Consistent, customer-aligned interactions across channels and business lines.
- Reduced customer friction, complaints, and avoidable servicing costs.
- A usable foundation for omnichannel delivery ("customer asked for X, consistently gets X").
- Better governance of default behaviors and exception handling (e.g., fallback when preferred channel fails).

## 3. Scope

### Included activities
- Capture and manage preference attributes: language/locale preferences; preferred channel(s) for different categories (servicing, alerts, marketing where permitted); notification preferences (which events generate notifications); contact time windows (where supported) and accessibility accommodations.
- Manage preference hierarchies and conflict resolution: "global" preferences vs product-specific preferences; precedence rules when multiple preferences apply.
- Ensure preferences are propagated to downstream communication execution systems and channels.
- Maintain change history and reason codes (customer-initiated, agent-updated, system-inferred).
- Define defaults and fallback rules (e.g., use SMS if push fails), subject to consent constraints.

### Excluded activities
- Legal permissioning and consent evidence (owned by cap.customer_consent_management).
- Delivery of regulated disclosures and capturing acknowledgements (owned by cap.customer_disclosure_attestation_management).
- Executing outbound communications (often owned by channel/comms systems; Customer governs the preference record).
- Authentication/authorization to change preferences (owned by domain.identity_access).
- "Do not contact" suppression list enforcement mandated by law where it requires consent/opt-out evidence (shared boundary with consent management; do not model as preference-only).

## 4. Upstream Dependencies
- cap.customer_contact_management (preferences depend on available, valid contact points).
- cap.customer_profile_management (language and accessibility are profile-adjacent).
- cap.customer_application_intake (initial preference capture during onboarding).
- domain.channels_experience (preference UI and customer self-service flows).
- domain.identity_access (step-up or authentication to approve changes).

## 5. Downstream Dependencies
- cap.customer_consent_management (preferences can be bounded/overridden by legal consents/opt-outs).
- domain.channels_experience (UI rendering and personalization).
- domain.servicing (agent desktops use preferences for compliant interaction).
- domain.data_rewards (analytics on engagement; must respect consent and privacy).
- domain.accounts (statement delivery "preferences" often intersect with e-delivery consent and product rules).

## 6. Related Value Streams
- `vs.customer_communications_management`
- `vs.customer_record_maintenance`
- `vs.relationship_management_and_service`

## 7. Related Journeys
- `journey.change_communication_preferences`
- `journey.set_language_preference`
- `journey.manage_notifications`
- `journey.open_savings_account`
- `journey.request_accessibility_accommodation`

## 8. Likely Processes
- proc.preference_capture_and_update
- proc.preference_conflict_resolution
- proc.preference_propagation_to_channels
- proc.preference_defaulting_and_fallback
- proc.preference_audit_reporting

## 9. Risks
- risk.preference_not_honored (customer frustration, complaints, avoidable churn).
- risk.communication_policy_conflict (preferences contradict legally enforced suppressions/consents).
- risk.preference_fragmentation (preferences stored in many systems, inconsistently applied).
- risk.inappropriate_fallback_routing (fallback uses channel lacking valid consent).
- risk.customer_data_exposure (preferences reveal sensitive attributes if mishandled).

## 10. Controls
- ctrl.central_preference_registry (one source of truth; explicit ownership).
- ctrl.preference_change_audit_trail (traceability for updates).
- ctrl.preference_propagation_testing (assure downstream enforcement across channels).
- ctrl.preference_fallback_rules_with_consent_checks (fallback must check consent eligibility).
- ctrl.role_based_access_for_agent_updates (limit agent ability to override preferences).
- ctrl.safeguards_program_alignment (protect preference data under customer info safeguards).

## 11. Typical Systems/Providers

### Typical systems
- Preference center / customer settings service (often within CRM)
- Customer communications platform (consumes preferences)
- Contact center desktop / servicing CRM (reads preferences)
- Event/notification system

### Representative providers
- CRM platforms: Salesforce, Microsoft Dynamics 365
- Communications tooling: Adobe Experience Cloud, Braze (common in fintech), Twilio

## 12. Open Questions
- Should "unsubscribe" (CAN-SPAM) and DNC suppression be modeled as consent artifacts, preference artifacts, or dual-tracked (preference + legal basis)?
- How should institution-inferred preferences (behavioral learning) be modeled and constrained by privacy rules and consent?
- Should accessibility accommodations be treated as preferences or as protected/sensitive profile attributes with extra controls?

## 13. Provenance

Synthesized from common omnichannel banking patterns and marketing compliance obligations where preferences intersect with legally constrained communications. Explicit boundary maintained with Consent Management. Confidence is medium given significant variation by jurisdiction and by whether a bank centralizes preference centers.
