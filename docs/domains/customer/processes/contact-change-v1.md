---
id: proc.contact_change_v1
type: process
name: Contact Change v1
journey_id: journey.update_contact_information
capability_ids:
  - cap.customer_contact_management
  - cap.customer_change_management
  - cap.customer_preference_management
  - cap.customer_consent_management
  - cap.customer_data_issue_remediation
  - cap.customer_profile_management
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Contact Change v1

## 1. Objective

Operationally receive, validate, approve, apply, and propagate a customer contact-point change with appropriate fraud resistance and auditability.

## 2. Scope

This process covers:
- contact-change request intake
- sensitivity assessment
- verification where required
- contact-point validation and update
- downstream propagation
- exception/remediation handling

It does not cover:
- broader profile maintenance outside contact points
- unrelated preference-only changes
- complaint/dispute handling unrelated to the record update

## 3. Trigger

An existing customer or authorized representative requests a contact update.

## 4. Entry Conditions

- Customer exists and can be located.
- Change channel is authenticated or assisted.
- Change policy for the requested contact type is available.
- Required validation services are reachable.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.contact_change.request_intake` | Request Intake | `domain.customer` |
| `stage.contact_change.sensitivity_and_verification` | Sensitivity and Verification | `domain.customer` with `domain.identity_access` participation |
| `stage.contact_change.contact_validation` | Contact Validation | `domain.customer` |
| `stage.contact_change.change_approval_and_application` | Change Approval and Application | `domain.customer` |
| `stage.contact_change.preference_and_consent_impact_review` | Preference and Consent Impact Review | `domain.customer` |
| `stage.contact_change.propagation_and_exception_closure` | Propagation and Exception Closure | `domain.customer` |

## 6. Stage-by-Stage Flow

### Stage: `stage.contact_change.request_intake`

**Description:** Capture the requested change and classify its type.

**Capabilities activated:** `cap.customer_change_management`, `cap.customer_contact_management`

**Systems involved:** `sys.crm_platform`, `sys.customer_master`, `sys.contact_data_store`

**Decision points:** What contact point is changing? Who is requesting the change? Is this routine or potentially high risk?

**Risks:** Unauthorized requester, wrong-customer selection, incomplete change request

**Controls:** Customer lookup confirmation, change-taxonomy classification, intake audit trail

**Evidence generated:** Change request record, requester context, requested old/new value set

**Handoff:** Move to sensitivity and verification stage

### Stage: `stage.contact_change.sensitivity_and_verification`

**Description:** Determine whether the requested change requires step-up checks and perform them where necessary.

**Capabilities activated:** `cap.customer_change_management`

**Participating domain:** `domain.identity_access`

**Systems involved:** `sys.otp_and_verification_service`, `sys.rules_engine`, `sys.audit_event_log`

**Decision points:** Is the change sensitive? Did verification succeed? Should the request be blocked, held, or escalated?

**Risks:** Account takeover via contact change, approval bypass, inadequate challenge for high-risk update

**Controls:** `ctrl.sensitive_change_step_up_controls`, `ctrl.maker_checker_for_high_risk_changes`, policy-based challenge rules

**Evidence generated:** Verification result, escalation flag if triggered, approval/hold decision

**Handoff:** Verified requests move to validation; failed ones terminate or escalate.

### Stage: `stage.contact_change.contact_validation`

**Description:** Validate and standardize the proposed contact point.

**Capabilities activated:** `cap.customer_contact_management`

**Systems involved:** `sys.address_validation_service`, `sys.contact_data_store`, `sys.customer_master`

**Decision points:** Is the address/phone/email valid? Should the change be effective immediately or future-dated? Does this create conflict with existing record state?

**Risks:** Invalid contact point, misdirected communication, inconsistent contact history

**Controls:** Address standardization, contact verification, effective-dating controls

**Evidence generated:** Validation result, standardized contact value, effective date metadata

**Handoff:** Validated change moves to approval/application.

### Stage: `stage.contact_change.change_approval_and_application`

**Description:** Apply the change, preserve history, and record the governing reason and authority.

**Capabilities activated:** `cap.customer_change_management`, `cap.customer_contact_management`

**Systems involved:** `sys.customer_master`, `sys.audit_event_log`, `sys.crm_platform`

**Decision points:** Final approve vs reject, immediate apply vs hold, single-system or multi-system update mode

**Risks:** Unauthorized applied change, loss of historical evidence, conflicting current vs future values

**Controls:** Effective-dating/history preservation, change audit trail, approval controls

**Evidence generated:** Applied change event, reason code, before/after values, authority record

**Handoff:** Move to preference/consent impact review.

### Stage: `stage.contact_change.preference_and_consent_impact_review`

**Description:** Review whether the updated contact point changes preference fulfillment or consent enforcement.

**Capabilities activated:** `cap.customer_preference_management`, `cap.customer_consent_management`

**Systems involved:** `sys.preference_center`, `sys.consent_registry`, `sys.notification_service`

**Decision points:** Do contact preferences need to be updated? Does the new channel require consent treatment changes? Should the customer be prompted for a related update?

**Risks:** Consent not honored on new contact point, preference fragmentation, invalid fallback routing

**Controls:** Separation of preference vs consent, propagation testing, fallback rules with consent checks

**Evidence generated:** Updated preference/consent linkage records, downstream communications eligibility state

**Handoff:** Move to propagation and closure.

### Stage: `stage.contact_change.propagation_and_exception_closure`

**Description:** Propagate the final approved change to downstream systems and resolve any exceptions.

**Capabilities activated:** `cap.customer_change_management`, `cap.customer_data_issue_remediation`

**Systems involved:** `sys.eventing_and_notifications`, `sys.exception_workbench`, `sys.reconciliation_dashboard`

**Decision points:** Did downstream propagation succeed? Did any systems reject or conflict with the update? Does remediation need to open?

**Risks:** Downstream change propagation failure, unresolved exception, stale contact data in secondary systems

**Controls:** Propagation reconciliation, remediation priority/SLA rules, closure evidence requirements

**Evidence generated:** Propagation status, reconciliation result, remediation case if opened, closure confirmation

**Handoff:** Process complete if synchronized; otherwise hand to remediation.

## 7. Decision Points

- Routine vs sensitive contact update
- Verification pass vs fail
- Valid vs invalid contact point
- Apply vs hold
- Preference/consent impact yes vs no
- Synchronized vs remediation required

## 8. Alternate Outcomes

- Low-risk self-service update
- High-risk change escalated for review
- Agent-assisted change
- Temporary address future-dated
- Verification failure and abort
- Remediation case opened

## 9. Risks (process-level)

- Fraudulent contact-point takeover
- Misdirected communications
- Inconsistent downstream updates
- Inadequate evidence of who requested the change

## 10. Controls (process-level)

- Sensitive-change verification controls
- Address/contact validation
- Change audit trail
- Propagation reconciliation
- Remediation escalation and closure standards

## 11. Evidence / Audit Considerations

Critical evidence:
- request origin and requester identity context
- verification result
- before/after contact values
- effective date
- downstream propagation result
- remediation case where needed

## 12. Systems Involved (summary)

- customer master, CRM platform, contact store, validation service, OTP/verification service, preference/consent systems, exception workbench, reconciliation dashboard

## 13. Provider Participation

Representative providers: Twilio, Loqate, Melissa, Salesforce, ServiceNow, Informatica

## 14. Open Questions

- Which specific contact changes should always require step-up verification?
- Should preference/consent review be mandatory for every contact change or only certain contact types?
- How should temporary and permanent address changes be separated later?

## 15. Provenance

This process is smaller than onboarding, but it is one of the highest-frequency control surfaces in the Customer domain and a common source of fraud and operational errors.
