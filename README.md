# nebulus-landscape

Canonical business architecture knowledge model for the fintech and banking landscape.

## Purpose

`nebulus-landscape` is a capability-first, journey-aware, process-aware, risk/control-aware, and provider-enriched knowledge model. It serves as the design authority for the Nebulus project ecosystem.

The repo answers these questions:
- What are the important business domains in banking / fintech?
- What enduring capabilities exist within those domains?
- What real user or business journeys occur across those capabilities?
- How are those journeys operationally executed?
- What risks and controls are present?
- What systems, interfaces, and messages participate?
- Where do providers fit?

## Triad

This repo is one leg of a three-project triad:

| Repo | Role |
|------|------|
| `nebulus-landscape` | Canonical domain model — what should exist and why |
| `nebulus-financial` | Reference application — one realization of that truth in running code |
| `guiderail` | Guided navigation across both layers, linked by stable IDs |

## Structure

```
docs/
  principles/       Modeling rules, naming conventions, confidence levels
  research-log/     Dated refresh notes per domain review cycle
  domains/          Domain-organized authored artifacts
  cross-domain/     Value streams, cross-cutting journeys, provider comparisons, taxonomy indexes
  sources/          Source reference catalogs

models/             Canonical structured YAML objects (domains, capabilities, journeys, processes, etc.)

mappings/           Explicit relationship files between canonical objects

templates/          Markdown templates for each artifact type

prompts/            Deep thinking prompts for domain-by-domain authoring
```

### Three layers

| Layer | Purpose |
|-------|---------|
| `docs/` | Human-readable authored artifacts — what you read, debate, and refine |
| `models/` | Canonical structured objects — what tooling and visualization depend on |
| `mappings/` | Explicit relationships — what prevents the repo from becoming isolated object files |

## Ordering principle

> Business truth first, process/control truth second, technical realization third, provider mapping fourth.

## Related repos

- [guiderail](https://github.com/backspring-labs/guiderail) — Guided architecture navigation
- nebulus-financial — Reference banking application (planned)
