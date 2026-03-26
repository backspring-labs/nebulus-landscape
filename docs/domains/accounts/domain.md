---
id: domain.accounts
type: domain
name: Accounts
status: draft
source_refs: []
last_reviewed: 2026-03-25
confidence: high
---

# Accounts

## 1. Definition

The Accounts domain is the durable business area responsible for creating, governing, servicing, constraining, and terminating the account contract between a financial institution and a customer for deposit products such as checking and savings.

It owns the customer-facing account relationship once the party has been established by `domain.customer`. It answers:

1. **What account has been opened, and under what product terms?**
2. **What is the account's lifecycle state?**
3. **What actions are allowed on the account right now?**
4. **When and how is the account changed, restricted, closed, converted, or archived?**

This domain does **not** own the business-customer relationship, digital access trust, payment-rail execution, card processing, or GL mechanics.

### Accounts vs Ledger boundary

A critical boundary exists between this domain and `domain.ledger_core`:

- **Accounts** owns the customer/account contract and product lifecycle — account number, ownership, product eligibility, terms, status, restrictions, statements, maintenance, closure, conversion.
- **Ledger** owns financial books-and-records — journal entries, posting logic, double-entry integrity, balance computation, settlement/posting pipelines, financial reconciliation.

**Boundary test:** "What product/account relationship exists?" → Accounts. "What accounting entries were posted?" → Ledger.

## 2. Business Purpose

Turn a known party into a governed deposit-product holder.

Without a coherent Accounts domain:

- onboarding cannot transition to product establishment,
- customers cannot hold governed product relationships,
- product terms, ownership, and restrictions fragment across systems,
- downstream domains lose a stable account anchor for payments, cards, statements, and servicing.

In banking and fintech, Accounts is the structural domain that bridges customer onboarding to ongoing product usage. Every payment, card transaction, interest accrual, statement, and servicing interaction ultimately references an account contract governed here.

## 3. Scope

### Includes

The Accounts domain includes capabilities required to:

- create deposit account contracts under defined product terms,
- define and manage deposit account product configurations, eligibility rules, and feature sets,
- assign, change, and remove account ownership and party roles (primary, joint, beneficiary, authorized signer, etc.),
- manage account terms, features, and constraints over the account lifecycle,
- manage account funding readiness and initial-funding coordination,
- govern account lifecycle state including active, dormant, restricted, frozen, and closed states,
- perform account maintenance and change management (address, product feature, nickname, preferences, etc.),
- apply, manage, and release holds, blocks, restrictions, and constraints on accounts,
- manage overdraft configuration, protection linkages, and related product constraints,
- produce and manage account statements, cycle processing, and related disclosures,
- configure and manage account-level interest accrual parameters and fee schedules,
- coordinate dormancy detection, escheatment processing, and unclaimed-property obligations,
- close accounts and manage closure processing, final disbursement coordination, and post-closure obligations,
- convert or migrate accounts between product types or platforms,
- manage account record retention, archival, and post-closure record obligations,
- handle account-level exceptions, remediation, and error correction.

### Excludes

- **Customer relationship creation, maintenance, and lifecycle** → `domain.customer`
  - Customer owns the business relationship and canonical customer record.
- **Authentication, authorization, and access control** → `domain.identity_access`
  - Identity & Access governs who can act on accounts; Accounts governs what the account is.
- **Fraud, AML, sanctions, and broader risk decisioning** → `domain.risk_fraud`
  - Risk & Fraud may inform account restrictions but does not own the account contract.
- **Payment execution and money movement** → `domain.payments`
  - Payments moves money; Accounts holds the governed product relationship.
- **Card issuance, processing, and lifecycle** → `domain.cards`
  - Cards may link to accounts but are governed as a separate product domain.
- **GL posting, balance computation, and financial reconciliation** → `domain.ledger_core`
  - Ledger owns books-and-records; Accounts owns the product/contract layer.
- **Workflow engines and generic orchestration tooling** → `domain.orchestration_control`
  - Accounts consumes orchestration services but does not own workflow infrastructure.
- **Channel UX and screen presentation** → `domain.channels_experience`
  - Accounts defines account state and rules; channels present the user experience.
- **Case management, complaints, dispute handling** → `domain.servicing`
  - Servicing owns case lifecycle; Accounts owns the account contract being serviced.
- **Customer analytics, rewards, and loyalty treatment** → `domain.data_rewards`

### Boundary recommendations

- **Account contract vs customer relationship**: The account is a product contract in `domain.accounts`; the customer/party is a relationship record in `domain.customer`. A customer may hold many accounts; an account may have many parties.
- **Account status vs access control**: Account restrictions (frozen, blocked) are contract-level states owned here; access restrictions (locked out, MFA required) belong in `domain.identity_access`.
- **Account interest/fee configuration vs GL posting**: Accounts owns the product-level configuration of interest and fee parameters; Ledger owns the computation, posting, and balance impact.
- **Account holds vs payment holds**: Account-level holds and constraints belong here; payment-level holds during clearing/settlement belong in `domain.payments`.
- **Overdraft configuration vs overdraft execution**: Accounts owns the product configuration and protection linkage; the actual funds-availability decision at transaction time may involve `domain.ledger_core` or `domain.payments`.

## 4. Included Capabilities

The following capabilities are proposed as the durable capability set for `domain.accounts`.

- `cap.deposit_account_product_management` — Define and manage deposit account product configurations, eligibility rules, feature sets, and product catalog entries for checking, savings, and related deposit types.
- `cap.account_opening` — Create a new deposit account contract, assign an account number, link to the governing product, and establish the account in an initial lifecycle state.
- `cap.account_application_and_contract_formation` — Manage the application process, product eligibility determination, disclosure delivery, consent capture, and contract formation for new deposit accounts.
- `cap.account_party_role_management` — Assign, change, and remove party roles on an account (primary owner, joint owner, beneficiary, authorized signer, custodian, fiduciary, etc.).
- `cap.account_terms_and_feature_management` — Manage the specific terms, features, and constraints applied to an account over its lifecycle (rate tier, fee schedule, transaction limits, feature flags, etc.).
- `cap.account_funding_readiness_management` — Manage the account's readiness to receive initial funding, coordinate funding-window obligations, and track initial-deposit status.
- `cap.account_status_and_restriction_management` — Govern the account's lifecycle state (active, inactive, dormant, restricted, frozen, closed) and manage transitions between states.
- `cap.account_maintenance_management` — Perform routine and non-routine changes to account attributes (address of record, account nickname, delivery preferences, product feature changes, etc.).
- `cap.account_hold_and_constraint_management` — Apply, manage, and release holds, blocks, levies, garnishments, and other constraints on accounts.
- `cap.account_statement_and_cycle_management` — Produce periodic statements, manage statement cycles, deliver disclosures, and maintain statement history.
- `cap.account_interest_and_fee_configuration_management` — Configure and manage account-level interest accrual parameters, rate structures, fee schedules, and fee waiver rules.
- `cap.account_overdraft_and_protection_management` — Configure overdraft settings, manage overdraft protection linkages, and govern overdraft-related product features.
- `cap.account_dormancy_and_escheat_coordination` — Detect dormancy conditions, manage reactivation, coordinate escheatment processing, and fulfill unclaimed-property obligations.
- `cap.account_closure_management` — Close accounts, manage closure processing, coordinate final balance disbursement, and fulfill post-closure obligations.
- `cap.account_conversion_and_migration_management` — Convert accounts between product types, migrate accounts across platforms, and manage conversion/migration lifecycle.
- `cap.account_record_retention_and_archival` — Manage account record retention schedules, archival processing, and post-closure record access obligations.
- `cap.account_exception_and_remediation_management` — Handle account-level exceptions, error corrections, remediation actions, and exception-driven state adjustments.

### Capability notes

A few boundaries are likely to be blurry:

- `cap.account_opening` vs `cap.account_application_and_contract_formation`: opening creates the account record; application/contract formation manages the broader intake, eligibility, disclosure, and consent process. In simpler models these may merge.
- `cap.account_status_and_restriction_management` vs `cap.account_hold_and_constraint_management`: status manages lifecycle state transitions; holds/constraints manage specific encumbrances that may or may not trigger a status change.
- `cap.account_interest_and_fee_configuration_management` vs Ledger posting: Accounts owns the product configuration (what rate tier, what fee schedule); Ledger owns the computation and posting of those values.
- `cap.account_dormancy_and_escheat_coordination` may interact heavily with `domain.risk_fraud` for suspicious-activity holds and `domain.orchestration_control` for state-specific escheatment workflows.
- `cap.account_conversion_and_migration_management` spans both product-level conversion (checking to savings) and platform migration. These may eventually split if the repo deepens.

## 5. Excluded Capabilities

- `cap.customer_identity_establishment` → `domain.customer`
  - Establishes the institution's canonical customer identity record at the business relationship level.
- `cap.customer_profile_management` → `domain.customer`
  - Maintains the business customer profile, not account contracts.
- `cap.customer_due_diligence_coordination` → `domain.customer`
  - Coordinates due diligence collection; Accounts consumes the result.
- `cap.authentication` / `cap.authorization_and_entitlements` → `domain.identity_access`
  - Access trust and control, not account contract governance.
- `cap.fraud_decisioning` / `cap.sanctions_screening` / `cap.kyc_aml_decisioning` → `domain.risk_fraud`
  - Risk decisions may inform account restrictions but are owned externally.
- `cap.payment_execution` / `cap.payment_clearing` / `cap.payment_settlement` → `domain.payments`
  - Money movement execution, not account contract management.
- `cap.card_issuance` / `cap.card_lifecycle_management` → `domain.cards`
  - Card product lifecycle, not deposit account lifecycle.
- `cap.journal_entry_posting` / `cap.balance_computation` / `cap.financial_reconciliation` → `domain.ledger_core`
  - Books-and-records mechanics, not account product governance.
- `cap.workflow_engine_management` / `cap.policy_engine_execution` → `domain.orchestration_control`
  - Generic workflow/policy tooling.
- `cap.channel_session_experience` → `domain.channels_experience`
  - Presentation-layer concerns.
- `cap.case_management` / `cap.dispute_handling` → `domain.servicing`
  - Case and dispute ownership.
- `cap.customer_analytics` / `cap.rewards_management` → `domain.data_rewards`

## 6. Upstream Dependencies

The Accounts domain typically depends on:

- `domain.customer`
  - provides the established party/customer context, KYC/CDD completion status, and relationship state required before an account contract can be created.
- `domain.identity_access`
  - provides authenticated and authorized access context for account opening, maintenance, and sensitive account actions.
- `domain.risk_fraud`
  - provides fraud, AML, and sanctions decisioning outcomes that may gate account opening, trigger restrictions, or inform closure.
- `domain.orchestration_control`
  - may provide workflow sequencing, policy routing, and exception orchestration for complex account lifecycle flows (opening, conversion, escheatment).
- `domain.channels_experience`
  - provides the digital, branch, or assisted entry points through which account actions are initiated.

## 7. Downstream Dependencies

The Accounts domain commonly provides account contract context to:

- `domain.payments`
  - payment execution depends on a valid, unrestricted account with appropriate product features.
- `domain.cards`
  - card issuance and processing depend on a linked deposit account contract.
- `domain.ledger_core`
  - GL posting, balance computation, and settlement depend on the account contract, product terms, and account status.
- `domain.servicing`
  - case management, inquiries, and dispute handling reference the account contract.
- `domain.channels_experience`
  - consumes account state, product features, restrictions, and balances to shape user experience.
- `domain.data_rewards`
  - analytics, rewards, and loyalty treatments reference account relationships and product holdings.
- `domain.risk_fraud`
  - consumes account state, ownership changes, and restriction events as inputs to ongoing risk monitoring.
- `domain.customer`
  - account holdings inform the customer relationship view, product cross-sell context, and relationship summary.

## 8. Related Journeys

Journeys that belong to or materially pass through `domain.accounts` include:

- `journey.open_checking_account`
- `journey.open_savings_account`
- `journey.add_joint_owner_to_account`
- `journey.remove_party_from_account`
- `journey.change_account_product_type`
- `journey.close_deposit_account`
- `journey.place_account_hold`
- `journey.release_account_hold`
- `journey.set_up_overdraft_protection`
- `journey.reactivate_dormant_account`
- `journey.process_escheatment`
- `journey.update_account_preferences`
- `journey.generate_account_statement`
- `journey.fund_new_account`

Some of these journeys will later span multiple domains. They are listed here because Accounts is either the primary business anchor or a mandatory participating domain.

## 9. Typical Risks and Controls

### Typical Risks

- Accounts are opened without adequate customer due diligence or identity verification.
- Account ownership or party roles are incorrectly assigned, changed, or not removed when required.
- Product terms, features, or constraints are misconfigured or inconsistently applied.
- Account restrictions or holds are not applied promptly when required by legal, regulatory, or risk events.
- Dormant accounts are not detected, managed, or escheated in compliance with state/jurisdictional requirements.
- Account closure processing is incomplete, leaving residual obligations or orphaned records.
- Account conversions or migrations introduce data integrity issues or service disruptions.
- Account statements or disclosures are inaccurate, delayed, or not delivered.
- Overdraft configurations expose customers to unintended fees or the institution to unintended credit risk.
- Account records are not retained or archived in compliance with regulatory retention schedules.

### Typical Controls

- Account opening gated by verified customer identity, completed due diligence, and product eligibility checks.
- Party role assignment and removal governed by documented authorization and audit trail.
- Product term and feature configuration validated at opening and at each maintenance event.
- Restriction and hold application enforced through automated triggers and manual review workflows.
- Dormancy detection schedules with automated notification, reactivation windows, and escheatment processing.
- Closure checklists ensuring final balance disbursement, linked-product cleanup, and record archival.
- Conversion/migration controls including pre-migration validation, reconciliation, and rollback procedures.
- Statement generation and delivery validation with exception handling for failures.
- Overdraft configuration governed by product policy, customer opt-in, and regulatory compliance rules.
- Record retention schedules enforced through automated archival and purge controls.

## 10. Typical Systems and Providers

These are representative patterns, not prescribed architecture.

### Typical system categories

- core banking / deposit account system of record
- account opening and origination platform
- product configuration and catalog management
- account servicing and maintenance platform
- statement generation and delivery platform
- dormancy and escheatment processing system
- account conversion and migration tooling
- hold and restriction management system
- overdraft and protection configuration engine

### Representative providers often seen around this domain

- `prov.fis` — core banking, deposit account processing, account origination adjacency
- `prov.fiserv` — core banking, account processing, statement generation adjacency
- `prov.jack_henry` — core banking, deposit account system of record adjacency
- `prov.temenos` — core banking, deposit product management adjacency
- `prov.thought_machine` — cloud-native core banking, deposit product configuration adjacency
- `prov.mambu` — cloud-native core banking, deposit account processing adjacency
- `prov.finxact` — cloud-native core banking, deposit account platform adjacency
- `prov.q2` — digital banking, account opening, account servicing adjacency
- `prov.alkami` — digital banking, account opening adjacency
- `prov.narmi` — digital account opening adjacency
- internal core banking, account origination, and servicing stacks

Provider placement should remain careful: many core banking vendors span Accounts, Ledger, Payments, and Cards concerns simultaneously. Providers should enrich the model, not redefine domain ownership.

## 11. Open Questions

- Should `cap.account_opening` and `cap.account_application_and_contract_formation` remain separate, or merge into a single capability if the repo stays at a business-architecture level rather than decomposing to process detail?
- How strongly should deposit accounts and other account types (loan, credit, investment) be modeled together versus as separate domains? This domain currently scopes to deposit accounts only.
- Should `cap.account_interest_and_fee_configuration_management` remain here, or shift partially to `domain.ledger_core` if fee/interest configuration is tightly coupled to posting logic in practice?
- Where should the boundary sit between account-level holds governed here and payment-level holds governed in `domain.payments` when a single regulatory event (e.g., garnishment) affects both?
- How first-class should sub-account or virtual-account constructs become if the institution supports pooled or omnibus account structures?
- Should escheatment coordination remain a capability within Accounts, or become a cross-domain process coordinated by `domain.orchestration_control` with Accounts as a participant?
- How should account conversion/migration interact with `domain.ledger_core` when product conversion requires simultaneous changes to both the account contract and the GL treatment?

## 12. Provenance

This artifact is derived from boundary analysis established during Customer and Identity & Access domain research, which clarified that `domain.customer` owns the business relationship, `domain.identity_access` owns access trust, and `domain.accounts` owns the deposit account contract and product lifecycle. The Accounts domain resolves the question of where product terms, ownership, lifecycle state, restrictions, maintenance, and closure are governed by formalizing Accounts as the institution's deposit product contract layer: the place where a known party becomes a governed account holder, and where that account relationship is maintained, constrained, converted, or terminated over its full lifecycle. The Accounts-vs-Ledger boundary is explicitly defined to separate product/contract governance from books-and-records mechanics.
