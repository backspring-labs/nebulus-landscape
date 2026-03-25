# Research Log: Customer Domain Provider Research Needed

**Date:** 2026-03-24
**Domain:** Customer
**Status:** Research logged but not promoted

## Summary

During the Customer domain capability research (Batches 1-3), 33 providers were identified as relevant to Customer domain capabilities. Lightweight YAML stubs have been created in `models/providers/` with basic category, summary, and status: stub.

Full provider research has **not** been conducted. The stubs need to be promoted to research-grade artifacts through the provider mapping workflow described in `prompts/provider-mapping-prompt.md`.

## What needs to happen

For each provider:
1. Research the provider's actual product capabilities and positioning
2. Create a provider mapping (`models/provider-mappings/map.prov.<name>.yaml`) linking the provider to specific capabilities, journeys, processes, risks, and controls
3. Assess confidence in the mapping
4. Identify open questions about provider scope and boundaries
5. Update provider status from `stub` to `active_research` or `draft`

## Priority providers for early research

These providers appear most frequently across Customer domain capabilities and processes:

| Provider | Frequency | Key capability areas |
|----------|-----------|---------------------|
| `prov.salesforce` | High | CRM, relationship management, onboarding workflow |
| `prov.alloy` | High | Onboarding orchestration, identity/risk integration (already has mapping) |
| `prov.informatica_mdm` | High | Party resolution, identity establishment, data quality |
| `prov.reltio` | High | MDM, party resolution, entity resolution |
| `prov.fenergo` | Medium | KYC/AML compliance, due diligence coordination |
| `prov.pega` | Medium | Orchestration, case management, CRM |
| `prov.docusign` | Medium | Disclosure/attestation, e-signature evidence |
| `prov.servicenow` | Medium | Workflow, case management, change governance |

## Providers that span multiple domains

Many of these providers will also appear in Identity & Access, Accounts, Risk & Fraud, and other domains. Provider research should be conducted with awareness that:
- provider placement must remain capability-first, not vendor-first
- the same provider may play different roles in different domains
- confidence in placement may vary by capability area

## Open questions

- Should provider research be conducted domain-by-domain or provider-by-provider across domains?
- Should provider comparison docs (`docs/cross-domain/capability-comparisons/`) be created during this pass or deferred?
- How should provider stubs that span multiple domains be governed once those domains are researched?
