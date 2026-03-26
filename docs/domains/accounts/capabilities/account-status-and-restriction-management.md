---
id: cap.account_status_and_restriction_management
type: capability
name: Account Status and Restriction Management
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Status and Restriction Management

## 1. Definition

Account Status and Restriction Management is the enduring business ability to govern the account's lifecycle state (active, inactive, dormant, restricted, frozen, closed) and manage transitions between states, including the application and removal of restrictions driven by regulatory, legal, risk, or operational events. It owns the account status state machine, the rules governing valid transitions, and the restriction taxonomy that determines what actions are permitted or blocked in each state.

## 2. Business Outcome

Ensure every account operates under the correct lifecycle state at all times, that status transitions occur only through authorized and governed pathways, and that restrictions are applied and removed promptly in response to regulatory, legal, risk, or operational triggers so that account usage reflects the institution's current obligations and risk posture.

## 3. Scope

### Included activities
- Defining and governing the account lifecycle state model (active, inactive, dormant, restricted, frozen, closed, and any institution-specific states)
- Managing transitions between lifecycle states with authorization, reason coding, and effective dating
- Applying account-level restrictions triggered by regulatory events, legal orders, risk decisions, or operational actions
- Removing or modifying restrictions when conditions are cleared, with governance and documentation
- Publishing account status change events for downstream consumers
- Maintaining the restriction taxonomy with standardized reason codes and permitted-action matrices
- Reconciling account status across systems of record, channels, and downstream consumers
- Supporting operations dashboards for status and restriction monitoring

### Excluded activities
- Applying, managing, and releasing holds, levies, garnishments, and other specific encumbrances (`cap.account_hold_and_constraint_management`)
- Dormancy detection, reactivation management, and escheatment processing (`cap.account_dormancy_and_escheat_coordination`)
- Account closure processing and post-closure obligations (`cap.account_closure_management`)
- Fraud or AML decisioning that may trigger a restriction (`domain.risk_fraud`)
- Legal order intake and case management (`domain.servicing`)
- Channel UX for displaying account status and restrictions (`domain.channels_experience`)

## 4. Upstream Dependencies
- `cap.account_opening` for the initial account record and its opening lifecycle state
- `cap.account_terms_and_feature_management` for product configuration that may constrain valid status transitions
- `domain.risk_fraud` for fraud, AML, and sanctions decisions that trigger restriction application
- `domain.servicing` for legal order intake and case-driven restriction requests
- `domain.orchestration_control` for workflow sequencing of complex status transitions

## 5. Downstream Dependencies
- `cap.account_hold_and_constraint_management` receives restriction context that may affect hold behavior
- `cap.account_dormancy_and_escheat_coordination` receives status signals for dormancy detection
- `cap.account_closure_management` receives the status transition that initiates closure processing
- `domain.payments` consumes account status to determine payment eligibility
- `domain.channels_experience` displays account status and restriction indicators to customers and staff
- `domain.reporting_analytics` for status distribution reporting, restriction metrics, and compliance dashboards

## 6. Related Value Streams
- `vs.account_lifecycle_governance`
- `vs.account_restriction_enforcement`
- `vs.deposit_account_servicing`
- `vs.regulatory_compliance_assurance`

## 7. Related Journeys
- `journey.place_account_hold`
- `journey.release_account_hold`
- `journey.reactivate_dormant_account`
- `journey.close_deposit_account`
- `journey.restrict_account_for_legal_order`

## 8. Likely Processes
- Account status transition request and authorization
- Restriction application with reason coding and effective dating
- Restriction removal with clearance documentation and governance
- Account status reconciliation across core banking, channels, and downstream systems
- Restriction taxonomy maintenance and permitted-action matrix updates
- Status change event publication and consumer notification
- Operations monitoring for accounts in restricted or frozen states
- Periodic review of long-standing restrictions for continued validity

## 9. Risks
- Unauthorized status transitions allow account usage that should be blocked or block usage that should be permitted
- Restrictions not applied in a timely manner when triggered by regulatory, legal, or risk events, creating compliance exposure
- Restrictions not removed in a timely manner when conditions are cleared, causing unnecessary customer friction and servicing escalations
- Status state divergence across systems where the core banking system, channel systems, and downstream consumers reflect different account states
- Dormancy detection failure where accounts are not transitioned to inactive or dormant status when activity thresholds are missed

## 10. Controls
- Status transition authorization matrix defining who can initiate and approve each type of transition
- Restriction application timeliness monitoring with SLA tracking and escalation for overdue restriction requests
- Restriction removal governance requiring documented clearance and approval before restrictions are lifted
- Account status reconciliation across systems at scheduled intervals with exception reporting for divergences
- Status change audit trail capturing every transition with timestamp, reason code, authorizing user, and source system

## 11. Typical Systems/Providers

### Typical systems
- Account status engine
- Restriction management service
- Account lifecycle state machine
- Operations servicing workbench
- Regulatory restriction intake service

### Typical providers
- Fiserv
- Jack Henry
- Temenos
- Thought Machine
- Mambu

## 12. Open Questions
- Should the account status state machine be a shared kernel component or remain fully owned within this capability?
- How should this capability interact with payment-level restrictions when a single regulatory event (e.g., OFAC match) requires both account-level and payment-level blocks?
- Where does the boundary sit between status-driven restrictions managed here and specific encumbrances (holds, levies, garnishments) managed by `cap.account_hold_and_constraint_management`?
- Should restriction reason codes be standardized across the enterprise or managed domain by domain?
- How should status transitions handle accounts with multiple concurrent restrictions from different sources (legal, risk, operations)?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on lifecycle state governance and restriction management. Informed by Regulation D (reserve requirements affecting transaction account status), UCC Article 4 (bank deposit and collections), common core banking account status patterns, and operational challenges observed in restriction application timeliness and cross-system status synchronization in US retail banking. Confidence is high because account status and restriction management is a well-understood, universal capability in deposit account processing with clear regulatory drivers and established industry patterns.
