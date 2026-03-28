---
id: cap.payment_fee_and_pricing_application
type: capability
name: Payment Fee and Pricing Application
domain_id: domain.payments
status: draft
source_refs: []
last_reviewed: 2026-03-27
confidence: medium
---

# Payment Fee and Pricing Application

## 1. Definition

Payment Fee and Pricing Application is the enduring business ability to calculate, apply, and manage fees and pricing for payment transactions based on payment type, rail, customer segment, relationship pricing, and contractual terms, ensuring accurate fee assessment, transparent disclosure, and proper revenue recognition across the payment lifecycle.

## 2. Business Outcome

Ensure the institution accurately assesses and collects fees for payment services in accordance with published fee schedules, contractual pricing agreements, and regulatory disclosure requirements, maximizing payment-related revenue while maintaining customer transparency and competitive pricing.

## 3. Scope

### Included activities
- Calculating payment fees based on payment type, rail, amount, frequency, customer segment, and relationship tier
- Managing fee schedules and pricing tables for all payment types and rails
- Determining applicable pricing tiers based on customer relationship, volume commitments, and contractual terms
- Assessing and posting payment fees to customer accounts at appropriate points in the payment lifecycle
- Disclosing fees to customers before payment confirmation in compliance with regulatory requirements
- Processing fee waivers, reversals, and refunds based on policy, promotion, or customer request
- Recognizing payment fee revenue in accordance with accounting standards
- Supporting promotional and introductory pricing for payment services
- Managing corporate and commercial payment pricing agreements and volume-based tiering

### Excluded activities
- Account-level fee management for non-payment fees (e.g., monthly maintenance fees, overdraft fees) (`cap.account_interest_and_fee_configuration_management`)
- Payment execution and processing (`rail-specific execution capabilities`)
- Customer billing and statement generation (`cap.account_statement_and_cycle_management`)
- General ledger and revenue accounting (`domain.finance`)
- Product pricing strategy and competitive analysis (business strategy function)
- Fee-related regulatory filing and reporting (`domain.compliance`)

## 4. Upstream Dependencies
- Payment instruction data from `cap.payment_initiation_management` including payment type, amount, and rail
- Customer segment and relationship tier data from `domain.customer`
- Account type and product data from `domain.accounts`
- Contractual pricing agreements and volume commitment data from `domain.customer` or commercial banking
- Fee schedule and pricing table configuration from product management
- Regulatory fee disclosure requirements from `domain.compliance`
- Payment volume and frequency data for volume-based pricing tier calculation

## 5. Downstream Dependencies
- `domain.accounts` for fee posting and account balance adjustments
- `domain.finance` for fee revenue recognition and general ledger entries
- Customer-facing channels for fee disclosure and confirmation display
- `domain.customer` for fee notification delivery
- `cap.payment_status_and_tracking` for recording fee assessment events in the payment lifecycle
- `cap.account_statement_and_cycle_management` for fee line items on customer statements
- Reporting and analytics for fee revenue tracking and pricing effectiveness analysis

## 6. Related Value Streams
- `vs.revenue_generation`
- `vs.payment_execution`
- `vs.customer_self_service`
- `vs.regulatory_compliance`

## 7. Related Journeys
- `journey.payment_fee_assessment`
- `journey.corporate_payment_pricing_negotiation`
- `journey.payment_fee_disclosure_and_consent`
- `journey.payment_fee_dispute_resolution`
- `journey.payment_fee_waiver_and_refund`

## 8. Likely Processes
- Payment fee calculation based on payment attributes, customer profile, and applicable fee schedule
- Fee schedule and pricing table creation, modification, and version management
- Pricing tier determination based on customer relationship, volume, and contractual terms
- Fee assessment and posting to customer accounts
- Pre-payment fee disclosure and customer consent capture
- Fee waiver request evaluation and approval
- Fee reversal and refund processing
- Fee revenue recognition and accounting entry generation
- Corporate and commercial pricing agreement management
- Promotional and introductory pricing application and expiration management
- Fee exception and override management with appropriate authorization
- Fee analytics and revenue reporting

## 9. Risks
- Incorrect fee calculation due to misconfigured fee schedules, incorrect tier assignment, or calculation errors
- Fee schedule misconfiguration after updates resulting in systematic overcharging or undercharging
- Fee disclosure noncompliance with regulatory requirements (e.g., EFTA, TILA, Regulation E) exposing the institution to regulatory action
- Fee revenue leakage from unapplied fees, incorrect waivers, or pricing agreement misapplication
- Unauthorized fee waiver or override resulting in revenue loss or policy violation
- Inconsistent fee application across channels creating customer fairness concerns
- Fee posting failures resulting in unbilled payment services
- Contractual pricing agreement expiration without renewal or reversion to standard pricing
- Customer complaints and attrition from fee transparency or competitiveness issues

## 10. Controls
- Fee calculation accuracy validation through automated testing and periodic sampling reconciliation
- Fee schedule change approval workflow requiring business owner and compliance review before activation
- Fee disclosure regulatory compliance check ensuring all required disclosures are presented before payment confirmation
- Fee revenue reconciliation comparing assessed fees to posted fees to recognized revenue
- Fee waiver authorization and limit check ensuring waivers are within policy and approved by authorized personnel
- Fee schedule version control and audit trail for all configuration changes
- Fee posting completeness monitoring ensuring all assessed fees are successfully posted
- Contractual pricing agreement expiration monitoring and renewal alerting
- Fee exception and override reporting for management review

## 11. Typical Systems/Providers

### Typical systems
- Payment fee calculation engines
- Payment pricing and rate management platforms
- Fee schedule management and configuration systems
- Fee posting and accounting integration systems
- Fee disclosure and notification systems
- Corporate pricing agreement management platforms

### Typical providers
- FIS
- Fiserv
- Jack Henry
- Temenos
- Finastra
- Oracle Financial Services
- In-house pricing and fee engines

## 12. Open Questions
- Should the capability own the fee schedule and pricing table configuration, or should that be managed by a product management capability with this capability consuming the configuration?
- How should the capability handle multi-currency payment fees where the fee currency may differ from the payment currency?
- What is the appropriate boundary between payment-specific fees (this capability) and account-level fees (`cap.account_interest_and_fee_configuration_management`)?
- How should the capability handle real-time fee calculation and disclosure for instant payments where latency budgets are extremely tight?
- Should interchange and network fees charged by payment networks be within scope, or should those be treated as cost-of-processing managed by a separate capability?
- How should the capability handle fee bundling or packaging where payment fees are included in broader relationship pricing packages?

## 13. Provenance

This capability reflects the importance of payment fees as a significant revenue stream for financial institutions and the increasing regulatory and customer expectations for fee transparency. As payment rails proliferate and pricing models become more complex -- with volume-based tiers, relationship pricing, promotional offers, and real-time fee disclosure requirements -- the ability to accurately calculate, disclose, and apply fees becomes a critical operational capability. The capability is deliberately scoped to fee calculation, disclosure, and application, separated from the account-level fee management and the product pricing strategy that inform fee schedule design. This separation enables institutions to manage payment pricing as a specialized function while maintaining integration with the broader account and revenue management ecosystem.
