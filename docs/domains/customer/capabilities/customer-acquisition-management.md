---
id: cap.customer_acquisition_management
type: capability
name: Customer Acquisition Management
domain_id: domain.customer
status: draft
source_refs: []
last_reviewed: 2026-03-24
confidence: medium
---

# Customer Acquisition Management

## 1. Definition

Customer Acquisition Management is the enduring business ability to attract, capture, qualify, and progress prospective customers into institution-recognized onboarding opportunities. It governs institution-led prospect management before a formal application is submitted and before the party is established as a customer record of authority.

## 2. Business Outcome

Create a controlled, measurable path from market interest to qualified onboarding entry so the institution can grow responsibly, reduce drop-off, and ensure that downstream onboarding starts with the right prospect context, product intent, and acquisition attribution.

## 3. Scope

### Included activities
- Capturing leads, prospects, referrals, and pre-application interest
- Tracking prospect source, campaign, partner, channel, and offer context
- Managing prospect qualification and readiness rules
- Recording expressed product interest and onboarding intent
- Managing appointment, callback, and follow-up progression for pre-application engagement
- Supporting prefill-eligible prospect data handoff into onboarding
- Managing prospect funnel stages such as lead, marketing-qualified, sales-qualified, invite-to-apply, and abandoned pre-application
- Coordinating branch-assisted, call-center-assisted, digital, and partner-originated prospect progression
- Recording acquisition attribution needed for incentive, analytics, or partner settlement purposes

### Excluded activities
- Channel UX, form rendering, ad experience, and campaign presentation (`domain.channels_experience`)
- Formal application capture and validation (`cap.customer_application_intake`)
- Workflow engine execution and generic orchestration tooling (`domain.orchestration_control`)
- Identity proofing or credential issuance (`domain.identity_access`)
- Risk decisioning, fraud screening, or sanctions decisions (`domain.risk_fraud`)
- Account contract creation (`domain.accounts`)

## 4. Upstream Dependencies
- Channel and campaign interactions from `domain.channels_experience`
- Product and offer definitions from product-related domains not yet modeled
- Referral or partner lead feeds from ecosystem/partner sources
- Prospect eligibility rules and policy guidance from compliance, product, and risk functions

## 5. Downstream Dependencies
- `cap.customer_application_intake` for transition from prospect to applicant
- `cap.customer_onboarding_orchestration` for milestone progression after intent hardens into onboarding
- `cap.customer_party_resolution` where existing-customer detection is needed before or during application start
- `cap.customer_identity_establishment` when a prospect becomes a canonical customer/applicant record
- `domain.accounts` once onboarding leads to an account-opening journey

## 6. Related Value Streams
- `vs.customer_growth`
- `vs.customer_onboarding`
- `vs.partner_distribution`
- `vs.deposit_acquisition`

## 7. Related Journeys
- `journey.open_savings_account`
- `journey.open_checking_account`
- `journey.start_application_from_marketing_offer`
- `journey.partner_referred_account_opening`
- `journey.branch_assisted_customer_onboarding`

## 8. Likely Processes
- Lead capture and source attribution
- Prospect qualification
- Referral intake and routing
- Pre-application follow-up
- Abandoned application recovery handoff
- Offer acceptance to apply
- Acquisition funnel monitoring and conversion measurement

## 9. Risks
- Poor-quality or duplicate prospect records distort funnel reporting
- Ineligible or mismatched prospects are inappropriately encouraged into onboarding
- Partner/referral attribution is inaccurate, creating financial, legal, or conduct issues
- Marketing consent or contact permissions are captured incorrectly before outreach
- Prospect data is over-collected before a legitimate onboarding need exists
- Unclear handoff criteria create leakage between acquisition and onboarding
- Incentive abuse occurs through fabricated referrals or repeated acquisition events

## 10. Controls
- Standard prospect stage definitions and entry/exit criteria
- Source attribution controls with immutable campaign/referral stamps
- Duplicate prospect checks before progression to application
- Data minimization standards for pre-application capture
- Contact-permission validation before outbound follow-up
- Controlled qualification rules for promoted offers or invite-only products
- Funnel reconciliation between prospect, application-start, and completed-onboarding counts
- Referral abuse monitoring and partner exception review

## 11. Typical Systems/Providers

### Typical systems
- CRM / prospect management platforms
- Lead management and campaign attribution systems
- Partner onboarding portals
- Branch sales platforms
- Contact center sales workbenches
- CDP / marketing automation platforms
- Prospect analytics and conversion dashboards

### Typical providers
- Salesforce
- Microsoft Dynamics
- HubSpot
- Adobe Experience Cloud
- Braze
- Iterable
- Fiserv, FIS, Jack Henry sales/origination front ends where used in banking distribution
- Embedded-finance or partner distribution platforms in co-brand / marketplace models

## 12. Open Questions
- Where should prospect consent for marketing contact live if broader consent governance later becomes a separate customer capability?
- Should referral and partner-attribution management remain here, or be separated if the landscape later adds a dedicated partner distribution domain?
- At what point should a high-confidence prospect become a party candidate for resolution against the customer master?
- Should abandoned application recovery be anchored here or under onboarding orchestration once an application shell exists?

## 13. Provenance

Derived from the Customer bootstrap artifact and expanded using common banking operating models for prospect-to-onboarding progression. Regulatory framing around customer onboarding and due diligence informs boundary placement, but this capability is primarily commercial and relationship-initiation oriented rather than compliance-decision oriented.
