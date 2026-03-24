# nebulus-landscape

Canonical business architecture knowledge model for the fintech and banking landscape.

## Triad Context

This repo is one leg of a three-project triad:

| Repo | Role |
|------|------|
| `nebulus-landscape` | Canonical domain model — what should exist and why |
| `nebulus-financial` | Reference application — one realization of that truth in running code |
| `guiderail` | Guided navigation across both layers, linked by stable IDs |

The goal is to maintain this triad to evaluate new patterns, practices, providers, and emerging concepts. Landscape establishes the design authority. Financial implements selected parts. Guiderail makes both navigable.

## Repo Authoring Rules

These rules govern all artifact creation and modification in this repo.

### Modeling discipline
- **Capability-first, not vendor-first.** The landscape is organized around durable business capabilities. Providers enrich the model; they do not define it.
- **Journey is not process.** Journey = the real user or business path. Process = the operational execution of that path. They are linked but not interchangeable.
- **Value stream frames but does not replace capability/journey.** Value streams are higher-order business flow framing. They should not swallow capabilities or journeys.
- **Risk/control thinking is integral, not bolted on.** Risks and controls attach directly to processes, journeys, capabilities, and provider/system participation.
- **Provenance must be captured when interpretation is involved.** Every meaningful artifact should be traceable to a source, an author, a confidence level, and open questions where uncertainty exists.

### ID conventions
IDs are stable and canonical once introduced. They are the spine that connects landscape → financial → guiderail.

| Object Type | ID Pattern | Example |
|-------------|-----------|---------|
| Domain | `domain.<name>` | `domain.customer` |
| Capability | `cap.<name>` | `cap.customer_onboarding` |
| Value Stream | `vs.<name>` | `vs.acquire_and_activate_customer` |
| Journey | `journey.<name>` | `journey.open_savings_account` |
| Process | `proc.<name>` | `proc.account_opening_v1` |
| Process Stage | `stage.<process>.<name>` | `stage.account_opening.verify_identity` |
| Risk | `risk.<name>` | `risk.synthetic_identity` |
| Control | `ctrl.<name>` | `ctrl.idv_before_funding` |
| Provider | `prov.<name>` | `prov.alloy` |
| Provider Mapping | `map.prov.<name>` | `map.prov.alloy` |
| System | `sys.<name>` | `sys.identity_service` |
| Interface | `if.<system>.<name>` | `if.identity_service.verify_identity` |
| Message | `msg.<name>` | `msg.identity_verification_request` |
| Evidence | `ev.<name>` | `ev.idv_policy_v1` |
| Source | `src.<name>` | `src.provider.alloy.docs` |

Rules:
- Use `snake_case` for all ID segments
- IDs must not be renamed once referenced by other artifacts or external repos
- Version suffixes (`_v1`, `_v2`) are used for process and evidence artifacts where versioning matters

### File naming
- YAML model files: `<id_pattern>.yaml` (e.g., `domain.customer.yaml`, `cap.customer_onboarding.yaml`)
- Markdown docs: `kebab-case.md` (e.g., `customer-onboarding.md`)
- Directories: `kebab-case`

### Frontmatter requirements
All markdown artifacts must include these required frontmatter fields:
```yaml
---
id: <canonical ID>
type: <object type>
name: <display name>
status: draft | active | deprecated | placeholder
source_refs: []
last_reviewed: <YYYY-MM-DD>
confidence: <high | medium | low | candidate | disputed>  # optional in early phases
---
```

### Placeholder discipline
Any placeholder or deferred document must be visibly marked as skeletal (e.g., `status: placeholder`) so the repo does not imply false completeness.

## GuideRail Alignment

Landscape YAML IDs map to guiderail entity IDs:
- `domain.*` → `Domain.id`
- `cap.*` → `Capability.id`
- `vs.*` → `ValueStream.id`
- `journey.*` → `Journey.id`
- `proc.*` → `Process.id`
- `stage.*` → `ProcessStage.id`
- `prov.*` → `Provider.id`

Provenance in YAML (`source_refs`, `status`, `confidence`) aligns with guiderail's `ProvenanceRef` shape (`sourceId`, `confidence`).

**Watchpoint:** Alignment with current guiderail schemas must not reduce this repo to a mere serialization target. The landscape remains the canonical authored model and may be richer than what guiderail currently ingests. Guiderail's schema may evolve to consume more — not the other way around.

## Authoring Workflow

For each domain, work in this order:
1. Write the domain artifact
2. Derive the capabilities
3. Pick 2–3 flagship journeys
4. Define 1–2 flagship processes
5. Attach top risks and controls
6. Map 1–3 important providers
7. Reconcile overlaps and weak naming

## Research Refresh

Domain research should be refreshed incrementally. Every refresh produces one of three outcomes:
1. **Canonical model updated** — findings promoted into canonical objects
2. **Research logged but not promoted** — findings captured in `docs/research-log/`, not yet canonical
3. **Ambiguity recorded** — uncertainty captured as an open question on the relevant artifact

See `docs/principles/research-refresh-convention.md` for the full workflow.
