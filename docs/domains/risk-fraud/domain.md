---
id: domain.risk_fraud
type: domain
name: Risk & Fraud
status: draft
source_refs:
  - src.fincen_cdd_rule_2026
  - src.ffiec_bsaaml_manual_2026
  - src.ofac_compliance_framework
  - src.fatf_risk_based_banking_guidance
last_reviewed: 2026-03-26
confidence: high
---

# Risk & Fraud

## 1. Definition

The Risk & Fraud domain is the durable business area that owns the decision layer for fraud and financial-crime risk across the institution. It is responsible for detecting, assessing, investigating, and deciding on fraud, sanctions, AML/BSA, and broader financial-crime risk conditions that gate customer, account, payment, and access activity.

This domain does **not** own the enforcement of its decisions (account closure execution, payment blocking infrastructure, credential revocation) or the underlying data collection about customers and transactions. It does, however, produce the risk signals, decisions, alerts, cases, and regulatory filings that other domains depend on or are constrained by.

A practical boundary is:
- **Risk & Fraud** owns risk evaluation, fraud/AML decisioning, sanctions screening, alert triage, case investigation, SAR filing, and the governance of fraud and financial-crime rules and models.
- **Customer** owns the customer record, onboarding intake, and due diligence coordination — but consumes risk decisions from this domain.
- **Identity & Access** owns authentication, credential management, and identity proofing execution — but may trigger or consume fraud signals.
- **Accounts** owns account lifecycle — but account restrictions, freezes, and offboarding may be driven by risk decisions originating here.
- **Payments** owns payment execution — but payment holds, blocks, and velocity controls are decided here.
- **Orchestration & Control** owns workflow routing and policy sequencing — but the risk rules and risk policies evaluated within those workflows are owned here.

## 2. Business Purpose

Enable the institution to detect, assess, decide on, and respond to fraud and financial-crime risk in a way that protects the institution, its customers, and the financial system while remaining operationally efficient and regulatorily defensible.

Without a coherent Risk & Fraud domain:
- fraud losses increase because detection is fragmented and decisioning is inconsistent across channels and products,
- AML/BSA compliance becomes indefensible as monitoring, alerting, investigation, and reporting lack a unified program view,
- sanctions screening gaps create existential regulatory exposure,
- onboarding risk decisions are duplicated or contradictory across journeys,
- payment and transfer risk decisioning is scattered across product-specific silos,
- model and rule governance cannot demonstrate effectiveness to regulators or auditors,
- alert volumes and false positives overwhelm operations because triage logic is not centrally governed,
- the institution cannot demonstrate a risk-based approach as required by FinCEN, FFIEC, OFAC, and FATF guidance.

In banking and fintech, Risk & Fraud is the decisioning substrate that allows customer, account, payment, and channel capabilities to operate within a governed risk envelope rather than each building their own fragmented risk logic.

## 3. Scope

### Includes

The Risk & Fraud domain includes capabilities required to:
- define and maintain the institution's financial-crime risk appetite, policies, and thresholds,
- rate and maintain risk scores for customers, entities, geographies, products, and channels,
- make risk decisions during onboarding and identity establishment (accept, decline, step-up, pend),
- screen customers, counterparties, and transactions against sanctions lists, watchlists, and PEP databases,
- decide when enhanced due diligence is required and manage periodic review decisioning,
- monitor transactions for suspicious patterns, anomalies, and policy violations,
- detect and score fraud across channels, payment types, and account activity,
- make risk decisions on payments and transfers (approve, hold, block, limit),
- surveil account and relationship activity for emerging risk indicators,
- triage, disposition, and escalate alerts from monitoring and detection systems,
- manage investigations from alert through case resolution,
- prepare and file Suspicious Activity Reports and other regulatory filings,
- respond to law enforcement requests and participate in regulatory information sharing (e.g., 314(a), 314(b)),
- govern fraud rules, models, thresholds, and their lifecycle,
- decide on risk-driven restrictions, account actions, and relationship offboarding recommendations,
- govern the overall financial-crime compliance program including testing, audit, and effectiveness assessment,
- manage compliance issues, findings, and remediation tracking.

### Excludes

- **Customer record creation and profile maintenance** → `domain.customer`
  - Risk & Fraud consumes customer data and publishes risk decisions back; it does not own the customer record.
- **Authentication, credential management, and session trust** → `domain.identity_access`
  - Risk & Fraud may consume authentication signals and publish fraud signals that affect access decisions, but does not own the access stack.
- **Identity proofing execution** → `domain.identity_access`
  - Document verification, biometric matching, and knowledge-based authentication execution belong to Identity & Access; Risk & Fraud consumes their outputs for decisioning.
- **Account opening, maintenance, and closure execution** → `domain.accounts`
  - Risk & Fraud may recommend or require account restrictions, freezes, or closures, but the execution belongs to Accounts.
- **Payment initiation, routing, and settlement execution** → `domain.payments`
  - Risk & Fraud decides whether a payment should proceed, be held, or be blocked; Payments owns the execution.
- **Workflow orchestration and policy engine infrastructure** → `domain.orchestration_control`
  - Risk & Fraud owns the risk rules and policies; Orchestration owns the engine that sequences and executes them.
- **Channel UX and presentation** → `domain.channels_experience`
  - Fraud challenges, step-up prompts, and risk-driven UX are presented by Channels but decided by Risk & Fraud.
- **General customer service, complaints, and disputes** → `domain.servicing`
  - Fraud disputes may originate in Servicing and escalate into Risk & Fraud for investigation, but general servicing remains separate.
- **Card network fraud operations and chargeback processing** → `domain.cards` / `domain.networks_schemes`
  - Network-level fraud rules, chargeback lifecycle, and scheme-specific operations are owned by Cards or Networks; Risk & Fraud provides the institution-level decisioning layer.
- **Regulatory examination management and enterprise risk management** → out of scope (enterprise risk or compliance domain, if modeled)
  - This domain covers financial-crime and fraud program governance, not enterprise-wide regulatory examination or operational risk management.

### Boundary recommendations

- **Risk decisioning vs enforcement**: Risk & Fraud decides; other domains enforce. A risk decision to restrict an account is made here; the account restriction is executed by `domain.accounts`.
- **Fraud detection vs fraud dispute**: detection, scoring, and investigation belong here; customer-facing dispute intake and provisional credit belong in `domain.servicing` or `domain.accounts`, with escalation back to Risk & Fraud for investigation.
- **Sanctions screening vs payment processing**: screening logic and list management belong here; the payment hold/release mechanism belongs in `domain.payments`.
- **Model governance vs data science infrastructure**: governance of fraud/AML models (validation, performance monitoring, champion-challenger) belongs here; underlying ML infrastructure, feature stores, and model training pipelines may belong in a data or platform domain.
- **Program governance vs enterprise audit**: financial-crime program testing, independent review, and effectiveness assessment belong here; enterprise-wide internal audit and examination management are broader.

## 4. Included Capabilities

The following capabilities are proposed as the durable capability set for `domain.risk_fraud`.

- `cap.financial_crime_risk_appetite_and_policy_management` — Define, maintain, and communicate the institution's financial-crime risk appetite, policies, thresholds, and tolerance levels that govern all downstream risk decisions.
- `cap.customer_and_entity_risk_rating` — Assign, calculate, maintain, and periodically reassess risk ratings for customers, counterparties, beneficial owners, geographies, products, and channels based on risk factors and scoring models.
- `cap.identity_and_onboarding_risk_decisioning` — Make risk-based decisions during customer onboarding and identity establishment, including accept, decline, step-up, and pend outcomes based on aggregated risk signals.
- `cap.sanctions_and_watchlist_screening` — Screen customers, counterparties, transactions, and related parties against sanctions lists (OFAC SDN, EU, UN), PEP databases, adverse media, and other watchlists, and manage hit disposition.
- `cap.enhanced_due_diligence_and_periodic_review_decisioning` — Determine when enhanced due diligence is required based on risk rating, trigger events, or regulatory mandate, and manage the decisioning and scheduling of periodic reviews.
- `cap.transaction_monitoring` — Monitor transactions, activity patterns, and behavioral indicators against rules, typologies, and models to detect potentially suspicious activity for AML/BSA and other financial-crime purposes.
- `cap.fraud_detection_and_scoring` — Detect and score potential fraud across channels, payment types, and account activity using rules, models, behavioral analytics, and device/session signals.
- `cap.payment_and_transfer_risk_decisioning` — Make real-time and near-real-time risk decisions on payments and transfers, including approve, hold, block, limit, and step-up outcomes based on fraud, sanctions, and velocity signals.
- `cap.account_and_relationship_surveillance` — Monitor account-level and relationship-level activity over time for emerging risk indicators, unusual patterns, and changes in expected behavior that may warrant investigation.
- `cap.alert_triage_and_disposition` — Receive, prioritize, review, and disposition alerts from transaction monitoring, fraud detection, screening, and surveillance systems, including escalation to case investigation.
- `cap.case_investigation_management` — Manage the lifecycle of financial-crime and fraud investigations from alert escalation through evidence gathering, analysis, narrative preparation, and case resolution.
- `cap.suspicious_activity_reporting` — Prepare, review, approve, and file Suspicious Activity Reports (SARs) and other regulatory filings in accordance with FinCEN requirements, including continuing activity reporting and supporting documentation.
- `cap.regulatory_information_sharing_and_law_enforcement_response` — Manage the institution's participation in regulatory information-sharing programs (314(a), 314(b)) and respond to law enforcement requests, subpoenas, and grand jury orders related to financial crime.
- `cap.fraud_rules_and_model_governance` — Govern the lifecycle of fraud and financial-crime detection rules and models, including creation, tuning, validation, performance monitoring, champion-challenger testing, and retirement.
- `cap.risk_restriction_and_offboarding_decisioning` — Decide on risk-driven account restrictions, relationship limitations, de-risking actions, and offboarding recommendations based on investigation outcomes, risk ratings, or policy triggers.
- `cap.financial_crime_program_governance_and_testing` — Govern the overall financial-crime compliance program, including independent testing, risk assessments, regulatory self-assessments, training, and program effectiveness reporting.
- `cap.compliance_issue_and_remediation_management` — Track, manage, and remediate compliance issues, audit findings, regulatory examination findings, and self-identified deficiencies related to financial-crime and fraud operations.

### Capability notes

A few boundaries are often blurry:
- `cap.identity_and_onboarding_risk_decisioning` vs `cap.customer_and_entity_risk_rating`: onboarding risk decisioning is a point-in-time decision during intake; risk rating is the ongoing risk score that persists and evolves over the relationship lifecycle.
- `cap.transaction_monitoring` vs `cap.fraud_detection_and_scoring`: transaction monitoring is typically AML/BSA-oriented (suspicious activity), while fraud detection is loss-prevention-oriented (unauthorized or deceptive activity). Many institutions are converging these, but they remain distinct capabilities because their regulatory drivers, model types, and operational workflows differ.
- `cap.alert_triage_and_disposition` vs `cap.case_investigation_management`: triage is the first-level review and disposition of system-generated alerts; case investigation is the deeper, analyst-driven investigation that follows escalation.
- `cap.risk_restriction_and_offboarding_decisioning` vs account closure execution: this capability decides that a restriction or exit is warranted; `domain.accounts` executes the restriction or closure.
- `cap.financial_crime_program_governance_and_testing` vs `cap.compliance_issue_and_remediation_management`: program governance covers the design and effectiveness of the overall program; issue management covers the tracking and resolution of specific deficiencies.

## 5. Excluded Capabilities

- `cap.customer_identity_establishment` → `domain.customer`
  - Establishing the business customer record is a customer domain responsibility, even though risk decisions gate it.
- `cap.identity_verification_execution` → `domain.identity_access`
  - Document verification, biometric matching, and proofing execution are identity capabilities; Risk & Fraud consumes their results.
- `cap.authentication` / `cap.session_trust_management` → `domain.identity_access`
  - Authentication and session trust are access concerns; Risk & Fraud may consume signals (e.g., device risk) or trigger step-up, but does not own the mechanism.
- `cap.account_opening` / `cap.account_maintenance` / `cap.account_closure_execution` → `domain.accounts`
  - Risk & Fraud recommends; Accounts executes.
- `cap.payment_execution` / `cap.payment_routing` → `domain.payments`
  - Risk & Fraud decides on payment risk; Payments owns execution and routing.
- `cap.chargeback_management` / `cap.network_fraud_operations` → `domain.cards` / `domain.networks_schemes`
  - Network-level fraud operations and chargeback lifecycle management are card/network concerns.
- `cap.dispute_intake` / `cap.provisional_credit` → `domain.servicing` / `domain.accounts`
  - Customer-facing dispute intake and provisional credit issuance are servicing or account operations.
- `cap.workflow_engine_management` / `cap.policy_engine_execution` → `domain.orchestration_control`
  - Risk & Fraud defines the rules; Orchestration provides the engine.
- `cap.customer_analytics` / `cap.behavioral_analytics_infrastructure` → `domain.data_rewards` or platform domain
  - Analytical infrastructure and customer-level analytics are data concerns; Risk & Fraud consumes features and scores.
- `cap.regulatory_examination_management` → enterprise risk / compliance domain
  - Broad regulatory examination and enterprise risk management extend beyond financial-crime scope.

## 6. Upstream Dependencies

The Risk & Fraud domain typically depends on:

- `domain.customer`
  - Customer profile, onboarding state, due diligence data, beneficial ownership, customer lifecycle status, and relationship context required for risk rating, screening, and decisioning.
- `domain.identity_access`
  - Identity proofing results, authentication signals, device trust, and session context consumed during fraud detection and onboarding risk decisioning.
- `domain.accounts`
  - Account profile, product type, balance, tenure, and account activity data consumed for transaction monitoring, surveillance, and risk rating.
- `domain.payments`
  - Payment and transfer details, counterparty information, and velocity data consumed for real-time fraud scoring, transaction monitoring, and sanctions screening.
- `domain.orchestration_control`
  - Workflow orchestration, policy engine infrastructure, and sequencing services that execute risk rules and route risk decisions within broader business processes.
- `domain.channels_experience`
  - Channel context, device metadata, session attributes, and interaction signals consumed for fraud detection and risk scoring.
- `domain.cards`
  - Card transaction data, authorization details, and network-level fraud signals consumed for card fraud detection and decisioning.
- `domain.networks_schemes`
  - Network rules, scheme-level fraud data, and consortium intelligence consumed for screening, monitoring, and fraud detection.
- external data providers and utilities
  - Sanctions lists (OFAC, EU, UN), adverse media feeds, consortium fraud databases, device intelligence, and geolocation services.
- `domain.ledger_core`
  - Ledger and core banking transaction records, balances, and posting details consumed for monitoring and surveillance.

## 7. Downstream Dependencies

The Risk & Fraud domain commonly provides decisions, signals, and filings to:

- `domain.customer`
  - Risk ratings, onboarding risk decisions, EDD requirements, periodic review outcomes, and risk-driven customer status changes.
- `domain.identity_access`
  - Fraud signals that trigger step-up authentication, session termination, or credential review.
- `domain.accounts`
  - Account restriction decisions, freeze recommendations, risk-driven closure recommendations, and surveillance-driven action requests.
- `domain.payments`
  - Payment approve/hold/block decisions, velocity limit enforcement signals, and real-time fraud scores.
- `domain.servicing`
  - Investigation outcomes for fraud disputes, case status updates, and risk-driven servicing restrictions.
- `domain.channels_experience`
  - Risk-driven UX decisions such as fraud challenges, step-up prompts, transaction decline messaging, and restricted functionality indicators.
- `domain.data_rewards`
  - Risk scores, flags, and investigation outcomes consumed for analytics, segmentation, and risk-adjusted customer treatment.
- regulatory bodies and law enforcement
  - SAR filings (FinCEN), 314(a) responses, law enforcement subpoena responses, and CTR filings.

## 8. Related Journeys

Journeys that pass through or materially depend on the Risk & Fraud domain include:

- `journey.open_savings_account`
  - Onboarding risk decisioning, sanctions screening, CDD/EDD determination, and risk rating assignment during account opening.
- `journey.open_checking_account`
  - Same risk decisioning pattern as savings; may include additional fraud checks for check-writing and debit access.
- `journey.open_joint_account`
  - Risk assessment of multiple parties, beneficial ownership screening, and aggregate relationship risk evaluation.
- `journey.customer_onboarding`
  - Risk-gated onboarding with accept/decline/pend/step-up outcomes driving the progression of the customer intake.
- `journey.send_ach_payment`
  - Real-time fraud scoring, sanctions screening of recipients, velocity checks, and payment risk decisioning.
- `journey.send_wire_transfer`
  - Enhanced sanctions screening, fraud detection, and risk decisioning with higher-value thresholds and correspondent banking considerations.
- `journey.send_p2p_payment`
  - Fraud scoring, recipient screening, velocity monitoring, and real-time risk decisioning for peer-to-peer transfers.
- `journey.report_fraud`
  - Fraud claim intake triggers investigation case creation, provisional credit decisioning, and account surveillance adjustments.
- `journey.dispute_transaction`
  - Transaction disputes may escalate into fraud investigation, requiring case management and evidence review.
- `journey.periodic_customer_review`
  - Scheduled EDD and periodic review driven by risk rating, triggering re-screening, updated risk assessment, and possible action.
- `journey.respond_to_law_enforcement_request`
  - 314(a) searches, subpoena response, and account information production coordinated through Risk & Fraud.
- `journey.investigate_suspicious_activity`
  - Alert-to-SAR lifecycle including triage, investigation, narrative, and filing.
- `journey.update_customer_profile`
  - Profile changes may trigger re-screening, risk rating recalculation, and surveillance rule adjustments.
- `journey.close_account`
  - Account closure may be risk-driven; exit risk assessment and SAR obligations may apply.
- `journey.onboard_business_customer`
  - Enhanced CDD, beneficial ownership screening, entity risk rating, and sanctions screening for business relationships.

Some of these journeys span multiple domains. They are listed here because Risk & Fraud is either the primary decisioning authority or a mandatory participating domain.

## 9. Typical Risks and Controls

### Typical Risks

- Sanctions screening gaps allow prohibited transactions or relationships to proceed, creating existential regulatory and reputational exposure.
- Transaction monitoring rules fail to detect suspicious patterns, resulting in missed SARs and regulatory criticism.
- Fraud detection models degrade over time, leading to increased false negatives and fraud losses.
- Alert volumes overwhelm operations due to poorly tuned rules, resulting in backlog, missed escalations, and regulatory risk.
- Onboarding risk decisions are inconsistent across channels or products, creating arbitrage opportunities for bad actors.
- CDD and EDD processes are incomplete or untimely, failing to meet FinCEN Customer Due Diligence Rule requirements.
- SAR filing is late, incomplete, or contains inaccurate narratives, exposing the institution to FinCEN enforcement.
- Case investigations lack sufficient documentation, making regulatory defense difficult.
- Model and rule changes are deployed without adequate validation, introducing unintended risk gaps.
- Information-sharing responses (314(a)) are late or incomplete, damaging the institution's regulatory standing.
- Risk ratings are stale because periodic reviews are not triggered or completed on schedule.
- Offboarding decisions are not consistently applied, allowing high-risk relationships to persist.

### Typical Controls

- Real-time and batch sanctions screening with automated list updates and documented hit disposition workflows.
- Risk-based transaction monitoring with documented typologies, tuning rationale, and below-the-line testing.
- Fraud detection models with scheduled performance reviews, champion-challenger testing, and documented model risk management.
- Alert prioritization and aging controls with SLA monitoring and escalation triggers.
- Standardized onboarding risk decisioning logic applied consistently across channels and products.
- CDD and EDD checklists with documented completion, escalation for exceptions, and periodic review scheduling.
- SAR quality review workflow with supervisory approval, narrative standards, and filing deadline tracking.
- Case management with required evidence, activity logs, disposition documentation, and retention.
- Model and rule change governance with testing, approval, rollback capability, and post-deployment monitoring.
- 314(a) search automation with response tracking and deadline compliance.
- Risk rating recalculation triggers on material events (profile change, alert, transaction threshold).
- Independent testing of BSA/AML program components on a risk-based schedule.
- Dual control for high-impact risk decisions (offboarding, SAR filing, model deployment).

## 10. Typical Systems and Providers

These are representative patterns, not prescribed architecture.

### Typical system categories

- Transaction monitoring platform (AML/BSA)
- Fraud detection and scoring engine
- Sanctions and watchlist screening platform
- Case management and investigation platform
- SAR and regulatory filing system
- Customer risk rating engine
- Payment risk decisioning engine (real-time)
- Model governance and validation tools
- Alert management and workflow platform
- Regulatory information-sharing response tools
- GRC / compliance issue tracking platform

### Representative providers often seen around this domain

- `prov.nice_actimize` — Transaction monitoring, fraud detection, SAR filing, and case management
- `prov.featurespace` — Adaptive behavioral analytics for fraud detection and AML monitoring
- `prov.feedzai` — Real-time fraud detection and payment risk decisioning
- `prov.fico_falcon` — Card and payment fraud detection and scoring
- `prov.sas_financial_crimes` — Transaction monitoring, alert management, and case investigation
- `prov.oracle_financial_crimes` — AML monitoring, case management, and regulatory reporting
- `prov.lexisnexis_risk_solutions` — Sanctions screening, identity risk, and watchlist management
- `prov.dow_jones_risk_and_compliance` — Sanctions, PEP, and adverse media screening data
- `prov.world_check_refinitiv` — Sanctions, PEP, and entity screening data
- `prov.alloy` — Onboarding risk decisioning and orchestration of risk signals
- `prov.sardine` — Fraud detection with device intelligence and behavioral biometrics
- `prov.unit21` — Transaction monitoring, case management, and SAR filing for fintechs
- `prov.hummingbird` — BSA/AML case management and SAR filing
- `prov.socure` — Identity fraud risk scoring adjacency
- `prov.chainalysis` / `prov.elliptic` — Cryptocurrency transaction monitoring and screening

Provider placement should remain careful: many vendors span multiple domains. In the canonical model, vendors should link to capabilities they support without redefining domain ownership.

## 11. Open Questions

- **Convergence of fraud and AML operations**: Should `cap.transaction_monitoring` and `cap.fraud_detection_and_scoring` remain separate capabilities, or should they be merged as institutions converge their financial-crime operations? Recommendation: keep them separate in v1 because regulatory expectations, model types, and operational workflows still differ materially, but flag for reassessment as convergence matures.
- **Model risk management scope**: Should `cap.fraud_rules_and_model_governance` cover only financial-crime models, or extend to all risk models used across the institution? Recommendation: scope to financial-crime and fraud models only; enterprise model risk management is a broader concern that may warrant its own domain or capability.
- **Cryptocurrency and digital asset coverage**: How should the domain handle cryptocurrency transaction monitoring, DeFi risk, and digital asset sanctions compliance as these become mainstream banking activities? Recommendation: include within existing capabilities (transaction monitoring, screening) with specialized typologies and provider integrations rather than creating separate capabilities.
- **Real-time vs batch decisioning boundaries**: Should `cap.payment_and_transfer_risk_decisioning` be decomposed into real-time (authorization-path) and batch (post-transaction) sub-capabilities? Recommendation: keep as a single capability in v1 with latency requirements documented at the process level; decompose only if operational complexity demands it.
- **Consortium and industry data sharing**: Should the domain model a distinct capability for consortium data sharing (e.g., fraud consortiums, SAR-sharing under 314(b)), or is this sufficiently covered by `cap.regulatory_information_sharing_and_law_enforcement_response`? Recommendation: monitor whether consortium fraud data sharing grows distinct enough from regulatory information sharing to warrant its own capability; for now, 314(b) is covered, and fraud consortium participation can be modeled as a provider integration.

## 12. Provenance

This artifact defines the Risk & Fraud domain for the nebulus-landscape canonical model based on authoritative regulatory sources including the FinCEN Customer Due Diligence Rule, FFIEC BSA/AML Examination Manual, OFAC Compliance Framework, and FATF Risk-Based Approach Guidance for the Banking Sector. It establishes 17 durable capabilities covering the full financial-crime and fraud decisioning lifecycle from risk appetite definition through detection, investigation, reporting, and program governance. Domain boundaries are drawn to preserve the principle that Risk & Fraud owns the decision layer while other domains own execution, ensuring clean integration with Customer, Identity & Access, Accounts, Payments, and other adjacent domains in the landscape.
