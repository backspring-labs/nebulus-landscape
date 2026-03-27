---
id: journey.screen_customer_against_sanctions_list
type: journey
name: Screen Customer Against Sanctions List
domain_ids:
  - domain.risk_fraud
  - domain.customer
  - domain.payments
capability_ids:
  - cap.sanctions_and_watchlist_screening
  - cap.alert_triage_and_disposition
  - cap.customer_and_entity_risk_rating
  - cap.case_investigation_management
  - cap.enhanced_due_diligence_and_periodic_review_decisioning
  - cap.financial_crime_risk_appetite_and_policy_management
value_stream_id: vs.financial_crime_prevention
process_id: proc.sanctions_screening_v1
status: draft
source_refs:
  - src.ofac_compliance_framework
  - src.ffiec_bsaaml_manual_2026
  - src.fatf_risk_based_banking_guidance
last_reviewed: 2026-03-27
confidence: high
---

# Screen Customer Against Sanctions List

## 1. Objective

Screen a customer, counterparty, or beneficial owner against OFAC SDN, sectoral sanctions, non-SDN lists, and other applicable watchlists to identify prohibited parties, ensure the institution does not engage in sanctioned activity, and meet regulatory screening obligations at onboarding, periodic review, and triggered events.

## 2. Primary Actor

A compliance analyst within the institution's BSA/AML or OFAC compliance team who is trained and authorized to evaluate screening results and render match disposition decisions.

## 3. Trigger

A screening event is initiated by one of several triggers: new customer onboarding, periodic rescreening schedule, OFAC list update publication, payment counterparty screening requirement, or ad hoc request from a business unit or regulator.

## 4. Preconditions

- Sanctions and watchlist data feeds are current and loaded into the screening platform.
- Screening rules, fuzzy-match thresholds, and filter tuning parameters are configured and approved.
- Customer and counterparty identity data is available in sufficient quality for screening (name, date of birth, country, identification numbers).
- Analysts have the training, entitlements, and access required to evaluate potential matches.
- OFAC compliance policies and escalation procedures are published and current.

## 5. Happy Path

### Stage 1 -- Screening initiation
A screening request is submitted to the sanctions screening engine, either programmatically (e.g., triggered by onboarding workflow or list update) or manually by a compliance analyst. The request includes the subject's identifying information: name, aliases, date of birth, nationality, and identification numbers.

**Capabilities activated:** `cap.sanctions_and_watchlist_screening`, `cap.financial_crime_risk_appetite_and_policy_management`

### Stage 2 -- Automated matching and scoring
The screening engine compares the subject against all configured sanctions and watchlists using exact and fuzzy-matching algorithms. Results are scored by match confidence and filtered against tuning rules to suppress known false positives. Matches above the configured threshold generate alerts for analyst review.

**Capabilities activated:** `cap.sanctions_and_watchlist_screening`, `cap.alert_triage_and_disposition`

### Stage 3 -- Analyst review and match evaluation
The compliance analyst reviews each potential match, comparing the subject's identifying details against the listed party's details. The analyst evaluates name similarity, date of birth, country, and other corroborating or differentiating information to determine whether the match is a true positive, a false positive, or requires additional information.

**Capabilities activated:** `cap.alert_triage_and_disposition`, `cap.customer_and_entity_risk_rating`, `cap.case_investigation_management`

### Stage 4 -- Disposition and action
The analyst renders a disposition: no match (cleared), false positive (documented and suppressed for future screening), or true match (confirmed). A true match triggers immediate blocking or rejection of the relationship or transaction, notification to the OFAC compliance officer, and initiation of regulatory reporting. The customer risk rating is updated to reflect screening results.

**Capabilities activated:** `cap.alert_triage_and_disposition`, `cap.customer_and_entity_risk_rating`, `cap.enhanced_due_diligence_and_periodic_review_decisioning`

### Stage 5 -- Documentation and audit trail
All screening results, analyst decisions, supporting rationale, and actions taken are recorded in the case management system. The screening record is retained for regulatory examination and audit purposes.

**Capabilities activated:** `cap.case_investigation_management`, `cap.financial_crime_risk_appetite_and_policy_management`

## 6. Alternate Paths

- Batch rescreening processes all existing customers against updated lists overnight, generating only new or changed alerts for review.
- Payment-specific screening occurs in real time or near-real time as part of wire or ACH processing, with tighter SLA requirements.
- Interdiction screening for correspondent banking requires screening of all parties in the payment chain (originator, beneficiary, intermediaries).
- Enhanced due diligence is triggered when screening reveals a match to a PEP (politically exposed person) list rather than a sanctions list.
- The OFAC compliance officer approves a specific license from OFAC that permits limited activity with a sanctioned party under defined conditions.

## 7. Failure / Exception Paths

- Screening engine is unavailable during a time-sensitive onboarding or payment, requiring manual hold procedures.
- Customer identity data is insufficient for reliable screening, producing excessive false positives or missed matches.
- List data feed fails to update, causing the institution to screen against stale data.
- Analyst incorrectly clears a true match, exposing the institution to sanctions violation liability.
- True match is identified after the relationship or transaction has already been established, requiring retroactive blocking and regulatory notification.
- Screening volume spike (e.g., major list update) overwhelms analyst capacity, causing SLA breaches.

## 8. Related Domains

- `domain.risk_fraud` -- owns sanctions screening, match disposition, and compliance reporting
- `domain.customer` -- provides customer identity data and lifecycle state for screening and blocking
- `domain.payments` -- provides counterparty and transaction data for payment-level screening and triggers interdiction holds

## 9. Related Capabilities

- `cap.sanctions_and_watchlist_screening`
- `cap.alert_triage_and_disposition`
- `cap.customer_and_entity_risk_rating`
- `cap.case_investigation_management`
- `cap.enhanced_due_diligence_and_periodic_review_decisioning`
- `cap.financial_crime_risk_appetite_and_policy_management`

## 10. Value Stream Context

This journey is a foundational obligation within `vs.financial_crime_prevention`. Sanctions screening failures carry severe consequences including civil and criminal penalties, consent orders, and reputational damage. The effectiveness and efficiency of this journey directly determines the institution's ability to meet OFAC strict liability standards while maintaining acceptable customer onboarding and payment processing times.

## 11. Supporting Process Candidates

- `proc.sanctions_screening_v1`
- `proc.watchlist_data_ingestion`
- `proc.screening_match_triage`
- `proc.true_match_blocking_and_notification`
- `proc.ofac_reporting_and_filing`
- `proc.false_positive_suppression_management`
- `proc.batch_rescreening_execution`

## 12. Key Risks and Controls Encountered

### Key risks
- Stale or incomplete list data causes missed true matches
- Overly aggressive fuzzy matching generates unsustainable false positive volumes
- Insufficient analyst training leads to incorrect match disposition
- Real-time screening latency impacts payment processing SLAs
- Sanctioned party relationship established before screening completes
- Filter tuning suppresses true matches along with false positives

### Key controls
- Automated list update monitoring with freshness verification
- Match threshold calibration with periodic backtesting
- Dual-review for true match dispositions and blocking decisions
- Real-time screening SLA monitoring with fallback hold procedures
- Mandatory OFAC compliance training and certification for analysts
- Quality assurance sampling of cleared and suppressed matches
- Comprehensive audit trail for all screening events and dispositions
- Segregation of filter tuning authority from alert disposition authority

## 13. Typical Systems and Providers Involved

### Typical systems
- Sanctions and watchlist screening engine
- List management and data feed aggregation platform
- Alert management and case management system
- Customer identity and profile repository
- Payment processing and interdiction platform
- Regulatory reporting and filing system

### Representative providers
- Fircosoft (Accuity / LexisNexis Risk Solutions)
- Dow Jones Risk & Compliance
- Refinitiv World-Check (LSEG)
- NICE Actimize
- Oracle Financial Services (OFSS)
- Verafin (Nasdaq)
- Bottomline Technologies

## 14. Open Questions

- How should the institution handle screening against non-US sanctions lists (EU, UK, UN) when operating internationally?
- What is the acceptable false positive rate for the screening engine, and how should this be benchmarked?
- Should beneficial owner screening use the same engine and thresholds as direct customer screening?
- How should real-time payment screening SLAs be balanced against match accuracy requirements?
- What governance process should control filter tuning changes that affect match sensitivity?

## 15. Provenance

This journey represents a core regulatory obligation within the Risk & Fraud domain. OFAC sanctions compliance is a strict liability regime, meaning the institution is liable for violations regardless of intent or knowledge. Grounded in OFAC's Framework for Compliance Commitments, FFIEC BSA/AML Examination Manual (2026), and FATF risk-based approach guidance for the banking sector.
