---
id: cap.fee_assessment_management
type: capability
name: Fee Assessment Management
domain_id: domain.ledger_core
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Fee Assessment Management

## 1. Definition

Fee Assessment Management is the enduring business ability to calculate and assess fees based on configured product terms, event triggers, schedules, or threshold conditions, producing explicit assessment records that serve as the basis for downstream fee posting.

## 2. Business Outcome

Ensure that fees are assessed accurately, consistently, and in accordance with product terms and regulatory requirements, enabling correct fee revenue recognition, transparent customer billing, and defensible fee practices that withstand regulatory scrutiny and customer challenge.

## 3. Scope

### Included activities
- Executing scheduled fee assessment cycles (monthly maintenance fees, annual fees)
- Evaluating event-triggered fee assessments (overdraft fees, wire transfer fees, stop payment fees)
- Applying threshold-based fee logic (minimum balance fees, excess transaction fees)
- Evaluating fee waiver conditions and applying governed waiver rules
- Creating explicit fee assessment records with full traceability to triggers and amounts
- Reconciling assessed fees against posted fee entries
- Maintaining governed fee schedules sourced from account and product configuration

### Excluded activities
- Fee posting to the ledger (`cap.posting_execution_management`, `cap.journal_entry_construction_management`)
- Fee schedule design and product pricing administration (product domain)
- Fee refund and reversal processing (`cap.posting_adjustment_and_reversal_management`)
- Customer fee disclosure and notification (channel domain)
- Fee dispute handling and resolution (customer service domain)

## 4. Upstream Dependencies
- Fee schedules and product terms from account and product configuration
- Account balance and activity data from `cap.balance_computation_management` for threshold evaluation
- Transaction events from originating domains that trigger event-based fee assessment
- Waiver eligibility criteria from relationship and product configuration

## 5. Downstream Dependencies
- `cap.posting_execution_management` for converting assessed fees to posted ledger entries
- `cap.balance_computation_management` for updating balances after fee posting
- Customer-facing transaction history and statement capabilities showing fee charges
- Fee revenue reporting and analytics capabilities
- Regulatory fee disclosure and compliance reporting

## 6. Related Value Streams
- `vs.fee_economics`
- `vs.accounting_event_processing`
- `vs.financial_truth_creation`

## 7. Related Journeys
- `journey.execute_monthly_fee_assessment`
- `journey.process_end_of_day_close`

## 8. Likely Processes
- Scheduled fee assessment cycle execution across eligible accounts
- Event-triggered fee evaluation upon qualifying transaction or account events
- Threshold condition evaluation against current balance and activity levels
- Fee waiver eligibility evaluation and governed waiver application
- Fee assessment record creation with trigger, amount, and rule traceability
- Assessed-versus-posted fee reconciliation
- Fee schedule version management and effective date handling

## 9. Risks
- Wrong fee amount assessed due to incorrect schedule application or calculation error
- Fee charged that should have been waived, resulting in customer harm and regulatory exposure
- Missed assessable fee where a qualifying event or threshold condition is not detected
- Inconsistent threshold treatment where the same balance or activity level produces different fee outcomes across accounts
- Fee schedule version conflicts where an outdated schedule is applied after a pricing change
- Assessment timing errors where fees are assessed against stale balance or activity data

## 10. Controls
- Governed fee schedules sourced from account configuration, ensuring fee amounts and rules are centrally managed
- Explicit assessment records documenting the trigger, calculation inputs, and resulting amount for every fee
- Trigger-to-amount traceability ensuring every assessed fee can be explained from its originating event or condition
- Controlled waiver handling with explicit waiver reason codes and approval governance
- Assessed-versus-posted fee reconciliation ensuring all assessed fees are eventually posted or explicitly cancelled
- Fee schedule effective date enforcement preventing application of expired or not-yet-active fee schedules

## 11. Typical Systems/Providers

### Typical systems
- Fee assessment engines and rules processors
- Fee trigger services and event listeners
- Threshold evaluation services
- Waiver evaluation services
- Fee assessment record stores

### Typical providers
- Temenos
- Thought Machine
- Mambu
- Fiserv
- In-house fee processing platforms

## 12. Open Questions
- Should fee assessment be synchronous (evaluated inline with triggering events) or asynchronous (evaluated in batch cycles)?
- How should the system handle fee assessment for accounts with complex relationship-based waiver rules spanning multiple products?
- What is the correct behavior when a fee trigger event is reversed after the fee has already been assessed but before it is posted?
- Should fee assessment support retroactive reassessment when product terms change with effective dates in the past?
- How should the system handle fee assessment during account migration when fee schedules differ between source and target platforms?

## 13. Provenance

This capability reflects the banking requirement to accurately and transparently assess fees as a distinct step before posting them to the ledger. Fee assessment involves complex logic including scheduled cycles, event triggers, threshold conditions, and waiver rules that must be governed independently from the posting mechanism. Regulatory scrutiny of fee practices, particularly for consumer accounts, requires that every fee be traceable to its trigger, calculated according to disclosed terms, and subject to appropriate waiver evaluation. Extracting fee assessment as a distinct capability allows the institution to independently govern fee calculation accuracy, waiver consistency, and the audit trail between fee triggers and posted charges.
