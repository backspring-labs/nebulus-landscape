---
id: cap.account_dormancy_and_escheat_coordination
type: capability
name: Account Dormancy and Escheat Coordination
domain_id: domain.accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Account Dormancy and Escheat Coordination

## 1. Definition

Account Dormancy and Escheat Coordination is the enduring business ability to detect, classify, and manage accounts approaching or in dormant status, execute required customer outreach and due-diligence contact attempts, and coordinate the escheatment of unclaimed property to the appropriate state jurisdiction. It encompasses the full lifecycle from dormancy detection through due-diligence outreach, dormancy fee assessment, escheat reporting, unclaimed property remittance, and post-escheat reclamation support, all in compliance with the Uniform Unclaimed Property Act (UUPA) and individual state escheat statutes.

## 2. Business Outcome

Ensure the institution meets all state-mandated dormancy notification, due-diligence contact, and escheat remittance obligations while minimizing premature escheatment of customer funds, preserving customer relationships through timely reactivation outreach, and maintaining accurate unclaimed property reporting that withstands state audit examination.

## 3. Scope

### Included activities
- Monitoring account activity to detect dormancy triggers based on state-specific inactivity period definitions
- Classifying accounts by dormancy stage (approaching dormancy, dormant, due-diligence required, escheat-eligible, escheated)
- Executing due-diligence contact attempts via required channels (mail, email) within state-mandated timeframes
- Retaining evidence of all due-diligence contact attempts for audit and examination purposes
- Assessing and managing dormancy fees in compliance with account terms and state fee restrictions
- Generating unclaimed property reports for each applicable state jurisdiction
- Remitting escheated funds to the appropriate state unclaimed property division
- Supporting customer reclamation of escheated funds by providing account history and documentation
- Processing account reactivation when customer-initiated activity breaks the dormancy period

### Excluded activities
- Account status transitions unrelated to dormancy (`cap.account_status_and_restriction_management`)
- Account closure processing triggered by escheatment (`cap.account_closure_management`)
- Customer address and contact information maintenance (`cap.account_maintenance_management`)
- General account activity monitoring for fraud or AML purposes (`domain.fraud`, `domain.compliance`)
- Investment or securities escheatment (`domain.wealth_management`)

## 4. Upstream Dependencies
- `cap.account_status_and_restriction_management` for current account status and restriction context
- `cap.account_maintenance_management` for current customer contact information used in due-diligence outreach
- `cap.account_interest_and_fee_configuration_management` for dormancy fee rules and assessment parameters
- `domain.customer` for customer contact details and relationship context across multiple accounts
- `domain.transactions` for transaction activity history used to determine dormancy triggers

## 5. Downstream Dependencies
- `cap.account_closure_management` for accounts closed as part of escheatment processing
- `cap.account_status_and_restriction_management` for dormancy-related status transitions
- `domain.reporting_analytics` for dormancy portfolio reporting, escheat liability tracking, and regulatory reporting
- `domain.finance` for escheat liability accrual and unclaimed property remittance accounting
- `domain.compliance` for escheat regulatory examination support and audit evidence

## 6. Related Value Streams
- `vs.account_lifecycle_management`
- `vs.regulatory_compliance`
- `vs.deposit_account_servicing`
- `vs.customer_retention`

## 7. Related Journeys
- `journey.reactivate_dormant_account`
- `journey.respond_to_dormancy_notice`
- `journey.reclaim_escheated_funds`
- `journey.review_dormant_account_portfolio`

## 8. Likely Processes
- Dormancy detection and classification based on state-specific inactivity rules
- Due-diligence contact attempt execution and evidence retention
- Dormancy fee assessment and compliance review
- Unclaimed property report generation by state jurisdiction
- Escheat remittance preparation and execution
- Account reactivation processing upon customer-initiated activity
- Post-escheat reclamation support and documentation
- Dormancy portfolio monitoring and management reporting

## 9. Risks
- Premature escheatment of customer funds due to incorrect dormancy classification or failure to recognize qualifying activity, resulting in customer harm and potential regulatory action
- Missed escheat reporting or remittance deadlines leading to state penalties, interest charges, and examination findings
- Dormancy fee assessment that violates state-specific fee restrictions or account terms, creating regulatory and reputational risk
- Insufficient due-diligence contact attempts or inadequate evidence retention, exposing the institution to state audit findings
- Escheat reporting data quality errors causing misreporting of unclaimed property amounts, owner information, or property types

## 10. Controls
- Dormancy classification rule validation ensuring state-specific inactivity periods and qualifying activity definitions are correctly applied
- Due-diligence contact evidence retention capturing contact method, date, address used, and delivery confirmation for each required attempt
- Escheat deadline monitoring with proactive alerting for upcoming reporting and remittance deadlines by state
- Dormancy fee compliance review validating fee assessments against state restrictions and account terms before posting
- Escheat remittance reconciliation confirming amounts reported match amounts remitted and account balances are properly adjusted

## 11. Typical Systems/Providers

### Typical systems
- Dormancy monitoring engine
- Escheat reporting service
- Due-diligence outreach manager
- Unclaimed property remittance service
- Dormancy classification rules engine

### Typical providers
- Fiserv
- Jack Henry
- Hyland
- Keane (a Wolters Kluwer business)
- Kelmar Associates

## 12. Open Questions
- How should dormancy classification handle accounts with multiple owners across different states with conflicting dormancy period definitions?
- Should due-diligence outreach be orchestrated centrally or delegated to individual channel systems?
- How should the institution handle accounts where the customer has other active relationships but the specific account is inactive?
- Where does the boundary sit between dormancy management here and the broader account status management in `cap.account_status_and_restriction_management`?
- How should dormancy fees interact with accounts that have holds or restrictions managed by `cap.account_hold_and_constraint_management`?

## 13. Provenance

Derived from Accounts domain bootstrap research focused on the dormancy and escheatment lifecycle for deposit accounts in US retail banking. Informed by the Revised Uniform Unclaimed Property Act (RUUPA), individual state escheat statutes, FDIC supervisory guidance on dormant account management, and common operational patterns observed in unclaimed property compliance programs. Confidence is high because dormancy detection, due-diligence outreach, and escheat remittance are well-established regulatory obligations with clear compliance patterns across all US depository institutions.
