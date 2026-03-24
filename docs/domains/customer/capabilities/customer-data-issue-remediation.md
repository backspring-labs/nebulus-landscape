---
id: cap.customer_data_issue_remediation
type: capability
name: Customer Data Issue Remediation
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: high
---

# Customer Data Issue Remediation

## 1. Definition

Customer Data Issue Remediation is the enduring business ability to identify, triage, resolve, and evidence incomplete, inconsistent, disputed, stale, or exception customer data that blocks or degrades onboarding, servicing, compliance, analytics, or exit handling.

## 2. Business Outcome

Restore customer-record integrity when normal maintenance fails, so downstream processes do not operate on broken, contradictory, or unsafe data.

## 3. Scope

### Included activities
- Detecting data-quality exceptions and unresolved conflicts
- Managing remediation queues and prioritization
- Investigating conflicting customer data across systems
- Resolving stale, missing, disputed, or contradictory attributes
- Coordinating manual review and stewardship actions
- Reopening or revalidating customer data when evidence changes
- Managing remediation SLAs and escalation paths
- Recording remediation outcomes, evidence, and residual uncertainty

### Excluded activities
- Regular steady-state maintenance (`cap.customer_profile_management`, `cap.customer_contact_management`)
- Governance of normal change requests (`cap.customer_change_management`)
- Entity matching logic ownership (`cap.customer_party_resolution`)
- Risk investigation case management (`domain.risk_fraud`)
- Complaint or dispute handling not primarily about customer-record integrity (`domain.servicing`)

## 4. Upstream Dependencies
- Every customer-record maintenance capability as a source of exception conditions
- `cap.customer_party_resolution` where duplicates or merge errors are involved
- `cap.customer_change_management` where disputed or failed changes triggered the issue
- `domain.accounts`, `domain.payments`, `domain.servicing`, and `domain.risk_fraud` as common producers of downstream data conflicts

## 5. Downstream Dependencies
- `cap.customer_lifecycle_state_management` when remediation completion enables activation or reactivation
- `cap.customer_record_retention_and_archival` when unresolved data impacts retention decisions
- `cap.customer_exit_management` when unresolved issues must be settled before exit
- All downstream operational domains that consume corrected data

## 6. Related Value Streams
- `vs.customer_record_maintenance`
- `vs.data_quality`
- `vs.operational_control`
- `vs.relationship_management_and_service`

## 7. Related Journeys
- `journey.resolve_customer_data_issue`
- `journey.correct_duplicate_customer_record`
- `journey.verify_contact_point`
- `journey.complete_pending_kyc_requirements`
- `journey.reactivate_dormant_customer`

## 8. Likely Processes
- `proc.customer_data_issue_detection`
- `proc.remediation_case_triage`
- `proc.attribute_conflict_resolution`
- `proc.data_steward_review`
- `proc.remediation_closure_and_confirmation`

## 9. Risks
- Broken customer data remains unresolved and continues to propagate
- Manual remediation creates inconsistent fixes across systems
- Duplicate or conflicting records cause servicing or compliance failure
- Remediation backlog grows and blocks onboarding or servicing
- Sensitive data is exposed in remediation workflows

## 10. Controls
- `ctrl.customer_data_issue_taxonomy`
- `ctrl.remediation_priority_and_sla_rules`
- `ctrl.data_steward_resolution_standards`
- `ctrl.cross_system_reconciliation_checks`
- `ctrl.remediation_evidence_and_closure_requirements`
- `ctrl.access_controls_for_exception_workbenches`

## 11. Typical Systems/Providers

### Typical systems
- Data quality monitoring service
- Stewardship console
- Exception/case workbench
- Customer master
- Reconciliation dashboard

### Typical providers
- Informatica Data Quality
- Reltio
- ServiceNow
- Collibra
- IBM Match 360

## 12. Open Questions
- Should duplicate-resolution remediation remain here, or stay purely under party resolution if the repo later models remediation as a generic operating pattern?
- How much unresolved-data tolerance should the canonical model permit before lifecycle state must change?
- Should remediation severity be represented explicitly on the customer record?

## 13. Provenance

This capability is the exception-handling complement to the Batch 2 steady-state customer record capabilities and to Batch 1 onboarding progression. It exists because the customer domain cannot assume all data arrives cleanly or remains clean over time.
