---
id: proc.sanctions_screening_and_match_review_v1
type: process
name: Sanctions Screening and Match Review v1
journey_id: journey.screen_customer_against_sanctions_list
capability_ids:
  - cap.sanctions_and_watchlist_screening
  - cap.alert_triage_and_disposition
  - cap.customer_and_entity_risk_rating
  - cap.case_investigation_management
  - cap.enhanced_due_diligence_and_periodic_review_decisioning
  - cap.financial_crime_risk_appetite_and_policy_management
status: draft
source_refs:
  - src.ofac_compliance_framework
  - src.ffiec_bsaaml_manual_2026
  - src.fatf_risk_based_banking_guidance
last_reviewed: 2026-03-27
confidence: high
---

# Sanctions Screening and Match Review v1

## 1. Objective

Operationally execute the controlled sequence required to screen customers, counterparties, and transactions against OFAC SDN, Consolidated Non-SDN, UN, EU, and other applicable sanctions and watchlists, evaluate potential matches through human adjudication, and render disposition decisions that either clear the subject, confirm a true match and initiate blocking, or escalate to the OFAC compliance officer for regulatory action.

## 2. Scope

This process covers:
- identification of screening triggers (onboarding, periodic review, list update, payment counterparty)
- selection and application of appropriate sanctions and watchlists
- automated screening execution and fuzzy match scoring
- potential match queue assignment and analyst review
- match adjudication and true/false positive determination
- true match escalation, blocking, and OFAC reporting
- clearance documentation and closure for false positives

This process does **not** own:
- customer onboarding orchestration (`domain.customer`)
- payment execution or hold mechanics (`domain.payments`)
- account restriction enforcement (`domain.accounts`)
- customer risk rating calculation logic (consumed from `cap.customer_and_entity_risk_rating`)
- SAR filing execution (covered by `proc.sar_filing_v1`)

## 3. Trigger

A screening event is initiated by one of: new customer onboarding, periodic rescreening schedule, sanctions list update propagation, payment counterparty screening, or manual compliance request.

## 4. Entry Conditions

- Sanctions screening engine is operational and list data is current.
- Applicable watchlists (OFAC SDN, Non-SDN, sectoral, EU, UN) are loaded and version-verified.
- Customer or counterparty identity data is available for screening input.
- Match review analyst capacity is available or queue routing is configured.
- Escalation path to OFAC compliance officer is defined and operational.

## 5. Stage Overview

| Stage ID | Stage Name | Owning Domain |
|---|---|---|
| `stage.sanctions_screening.screening_trigger_and_list_selection` | Screening Trigger and List Selection | `domain.risk_fraud` |
| `stage.sanctions_screening.automated_screening_execution` | Automated Screening Execution | `domain.risk_fraud` |
| `stage.sanctions_screening.potential_match_queue_assignment` | Potential Match Queue Assignment | `domain.risk_fraud` |
| `stage.sanctions_screening.match_review_and_adjudication` | Match Review and Adjudication | `domain.risk_fraud` |
| `stage.sanctions_screening.escalation_and_true_match_confirmation` | Escalation and True Match Confirmation | `domain.risk_fraud` |
| `stage.sanctions_screening.blocking_and_restriction_execution` | Blocking and Restriction Execution | `domain.risk_fraud` with `domain.accounts`, `domain.payments` participation |
| `stage.sanctions_screening.clearance_documentation_and_closure` | Clearance Documentation and Closure | `domain.risk_fraud` |

## 6. Stage-by-Stage Flow

### Stage: `stage.sanctions_screening.screening_trigger_and_list_selection`

**Description:** A screening event is identified based on the trigger type. The process determines which lists and screening scope apply based on the trigger context: full-list screening for onboarding and periodic review, delta-list screening for list updates, or counterparty-specific screening for payment events. The screening input payload is assembled from available identity data.

**Capabilities activated:** `cap.sanctions_and_watchlist_screening`, `cap.financial_crime_risk_appetite_and_policy_management`

**Systems involved:** `sys.sanctions_screening_engine`, `sys.watchlist_management_platform`, `sys.customer_master`

**Decision points:**
- What triggered this screening event (onboarding, periodic, list update, payment)?
- Which lists are applicable based on the subject's jurisdiction and product exposure?
- Is the screening input data sufficient for meaningful matching?
- Should the screening scope include related parties or beneficial owners?

**Risks:** `risk.screening_coverage_gap`, `risk.insufficient_screening_input_data`, `risk.list_selection_mismatch`

**Controls:** `ctrl.screening_coverage_matrix`, `ctrl.list_currency_verification`, `ctrl.screening_input_minimum_fields`

**Evidence generated:** Screening trigger event record, list selection justification, screening input payload, trigger source attribution

**Handoff:** Screening input assembled and lists selected; move to automated screening execution.

### Stage: `stage.sanctions_screening.automated_screening_execution`

**Description:** The sanctions screening engine executes the screening run against the selected lists using configured matching algorithms (exact, fuzzy, phonetic, transliteration). The engine produces a result set of no-match confirmations and potential match candidates with match scores. Results above the configured threshold are routed for human review; results below threshold are logged as cleared.

**Capabilities activated:** `cap.sanctions_and_watchlist_screening`

**Systems involved:** `sys.sanctions_screening_engine`, `sys.watchlist_management_platform`

**Decision points:**
- Did the screening run complete successfully against all required lists?
- Are any results above the match threshold requiring human review?
- Should below-threshold results be logged for audit or require secondary review?
- Did the screening engine apply all configured matching algorithms?

**Risks:** `risk.false_negative_match`, `risk.excessive_false_positives`, `risk.screening_engine_failure`, `risk.algorithm_coverage_gap`

**Controls:** `ctrl.match_scoring_threshold_governance`, `ctrl.screening_completion_verification`, `ctrl.algorithm_coverage_validation`, `ctrl.below_threshold_audit_logging`

**Evidence generated:** Screening run ID, run timestamp, lists screened, matching algorithms applied, result count, match scores for potential matches, no-match confirmation for below-threshold results

**Handoff:** Potential matches above threshold routed to match review queue. No-match results logged and cleared.

### Stage: `stage.sanctions_screening.potential_match_queue_assignment`

**Description:** Potential match alerts are ingested into the alert management platform, enriched with subject context (customer profile, account relationships, prior screening history), and routed to the appropriate compliance analyst queue based on match severity, list type, and analyst specialization.

**Capabilities activated:** `cap.alert_triage_and_disposition`, `cap.sanctions_and_watchlist_screening`

**Systems involved:** `sys.alert_management_platform`, `sys.customer_master`, `sys.sanctions_screening_engine`

**Decision points:**
- What is the priority of this match based on score and list type (SDN vs non-SDN)?
- Is this a recurring potential match that has been previously reviewed and cleared?
- Should the match be auto-dispositioned based on prior clearance records?
- Which analyst queue is appropriate based on list type and jurisdiction?

**Risks:** `risk.delayed_match_review`, `risk.stale_prior_clearance_applied_incorrectly`, `risk.queue_overflow`

**Controls:** `ctrl.match_review_sla_tracking`, `ctrl.prior_clearance_revalidation_rules`, `ctrl.sla_based_queue_management`

**Evidence generated:** Alert ID, queue assignment record, enrichment data attached, prior review history linkage, priority classification

**Handoff:** Match alert assigned to analyst for review and adjudication.

### Stage: `stage.sanctions_screening.match_review_and_adjudication`

**Description:** The compliance analyst reviews the potential match by comparing the subject's identity data against the sanctions list entry, evaluating name similarity, date of birth, address, nationality, identification numbers, and other distinguishing attributes. The analyst determines whether the match is a true positive (confirmed match to a sanctioned person/entity) or a false positive (different person/entity with similar attributes).

**Capabilities activated:** `cap.alert_triage_and_disposition`, `cap.sanctions_and_watchlist_screening`, `cap.customer_and_entity_risk_rating`

**Systems involved:** `sys.alert_management_platform`, `sys.sanctions_screening_engine`, `sys.customer_master`, `sys.case_management_system`

**Decision points:**
- Do the distinguishing attributes (DOB, address, nationality, ID numbers) confirm or refute the match?
- Is additional information needed from the customer or internal sources to resolve ambiguity?
- Does the match require escalation to a senior analyst or OFAC compliance officer?
- Should a case be opened for further investigation?

**Risks:** `risk.incomplete_match_review_documentation`, `risk.false_negative_adjudication`, `risk.analyst_inconsistency`, `risk.insufficient_distinguishing_data`

**Controls:** `ctrl.mandatory_match_review_fields`, `ctrl.adjudication_documentation_requirements`, `ctrl.escalation_criteria_for_ambiguous_matches`, `ctrl.adjudication_consistency_monitoring`

**Evidence generated:** Match review worksheet, attribute-by-attribute comparison record, analyst determination with rationale, escalation decision if applicable, additional information request records

**Handoff:** If false positive: route to clearance documentation. If true positive or ambiguous: escalate to OFAC compliance officer.

### Stage: `stage.sanctions_screening.escalation_and_true_match_confirmation`

**Description:** Potential or confirmed true matches are escalated to the OFAC compliance officer or senior compliance leadership for final determination. The OFAC compliance officer evaluates the match, confirms true match status, and initiates required actions: OFAC blocking report filing, account/transaction blocking, and coordination with legal and senior management.

**Capabilities activated:** `cap.sanctions_and_watchlist_screening`, `cap.financial_crime_risk_appetite_and_policy_management`, `cap.case_investigation_management`

**Systems involved:** `sys.case_management_system`, `sys.ofac_reporting_system`, `sys.alert_management_platform`

**Decision points:**
- Is this a confirmed true match requiring OFAC blocking and reporting?
- What is the scope of blocking required (all accounts, specific transactions, relationship termination)?
- Is the match subject to a specific OFAC license or exemption?
- Should law enforcement be notified?
- Is a 10-day OFAC blocking report required?

**Risks:** `risk.delayed_blocking_action`, `risk.ofac_reporting_failure`, `risk.incomplete_blocking_scope`, `risk.license_exemption_misapplication`

**Controls:** `ctrl.true_match_escalation_protocol`, `ctrl.blocking_action_sla`, `ctrl.ofac_reporting_deadline_tracking`, `ctrl.license_exemption_verification`

**Evidence generated:** OFAC compliance officer determination record, true match confirmation, blocking order, OFAC blocking report, license/exemption analysis if applicable, law enforcement referral if applicable

**Handoff:** True match confirmed; move to blocking and restriction execution. If determined not a true match after senior review, route to clearance documentation.

### Stage: `stage.sanctions_screening.blocking_and_restriction_execution`

**Description:** Following true match confirmation, blocking and restriction actions are executed across relevant systems. Account restrictions are requested from Accounts domain, transaction blocks are applied through Payments domain, and the customer relationship is evaluated for termination. All blocking actions are tracked and confirmed.

**Capabilities activated:** `cap.sanctions_and_watchlist_screening`, `cap.case_investigation_management`

**Systems involved:** `sys.core_banking_platform`, `sys.payment_processing_platform`, `sys.customer_master`, `sys.case_management_system`, `sys.ofac_reporting_system`

**Participating domains:** `domain.accounts`, `domain.payments`

**Decision points:**
- Which accounts and relationships require blocking?
- Are there pending transactions that must be intercepted?
- Should the customer relationship be terminated immediately or after a defined period?
- Are there joint account holders or related parties that require separate screening?

**Risks:** `risk.incomplete_blocking_execution`, `risk.collateral_impact_on_joint_holders`, `risk.pending_transaction_leakage`, `risk.blocking_reversal_without_authorization`

**Controls:** `ctrl.blocking_scope_validation`, `ctrl.blocking_confirmation_tracking`, `ctrl.joint_holder_screening_trigger`, `ctrl.blocking_reversal_authorization`

**Evidence generated:** Blocking order execution records, account restriction confirmations, transaction block confirmations, relationship termination records if applicable, joint holder screening results

**Handoff:** All blocking actions confirmed; case documentation and OFAC reporting finalized.

### Stage: `stage.sanctions_screening.clearance_documentation_and_closure`

**Description:** For false positive determinations, the analyst documents the clearance rationale, records the distinguishing attributes that differentiate the subject from the sanctions list entry, and closes the match review. Clearance records are retained for future screening runs to enable efficient re-review and for regulatory examination.

**Capabilities activated:** `cap.alert_triage_and_disposition`, `cap.sanctions_and_watchlist_screening`

**Systems involved:** `sys.alert_management_platform`, `sys.sanctions_screening_engine`, `sys.evidence_repository`

**Decision points:**
- Is the clearance documentation complete and defensible for examination?
- Should this clearance be flagged for automatic application on future screening runs?
- Does the clearance require periodic revalidation?

**Risks:** `risk.insufficient_clearance_documentation`, `risk.stale_clearance_applied_to_changed_circumstances`, `risk.clearance_without_adequate_review`

**Controls:** `ctrl.clearance_documentation_requirements`, `ctrl.clearance_revalidation_schedule`, `ctrl.clearance_review_authority`

**Evidence generated:** Clearance decision record, distinguishing attribute documentation, clearance rationale narrative, future screening disposition flag, revalidation schedule if applicable

**Handoff:** Match review closed. Clearance record available for future screening efficiency. No further action required.

## 7. Decision Points

Key decisions across the process:
- Screening trigger type and list applicability
- Match threshold: above vs below
- Potential match priority and queue routing
- Adjudication: true positive vs false positive vs ambiguous
- Escalation to OFAC compliance officer required vs not
- True match confirmed: blocking scope determination
- OFAC reporting required vs not
- Clearance: documented and closed vs requires revalidation

## 8. Alternate Outcomes

- No match found; screening cleared with no further action
- Potential match cleared as false positive with documented rationale
- True match confirmed with full blocking and OFAC reporting
- Ambiguous match requiring additional information from customer
- Match cleared but enhanced monitoring applied to customer
- List update rescreening produces new matches on previously cleared customers
- Payment counterparty blocked; originating transaction rejected

## 9. Risks (process-level)

- Stale sanctions list data causing false negatives
- Screening coverage gaps for certain product lines or counterparty types
- Excessive false positives degrading analyst efficiency and review quality
- Delayed blocking action on confirmed true matches
- Incomplete OFAC reporting or missed filing deadlines
- Inconsistent adjudication standards across analysts
- Prior clearance records applied without revalidation after material changes

## 10. Controls (process-level)

- List currency verification before each screening run
- Screening coverage matrix ensuring all required triggers and populations are covered
- Match scoring threshold governance with periodic calibration
- Mandatory match review fields and documentation requirements
- True match escalation protocol with defined authority chain
- Blocking action SLA enforcement
- OFAC reporting deadline tracking
- Clearance documentation standards and revalidation schedule

## 11. Evidence / Audit Considerations

This process generates a high-density audit trail:
- screening trigger events and list selection records
- screening run results with match scores
- queue assignment and SLA compliance records
- match review worksheets with attribute-by-attribute comparison
- adjudication decisions with rationale documentation
- escalation records and OFAC compliance officer determinations
- blocking orders and execution confirmations
- OFAC blocking reports and filing confirmations
- clearance documentation with distinguishing attribute records
- prior clearance revalidation records

## 12. Systems Involved (summary)

- sanctions screening engine
- watchlist management platform
- alert management platform
- case management system
- customer master / CIF
- payment processing platform
- OFAC reporting system
- evidence repository

## 13. Provider Participation

Representative providers often adjacent to this process:
- Dow Jones Risk & Compliance, Refinitiv World-Check, LexisNexis Risk Solutions, NICE Actimize, Fircosoft (Accuity), Oracle Financial Services, Temenos, Bottomline Technologies, Alessa (formerly CaseWare RCM)

## 14. Open Questions

- Should payment counterparty screening be modeled as a separate sub-process given its real-time latency requirements?
- How should list update rescreening be coordinated with periodic customer review schedules to avoid redundant screening?
- Should the process explicitly model the OFAC license application workflow for blocked property?
- How should beneficial owner screening be sequenced relative to direct customer screening?

## 15. Provenance

This process is the primary operational spine for sanctions compliance within `domain.risk_fraud`. It is among the highest-stakes processes in the institution because OFAC violations carry strict liability, meaning a failure to block a sanctioned party's transaction can result in significant penalties regardless of intent. Examiners evaluate list management, screening coverage, match review quality, blocking timeliness, and OFAC reporting as core sanctions program effectiveness indicators.
