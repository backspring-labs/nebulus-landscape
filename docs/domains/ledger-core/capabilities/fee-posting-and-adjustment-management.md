---
id: cap.fee_posting_and_adjustment_management
type: capability
name: Fee Posting and Adjustment Management
domain_id: domain.ledger_core
status: draft
source_refs: []
last_reviewed: 2026-04-02
confidence: medium
---

# Fee Posting and Adjustment Management

## 1. Definition

Fee Posting and Adjustment Management is the enduring business ability to post fees, waivers, refunds, rebates, and associated adjustments into the accounting record while preserving linkage to the originating assessment and the full correction history of each fee entry.

## 2. Business Outcome

Ensure that every fee appearing in the ledger is traceable to a valid assessment, that adjustments such as waivers and refunds are governed and linked to the original charge, and that fee revenue is accurately recognized without duplication, omission, or distortion from ungoverned corrections.

## 3. Scope

### Included activities
- Posting assessed fees to the ledger as governed journal entries
- Posting fee waivers with linkage to the waived assessment and waiver reason
- Posting fee refunds with linkage to the original fee charge
- Posting fee rebates and adjustments with explicit correction references
- Preventing duplicate fee postings for the same assessment
- Reconciling assessed fees against posted fee entries
- Maintaining linkage between fee adjustments and the original fee posting

### Excluded activities
- Fee calculation and assessment logic (`cap.fee_assessment_management`)
- Fee schedule design and product pricing administration (product domain)
- General posting execution for non-fee entries (`cap.posting_execution_management`)
- Customer notification of fee charges or refunds (channel domain)
- Fee dispute investigation and resolution (customer service domain)

## 4. Upstream Dependencies
- Fee assessment records from `cap.fee_assessment_management` providing the basis for each posting
- Journal entry construction rules from `cap.journal_entry_construction_management` for fee-specific accounting treatment
- Account and product configuration for fee GL mapping and revenue recognition rules
- Waiver and refund authorization decisions from upstream fee governance processes

## 5. Downstream Dependencies
- `cap.balance_computation_management` for updating account balances after fee postings
- `cap.interest_accrual_management` where fee postings affect accrual-eligible balances
- `cap.financial_audit_trail_and_lineage_management` for preserving the assessment-to-posting chain
- Fee revenue reporting and finance reconciliation processes
- Customer-facing transaction history and statement capabilities

## 6. Related Value Streams
- `vs.fee_economics`
- `vs.financial_truth_creation`
- `vs.accounting_control`

## 7. Related Journeys
- `journey.execute_monthly_fee_assessment`
- `journey.reverse_erroneous_posting`

## 8. Likely Processes
- Fee posting cycle execution converting assessed fees to ledger entries
- Fee waiver posting with governed waiver reason codes and approval linkage
- Fee refund posting with mandatory linkage to the original fee charge
- Fee rebate and adjustment posting with explicit correction references
- Duplicate fee post detection and prevention
- Assessed-versus-posted fee reconciliation
- Fee revenue recognition alignment with posting timing

## 9. Risks
- Fee posted without a valid assessment, creating an unsubstantiated charge to the customer
- Duplicate fee posting where the same assessed fee is posted more than once
- Refund not linked to the original fee, making it impossible to trace the correction to its cause
- Revenue distortion from incorrect fee adjustment where a waiver, refund, or rebate is applied to the wrong amount or period
- Fee adjustment applied without proper authorization, creating ungoverned revenue leakage
- Timing mismatch between fee assessment and posting creating temporary balance inaccuracies

## 10. Controls
- Assessment-to-posting linkage requiring every fee posting to reference a valid, unposted assessment record
- Duplicate fee post prevention ensuring each assessment can only be posted once
- Controlled fee adjustment treatment requiring explicit reason codes, approval chains, and original-fee linkage for all waivers, refunds, and rebates
- Assessed-versus-posted fee reconciliation detecting gaps between what was assessed and what reached the ledger
- Fee adjustment authorization controls ensuring waivers and refunds are approved according to governance thresholds
- Revenue recognition alignment controls ensuring fee postings and adjustments are reflected in the correct accounting period

## 11. Typical Systems/Providers

### Typical systems
- Fee posting engines
- Fee adjustment services
- Fee revenue recognition services
- Assessment-to-posting linkage stores

### Typical providers
- Temenos
- Thought Machine
- Mambu
- Fiserv
- In-house fee processing platforms

## 12. Open Questions
- Should fee posting be synchronous with assessment or decoupled into a separate batch cycle?
- How should the system handle partial refunds where only a portion of the original fee is returned?
- What is the correct treatment when a fee assessment is reversed after the fee has already been posted and recognized as revenue?
- Should fee adjustments support retroactive posting to prior periods, and if so, what governance applies?
- How should fee posting handle multi-currency accounts where the fee is assessed in one currency but posted in another?

## 13. Provenance

This capability reflects the banking requirement to govern the posting of fees as a distinct step from fee assessment. While fee assessment determines what should be charged, fee posting ensures that only validly assessed fees reach the ledger and that all subsequent adjustments -- waivers, refunds, rebates -- maintain explicit linkage to the original charge. Regulatory expectations around fee transparency and consumer protection require that every posted fee be traceable to its assessment, and every adjustment be traceable to the fee it corrects. Extracting fee posting and adjustment as a distinct capability allows the institution to independently govern posting accuracy, prevent duplication, and maintain the full correction history needed for audit, dispute resolution, and revenue integrity.
