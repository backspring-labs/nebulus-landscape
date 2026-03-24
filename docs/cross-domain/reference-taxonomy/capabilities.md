---
id: ref.capabilities
type: reference_taxonomy
name: Capability Index
status: draft
source_refs: []
last_reviewed: 2026-03-24
---

# Capability Index

## Customer domain (18 capabilities)

### Batch 1 — Acquisition and Onboarding

| Capability | ID | Confidence |
|------------|----|------------|
| Customer Acquisition Management | `cap.customer_acquisition_management` | medium |
| Customer Application Intake | `cap.customer_application_intake` | high |
| Customer Onboarding Orchestration | `cap.customer_onboarding_orchestration` | high |
| Customer Identity Establishment | `cap.customer_identity_establishment` | high |
| Customer Party Resolution | `cap.customer_party_resolution` | high |
| Customer Due Diligence Coordination | `cap.customer_due_diligence_coordination` | high |

### Batch 2 — Profile, Contact, Consent

| Capability | ID | Confidence |
|------------|----|------------|
| Customer Profile Management | `cap.customer_profile_management` | medium |
| Customer Contact Management | `cap.customer_contact_management` | medium |
| Customer Preference Management | `cap.customer_preference_management` | medium |
| Customer Consent Management | `cap.customer_consent_management` | medium |
| Customer Disclosure & Attestation Management | `cap.customer_disclosure_attestation_management` | medium |
| Customer Relationship Management | `cap.customer_relationship_management` | low |

### Batch 3 — Lifecycle, Governance, Exit

| Capability | ID | Confidence |
|------------|----|------------|
| Customer Householding and Association Management | `cap.customer_householding_and_association_management` | medium |
| Customer Lifecycle State Management | `cap.customer_lifecycle_state_management` | high |
| Customer Change Management | `cap.customer_change_management` | high |
| Customer Data Issue Remediation | `cap.customer_data_issue_remediation` | high |
| Customer Record Retention and Archival | `cap.customer_record_retention_and_archival` | medium |
| Customer Exit Management | `cap.customer_exit_management` | medium |

## Capability lifecycle arc

- **Batch 1** = establish the customer relationship
- **Batch 2** = maintain the live customer record
- **Batch 3** = govern, repair, age, and terminate

## Capabilities referenced but not yet modeled

These capabilities are referenced by seeded journey or process artifacts but belong to domains that are skeleton-only.

| Capability | ID | Expected Domain |
|------------|----|-----------------|
| Authentication | `cap.authentication` | `domain.identity_access` |
| Account Opening | `cap.account_opening` | `domain.accounts` |
| Initial Funding | `cap.initial_funding` | `domain.accounts` |
