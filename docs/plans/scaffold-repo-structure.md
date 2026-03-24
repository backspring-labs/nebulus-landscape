# Plan: Bootstrap nebulus-landscape repo grammar and first vertical slice

## Context

`nebulus-landscape` is a canonical business architecture knowledge model for the fintech/banking landscape. It will serve as the data substrate for **guiderail** (guided journey traversal UI) and the upcoming **nebulus reference bank** app. The repo currently has only a README and three idea documents in `docs/ideas/`.

The guiderail repo (`@guiderail/core`) already defines Zod-validated domain model types (Domain, Capability, ValueStream, Journey, Step, Process, ProcessStage, Provider, Interface, Node, Edge, etc.) with a `ProvenanceRef` system. The landscape YAML models must produce data that aligns with these types so guiderail can consume them.

Three idea documents describe the target vision:
1. **Canonical Model Master** — full repo structure, object model, schemas, templates, prompts
2. **Provider Research Model** — provider layering (profile, research, mapping)
3. **Data/Analytics/Agency Addendum** — extended object types (deferred to later phase)

## Framing

This is a **repo grammar and bootstrap phase**, not a content-completeness phase. Phase 1 establishes the canonical repo grammar for `nebulus-landscape`: folder structure, principles, templates, prompts, stable IDs, refresh rules, and one seeded vertical slice that proves the model can carry a coherent thread from domain through process stage, provider mapping, and system linkage while remaining aligned to future GuideRail ingestion.

Phase 1 is about establishing the canonical repo grammar, not about achieving broad landscape completeness. This protects the effort from ballooning into modeling half the banking industry before the repo structure is stable.

### Triad context
`nebulus-landscape` is one leg of a three-project triad:
- **nebulus-landscape** — canonical domain model (what should exist and why)
- **nebulus-financial** — reference application (one realization of that truth in running code)
- **guiderail** — guided navigation across both layers, linked by stable IDs

The goal is to maintain this triad to evaluate new patterns, practices, providers, and emerging concepts. Landscape establishes the design authority. Financial implements selected parts. Guiderail makes both navigable — showing what's modeled, what's realized, and where the gaps are. Phase 1 focuses on landscape only, but the structure and ID conventions are designed with the full triad in mind.

## Approach

Scaffold the repo with the core structure, templates, and a seeded first domain (Customer) as a working example. Defer the addendum objects and full domain coverage to future authoring phases.

## Scope rules

- Phase 1 does **not** introduce DataObject, Decision, Policy, Signal, AgentRole, DelegationBoundary, or ObservabilityArtifact files, even though the repo structure should not block them later.
- Templates and scaffolding should accelerate authored thinking, not encourage empty or low-signal placeholder artifacts that look complete but say very little. If a stub exists, it should either carry real content or be explicitly marked as a placeholder with a clear reason for existing.
- Any placeholder or deferred document created in Phase 1 must be **visibly marked as skeletal** (e.g., a `status: placeholder` field or a clear "This is a scaffold placeholder" note) so the repo does not imply false completeness.
- `confidence` is **allowed but optional** in Phase 1. Seeded artifacts may include confidence values where the author has a basis for them, but confidence is not required on every object until the model matures. The field must be present in templates and schemas so the shape is established.

## Steps

### 1. Project setup files
- `.gitignore` (OS files, editor files, etc.)
- `CLAUDE.md` — project-specific instructions for Claude including:
  - ID conventions and naming rules
  - Modeling rules (see step 1a below)
  - Authoring workflow
  - GuideRail alignment notes (ID mapping, provenance shape)
- Update `README.md` with project purpose, structure overview, and relationship to sibling repos

#### 1a. Repo authoring rules in CLAUDE.md
Include explicit rules for future artifact generation:
- Capability-first, not vendor-first
- Journey is not process
- Value stream frames but does not replace capability/journey
- Provenance must be captured when interpretation is involved
- IDs are stable and canonical once introduced
- Risk/control thinking is integral, not bolted on

### 2. Core folder structure
Create the directory skeleton from the Canonical Model doc, scoped to Phase 1:

```
docs/
  ideas/              (already exists)
  plans/              (already exists)
  principles/         (modeling rules, naming, confidence levels)
  research-log/       (dated refresh notes per domain review cycle)
  domains/
    customer/         (seeded first domain)
      domain.md
      capabilities/
      journeys/
      processes/
      risks-controls/
      providers/
      mappings/
    identity-access/  (empty skeleton)
    accounts/         (empty skeleton)
  cross-domain/
    value-streams/
    journeys/
    providers/
    capability-comparisons/
    reference-taxonomy/
  sources/

models/
  domains/
  capabilities/
  value-streams/
  journeys/
  processes/
  process-stages/
  risks/
  controls/
  providers/
  provider-mappings/
  provider-research/
  systems/
  interfaces/
  messages/
  evidence/
  sources/

mappings/

templates/

prompts/
```

### 3. Principles docs
Create foundational principles files:
- `docs/principles/modeling-principles.md` — core design rules from the idea docs
- `docs/principles/naming-conventions.md` — stable ID strategy, file naming
- `docs/principles/confidence-levels.md` — confidence values and when to use them
- `docs/principles/research-refresh-convention.md` — how to do incremental domain research refreshes
- `docs/principles/realization-status.md` — defines the lifecycle states that guiderail uses to distinguish landscape-modeled vs. financial-realized entities
- `docs/principles/bridge-contract.md` — defines how nebulus-financial will reference landscape canonical IDs (manifest shape, annotation conventions, TBD details deferred until financial begins)

#### Realization status concept
Every entity in the landscape can be in one of three lifecycle states from guiderail's perspective:
1. **Modeled** — exists in landscape, not yet implemented in financial. Guiderail can show the business architecture (domain, capability, journey, process, provider research) but there is no code to tour. This is authored research with provenance, not vapor.
2. **Realized** — exists in landscape AND implemented in financial. Guiderail can show both the concept and the code. This is the gold standard.
3. **Implemented but unmodeled** — exists in financial code but doesn't yet have a clean landscape artifact. Guiderail can still tour the code.

These are not failure states — they are lifecycle states. Every capability moves through modeled → realized as financial grows. Some may stay modeled-only long-term (research or future candidates). Guiderail must be honest about which state something is in.

These are not failure states — but they are not all equal:
- "Implemented but unmodeled" is a valid **discovery state**, but should generally trigger follow-up modeling work rather than remain the desired long-term condition. If financial implements something that landscape doesn't model, the triad is drifting and repair work is needed.

This concept is documented in Phase 1 as vocabulary. It does not require a field on every YAML file today, but it establishes the language that guiderail will use to bridge the gap between landscape completeness and financial implementation coverage.

In Phase 1, realization status is documented as **cross-repo vocabulary only**. Landscape authors do not manually maintain realization lifecycle state in canonical YAML; guiderail derives it later from landscape + financial bridge data. This prevents a future half-manual, half-derived lifecycle mess.

#### Bridge contract (placeholder)
The bridge contract defines how nebulus-financial declares which landscape concepts it implements. The contract shape is TBD until financial begins, but candidate approaches include:
- A manifest file in financial (e.g., `landscape-bridge.yaml`) mapping landscape IDs to code paths
- Annotations or metadata in financial source code referencing canonical IDs
- A shared mapping file in a known location

Regardless of format, the bridge contract must be able to answer these minimum semantics:
- Which landscape IDs are realized in financial?
- Where are they realized (code path, module, service)?
- At what granularity is the realization claimed (full, partial, approximation)?
- Are there gaps or approximations worth noting?

Landscape remains the canonical source of modeled business truth. The bridge contract does not redefine that truth — it only declares realization linkage into financial.

The bridge contract is what allows guiderail to know which entities are "realized" vs. "modeled." Without it, guiderail cannot stitch the two sources together. The Phase 1 deliverable is the principle doc naming this problem, the minimum semantics, and the vocabulary — not the contract implementation itself.

The research refresh convention must define three explicit outcomes for every refresh cycle:
1. **Canonical model updated** — new findings promoted into canonical objects
2. **Research logged but not promoted** — findings captured in research-log, not yet ready for canonical status
3. **Ambiguity recorded** — uncertainty explicitly captured as an open question on the relevant artifact

This prevents refresh from becoming vague note-taking and makes it a real maintenance loop.

### 4. Templates
Create markdown templates for the core object types (from Canonical Model doc):
- `templates/domain-template.md`
- `templates/capability-template.md`
- `templates/value-stream-template.md`
- `templates/journey-template.md`
- `templates/process-template.md`
- `templates/risk-template.md`
- `templates/control-template.md`
- `templates/provider-template.md`
- `templates/system-template.md`

All templates must include **required frontmatter fields**:
- `id`
- `type`
- `name`
- `status`
- `source_refs`
- `last_reviewed`
- `confidence` (present in template shape; allowed but optional in Phase 1 content)

Optional fields depending on type:
- `domain_id`
- `capability_ids`
- `journey_id`
- `process_id`

### 5. Prompts
Create the deep thinking prompts (from Canonical Model doc):
- `prompts/domain-discovery-prompt.md`
- `prompts/capability-decomposition-prompt.md`
- `prompts/journey-authoring-prompt.md`
- `prompts/process-authoring-prompt.md`
- `prompts/risk-control-prompt.md`
- `prompts/provider-mapping-prompt.md`
- `prompts/reconciliation-prompt.md`

### 6. Seed the Customer domain as a vertical slice pattern proof

Customer is seeded first because it exercises the full modeling chain (domain → capability → journey → process → process stage → provider → system) and links naturally to Identity-Access and Accounts. This is a **pattern proof**, not a business-priority claim — the seed demonstrates the repo grammar, not a statement about which domain matters most.

Create one complete vertical slice:
- `docs/domains/customer/domain.md` (authored from template)
- `models/domains/domain.customer.yaml`
- 2-3 capability stubs under `models/capabilities/` (e.g., `cap.customer_onboarding.yaml`, `cap.customer_profile.yaml`)
- 1 journey: `models/journeys/journey.open_savings_account.yaml`
- 1 process: `models/processes/proc.account_opening_v1.yaml`
- 1 process stage: `models/process-stages/stage.account_opening.verify_identity.yaml`
- 1 provider profile + mapping: `models/providers/prov.alloy.yaml` + `models/provider-mappings/map.prov.alloy.yaml`
- 1 system stub: `models/systems/sys.identity_service.yaml`

#### 6a. Cross-domain relationships in the seed
The Customer seed must include explicit references to Identity-Access and Accounts as related domains, even though those remain mostly skeletal in Phase 1. This proves the model is relationship-first, not folder-first. For example:
- `domain.customer.yaml` should reference `domain.identity_access` and `domain.accounts` in `related_domain_ids`
- `journey.open_savings_account.yaml` should reference domain IDs from Customer, Identity-Access, and Accounts
- The provider mapping must reference capabilities and processes from the seed
- The system stub must be linked to the seeded process or capability

#### 6b. Linked thread requirement
The provider stub and system stub are **not standalone placeholders**. They must participate in the seeded thread:
- `map.prov.alloy.yaml` must reference `cap.customer_onboarding`, `journey.open_savings_account`, `proc.account_opening_v1`, and `sys.identity_service`
- `sys.identity_service.yaml` must reference `cap.customer_onboarding` or `proc.account_opening_v1`
- `stage.account_opening.verify_identity.yaml` must reference `proc.account_opening_v1` and `sys.identity_service`, and must include at least one risk, control, or system linkage to prove process-stage semantics rather than just existence

This proves traceability end-to-end, not just object presence.

### 7. Reference taxonomy seeds
- `docs/cross-domain/reference-taxonomy/domains.md` — index of planned domains with IDs
- `docs/cross-domain/reference-taxonomy/capabilities.md` — placeholder index
- `docs/cross-domain/reference-taxonomy/value-streams.md` — index of planned value streams with IDs

Value streams are included because they are central framing objects in the model — the repo should acknowledge them from the start even if content is still sparse.

### 8. Reconciliation pass
After the Customer seed is complete, run a reconciliation pass on the seeded artifacts using the reconciliation prompt. Check for:
- Domain/capability/journey/process confusion
- Naming and ID consistency
- Relationship completeness (do cross-references actually point to real IDs?)
- Template adherence
- Linked thread integrity (can you follow IDs from domain → capability → journey → process → stage → provider → system?)

This is exactly the point where early mistakes are cheapest to fix.

## Key alignment with guiderail

Landscape YAML IDs should map cleanly to guiderail entity IDs:
- `domain.*` → guiderail `Domain.id`
- `cap.*` → guiderail `Capability.id`
- `vs.*` → guiderail `ValueStream.id`
- `journey.*` → guiderail `Journey.id`
- `proc.*` → guiderail `Process.id`
- `stage.*` → guiderail `ProcessStage.id`
- `prov.*` → guiderail `Provider.id`

Provenance in YAML (`source_refs`, `status`) should align with guiderail's `ProvenanceRef` shape (`sourceId`, `confidence`).

**Watchpoint:** Alignment with current guiderail schemas must not reduce `nebulus-landscape` to a mere serialization target. The repo remains the canonical authored model and may need to stay richer than what guiderail currently ingests. Guiderail's schema may evolve to consume more of the landscape — not the other way around.

## What's deferred
- Addendum object types (DataObject, Decision, Policy, Signal, AgentRole, DelegationBoundary, ObservabilityArtifact) — Phase 2+
- Domains beyond Customer/Identity-Access/Accounts skeletons
- Full provider research content
- Mapping files in `mappings/`
- nebulus-financial bridge contract implementation (Phase 1 documents the concept and vocabulary only)
- Guiderail ingestion of landscape YAML (replaces synthetic seed data — guiderail Phase 2)
- nebulus-financial repo creation and initial implementation
- Two-source ingestion and modeled/realized lifecycle rendering (guiderail's responsibility, not landscape's)

## Verification

### Structural
- All folders exist and follow the documented structure
- Templates have correct required frontmatter fields (including `confidence` in shape) and sections matching the idea docs
- Customer domain seed files parse as valid YAML
- All placeholder/skeletal docs are visibly marked as such

### Naming and ID discipline
- All seeded IDs and filenames follow canonical naming conventions exactly:
  - `domain.customer`
  - `cap.customer_onboarding`
  - `journey.open_savings_account`
  - `proc.account_opening_v1`
  - `stage.account_opening.verify_identity`
  - `prov.alloy`
  - `map.prov.alloy`
  - `sys.identity_service`

### Semantic / done-when
- A reader can start at the Customer domain doc, follow IDs/links through one capability, one journey, one process, one process stage, one provider mapping, and one system stub, and understand one coherent example thread through the model.
- Cross-domain references to Identity-Access and Accounts are present and make the relational nature of the model visible.
- The provider and system stubs are linked into the thread, not isolated.
- Reconciliation pass completed with no unresolved confusion.
- `realization-status.md` and `bridge-contract.md` together make it clear how an entity moves from modeled to realized and how guiderail would distinguish the states — without requiring Phase 1 YAML lifecycle fields.

## Proof statement

Phase 1 proves that `nebulus-landscape` can hold one coherent, navigable vertical slice of canonical business architecture content — with stable IDs, structured YAML, readable docs, refresh discipline, cross-domain relationships, and GuideRail-aligned schemas — while also establishing the lifecycle vocabulary and bridge-contract concept that will later allow guiderail to distinguish modeled truth from realized implementation across the full triad. It does this without overreaching into full landscape population or requiring manual lifecycle metadata in Phase 1 YAML.
