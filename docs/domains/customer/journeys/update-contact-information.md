---
id: journey.update_contact_information
type: journey
name: Update Contact Information
domain_ids:
  - domain.customer
  - domain.identity_access
  - domain.channels_experience
  - domain.servicing
  - domain.orchestration_control
capability_ids:
  - cap.customer_contact_management
  - cap.customer_change_management
  - cap.customer_preference_management
  - cap.customer_consent_management
  - cap.customer_data_issue_remediation
  - cap.customer_profile_management
value_stream_id: vs.customer_record_maintenance
process_id: proc.contact_change_v1
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Update Contact Information

## 1. Objective

Allow an existing customer to change address, phone number, email, or similar contact points in a controlled way that preserves contactability, reduces fraud exposure, and propagates the update reliably.

## 2. Primary Actor

Existing active customer.

## 3. Trigger

The customer initiates a contact update through digital self-service, branch, call center, or assisted servicing.

## 4. Preconditions

- The customer already exists and is active or otherwise eligible to update their record.
- The institution can authenticate or verify the requester at a level appropriate to the change.
- Existing contact data is retrievable.
- Contact update policies and sensitive-change rules are defined.

## 5. Happy Path

### Stage 1 — Change initiation
The customer selects contact information to update.

**Capabilities activated:** `cap.customer_change_management`, `cap.customer_contact_management`

### Stage 2 — Verification and request qualification
The institution determines whether the change is routine or sensitive and applies any required verification.

**Capabilities activated:** `cap.customer_change_management`

**Participating neighbor domains:** `domain.identity_access`

### Stage 3 — Contact update capture
The customer provides the new address, email, phone, or other contact point.

**Capabilities activated:** `cap.customer_contact_management`

### Stage 4 — Validation and effectivity handling
The institution validates, standardizes, verifies, and applies the change, including effective dating and history preservation.

**Capabilities activated:** `cap.customer_contact_management`, `cap.customer_change_management`

### Stage 5 — Preference/consent implications
If the new contact point affects communication preferences or consented channels, the institution updates or re-evaluates those settings.

**Capabilities activated:** `cap.customer_preference_management`, `cap.customer_consent_management`

### Stage 6 — Propagation and confirmation
The change is propagated to relevant downstream systems and confirmation is sent when appropriate.

**Capabilities activated:** `cap.customer_change_management`, `cap.customer_contact_management`

## 6. Alternate Paths

- Address-only change with low sensitivity
- Email or phone update requiring stronger verification
- Assisted change by agent or representative
- Future-dated contact change
- Temporary or seasonal address addition rather than replacement

## 7. Failure / Exception Paths

- Verification fails and the update is blocked
- Address or contact validation fails
- Downstream systems do not reconcile to the updated value
- Customer disputes a recent contact change
- Potential account-takeover signal causes the change to be paused for review
- The contact point update creates a conflict requiring remediation

## 8. Related Domains

- `domain.customer` — owns the contact record and change governance
- `domain.identity_access` — may provide step-up verification or authentication
- `domain.channels_experience` — owns self-service or assisted UI
- `domain.servicing` — handles agent-assisted changes and customer follow-up
- `domain.orchestration_control` — may coordinate change workflow and propagation

## 9. Related Capabilities

- `cap.customer_contact_management`
- `cap.customer_change_management`
- `cap.customer_preference_management`
- `cap.customer_consent_management`
- `cap.customer_data_issue_remediation`
- `cap.customer_profile_management`

## 10. Value Stream Context

This journey sits squarely in `vs.customer_record_maintenance` and is one of the most common operational expressions of Customer-domain stewardship.

## 11. Supporting Process Candidates

- `proc.contact_change_v1`
- `proc.capture_contact_points`
- `proc.contact_point_verification`
- `proc.sensitive_contact_change_workflow`
- `proc.change_propagation_and_confirmation`
- `proc.remediation_case_triage`

## 12. Key Risks and Controls Encountered

### Key risks
- Fraudulent contact change used for account takeover
- Misdirected communications due to invalid update
- Missing downstream propagation
- Contactability degradation
- Inconsistent evidence of who requested the change

### Key controls
- Step-up verification for sensitive updates
- Effective-dated history preservation
- Address/contact validation and standardization
- Change audit trail
- Propagation reconciliation
- Exception/remediation workbench for conflicts

## 13. Typical Systems and Providers Involved

### Typical systems
- Customer master
- CRM platform
- Contact data store
- Address validation service
- OTP/verification service
- Eventing/notification system
- Exception workbench

### Representative providers
- Twilio
- Loqate
- Melissa
- Salesforce
- ServiceNow
- Informatica

## 14. Open Questions

- Which contact changes should always require step-up verification?
- Should communications preference updates be embedded in this journey or modeled as an adjacent follow-on journey?
- How should the journey differentiate temporary vs permanent contact changes?

## 15. Provenance

This journey is intentionally high-frequency and risk-relevant. It is simpler than onboarding, but operationally critical because contact-point changes are a common vector for customer harm and control failure.
