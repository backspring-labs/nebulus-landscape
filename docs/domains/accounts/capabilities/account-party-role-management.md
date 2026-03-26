---
id: cap.account_party_role_management
type: capability
name: Account Party Role Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: medium
---

# Account Party Role Management

## 1. Definition

Account Party Role Management is the enduring business ability to manage account-level ownership, signer, beneficiary, authorized-party, and other product-role relationships that define who can act on, benefit from, or govern a specific account. These roles are distinct from the broader customer relationship record and represent the account-specific authority and responsibility bindings.

## 2. Business Outcome

Maintain accurate, governed, and auditable account-level party roles so the institution can correctly enforce transaction authority, comply with legal ownership requirements, support multi-party account operations, and ensure servicing actions are performed by authorized individuals.

## 3. Scope

### Included activities
- Assigning and removing account-level roles (owner, joint owner, signer, authorized signer, beneficiary, trustee, custodian, POA)
- Managing role-specific evidence requirements (signature cards, authorization forms, legal documents)
- Governing role changes with approval workflows and effective dating
- Maintaining the account-level authority and ownership registry
- Supporting joint account role setup and multi-party role coordination
- Distinguishing account-level roles from customer-level relationship roles
- Recording role change history for audit and compliance

### Excluded activities
- Customer record creation, identity verification, or party resolution (`domain.customer`)
- Customer-level relationship and household management (`cap.customer_relationship_management`)
- Transaction authorization decisioning at runtime (`domain.identity_access`, `domain.risk_fraud`)
- Legal entity structure management for commercial accounts (`domain.commercial_banking`)
- Beneficial ownership determination for BSA/AML purposes (`domain.risk_fraud`, `domain.compliance_legal`)
- Account opening and record creation (`cap.account_opening`)

## 4. Upstream Dependencies
- `domain.customer` for verified party records that can be assigned to account roles
- `cap.account_opening` for the initial account record to which roles are attached
- `domain.compliance_legal` for role-specific regulatory requirements (e.g., POA documentation, beneficiary designation rules)
- `domain.identity_access` for identity verification of parties being added to account roles

## 5. Downstream Dependencies
- `domain.identity_access` uses account roles to determine transaction authorization and entitlements
- `cap.account_terms_and_feature_management` may reference role configuration for feature eligibility
- `domain.transaction_operations` enforces role-based authority at transaction time
- `domain.channels_experience` displays role-appropriate account access and servicing options
- `domain.reporting_analytics` for role composition reporting and regulatory filings

## 6. Related Value Streams
- `vs.account_contract_establishment`
- `vs.account_authority_governance`
- `vs.multi_party_account_management`
- `vs.account_servicing_enablement`

## 7. Related Journeys
- `journey.open_joint_account`
- `journey.add_authorized_signer_to_account`
- `journey.add_beneficiary_to_account`
- `journey.remove_account_signer`
- `journey.close_account`

## 8. Likely Processes
- Account party role assignment and evidence collection
- Joint account role setup during account opening
- Account role change request and approval governance
- Role removal and de-authorization processing
- Signature card and authorization form management
- Role effective dating and history maintenance
- Account role reconciliation and periodic attestation

## 9. Risks
- Incorrect account authority assignment allows unauthorized individuals to transact on or control the account
- Joint account role inconsistency where co-owners have mismatched authority levels without clear governance
- Customer-level vs. account-level role confusion causes servicing agents to grant or deny access based on the wrong role context
- Stale account role data where departed or deceased parties remain active on account roles
- Unauthorized role change due to insufficient approval controls or social engineering

## 10. Controls
- Account role taxonomy governance with standardized role definitions, permissions, and evidence requirements
- Account role evidence requirements ensuring documentation (signature cards, legal instruments) is collected before role activation
- Multi-party role approval rules requiring appropriate authorization before adding or removing account roles
- Account role effective dating ensuring all role changes are timestamped and no retroactive changes occur without governance
- Account role change audit trail capturing who requested, who approved, what changed, and when for every role modification

## 11. Typical Systems/Providers

### Typical systems
- Core deposit role repository
- Account servicing workbench
- Authority and ownership registry
- Joint account setup workflow
- Role governance audit log

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- Q2
- nCino

## 12. Open Questions
- Should account role taxonomy be standardized across all account types (deposit, loan, investment) or domain-specific?
- How should the boundary between account-level roles and customer-level relationship roles be enforced when the same party exists in both?
- Where does beneficial ownership for BSA/AML purposes sit relative to account party roles?
- Should POA and fiduciary role management be a sub-capability or remain within the general role management scope?
- How should role inheritance work for business accounts where organizational changes affect multiple account roles simultaneously?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on account-level party authority and role management. Informed by FFIEC CIP guidance on account ownership documentation, UCC provisions on deposit account authority, and common core banking role management patterns in US retail and commercial banking. Confidence is medium due to the significant boundary overlap with customer-level relationship management and identity/access domains that requires further refinement.
