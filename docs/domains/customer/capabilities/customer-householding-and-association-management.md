---
id: cap.customer_householding_and_association_management
type: capability
name: Customer Householding and Association Management
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: medium
---

# Customer Householding and Association Management

## 1. Definition

Customer Householding and Association Management is the enduring business ability to create, maintain, and govern relationship links among customers, businesses, households, beneficial owners, authorized parties, affiliates, and other associated parties so the institution can treat related parties consistently for servicing, risk context, outreach, and relationship understanding.

## 2. Business Outcome

Provide a governed model of how customers are related to one another and to organizations so downstream teams can recognize shared context, avoid fragmented treatment, support multi-party servicing, and apply appropriate relationship-aware controls.

## 3. Scope

### Included activities
- Creating and maintaining household relationships between individuals
- Creating and maintaining customer-to-business affiliations
- Managing relationship types such as spouse, joint participant, beneficial owner, guarantor, trustee, authorized signer, power of attorney, beneficiary, employee, or controller
- Storing effective dates, evidence, ownership, and provenance for relationship links
- Managing primary/secondary household roles where modeled
- Managing relationship hierarchies, association statuses, and link confidence
- Maintaining relationship metadata needed for servicing, compliance context, marketing eligibility, or operational routing
- Managing association review, correction, and removal workflows
- Supporting downstream consumption of association links by servicing, onboarding, risk, and analytics functions

### Excluded activities
- Individual customer demographic/profile ownership (`cap.customer_profile_management`)
- Customer segment/tier/relationship-owner treatment logic (`cap.customer_relationship_management`)
- Account signer/owner contract structures owned as account constructs (`domain.accounts`)
- Fraud or AML beneficial-ownership decisioning (`domain.risk_fraud`)
- Identity proofing of associated parties (`domain.identity_access`)
- Generic workflow tooling (`domain.orchestration_control`)

## 4. Upstream Dependencies
- `cap.customer_identity_establishment` for canonical party anchors
- `cap.customer_party_resolution` to avoid linking ambiguous or duplicate parties
- `cap.customer_profile_management` for core party attributes
- `cap.customer_due_diligence_coordination` for relationship evidence collection in business or higher-risk contexts
- `domain.accounts` where account-level party roles must be synchronized with customer-level associations

## 5. Downstream Dependencies
- `cap.customer_relationship_management` for relationship treatment and ownership context
- `cap.customer_lifecycle_state_management` when household or affiliation changes alter lifecycle handling
- `cap.customer_data_issue_remediation` when broken or conflicting links require cleanup
- `domain.risk_fraud` for beneficial ownership, related-party, or control-person context
- `domain.servicing` for household-aware servicing and authorized-party interactions
- `domain.data_rewards` for relationship-based segmentation or household rollups

## 6. Related Value Streams
- `vs.relationship_management_and_service`
- `vs.customer_record_maintenance`
- `vs.enterprise_party_management`
- `vs.compliance_enablement`

## 7. Related Journeys
- `journey.add_beneficial_owner_or_related_party`
- `journey.manage_household_relationships`
- `journey.open_joint_account`
- `journey.small_business_relationship_start`
- `journey.authorize_third_party_to_act_on_customer_behalf`

## 8. Likely Processes
- `proc.association_link_creation`
- `proc.household_membership_management`
- `proc.related_party_role_assignment`
- `proc.association_evidence_review`
- `proc.association_unlink_and_cleanup`

## 9. Risks
- Related parties are linked incorrectly, creating privacy or servicing harm
- Beneficial-owner or control-person associations are incomplete
- Stale associations continue to drive downstream decisions after the relationship changed
- Householding logic creates unintended data exposure across individuals
- Duplicate customer records create contradictory association graphs

## 10. Controls
- `ctrl.relationship_type_taxonomy`
- `ctrl.association_evidence_requirements`
- `ctrl.effective_dated_association_links`
- `ctrl.privacy_scoped_householding_rules`
- `ctrl.association_change_audit_trail`
- `ctrl.periodic_association_review`

## 11. Typical Systems/Providers

### Typical systems
- Party master / household graph store
- CRM relationship hierarchy model
- Beneficial ownership registry
- Data stewardship console
- Relationship review workbench

### Typical providers
- Informatica MDM
- Reltio
- IBM Match 360
- Fenergo
- Salesforce

## 12. Open Questions
- Should householding remain here, or eventually split from broader association management if the repo deepens legal-entity modeling?
- How far should customer-level association modeling extend before overlap with account-party constructs becomes too great?
- Should relationship-link confidence be explicit in the canonical model?

## 13. Provenance

This capability extends the Batch 2 relationship-context model into explicit inter-party linkage. It remains distinct from profile and relationship treatment, consistent with the earlier boundary that customer record ownership and relationship context are separate but adjacent concerns.
