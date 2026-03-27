# GuideRail Ingestion Mapping

## Purpose

This document defines the field mapping rules required to transform nebulus-landscape YAML into guiderail TypeScript entity models. The landscape remains the canonical authored model in snake_case; guiderail's ingestion layer performs the translation.

## Systematic transformations

### 1. Case convention
All field names: `snake_case` → `camelCase`

Examples:
- `domain_id` → `domainId`
- `capability_ids` → `capabilityIds`
- `value_stream_id` → `valueStreamId`
- `entry_capability_id` → `entryCapabilityId`

### 2. Label vs Name
Every landscape entity uses `name`. Every guiderail entity uses `label`.

Mapping: `name` → `label`

### 3. Provenance
Landscape uses flat `source_refs: []` and a separate `confidence` field.
Guiderail expects `provenance: { sourceId, confidence, ... }`.

Mapping: construct `ProvenanceRef` from landscape's `confidence` value + first `source_refs` entry where available.

### 4. ProcessStage ordering
Landscape uses `order`. Guiderail uses `sequenceNumber`.

Mapping: `order` → `sequenceNumber`

### 5. Provider description
Landscape uses `summary`. Guiderail uses `description`.

Mapping: `summary` → `description`

## Per-entity field mapping

### Domain
| Landscape | Guiderail | Notes |
|-----------|-----------|-------|
| `id` | `id` | Direct |
| `name` | `label` | Rename |
| `description` | `description` | Direct |
| `capability_ids` | — | Dropped (guiderail derives from Capability.domainId) |
| `related_domain_ids` | `metadata.relatedDomainIds` | Into metadata |
| `source_refs` | `provenance` | Transform |
| `status` | `metadata.status` | Into metadata |

### Capability
| Landscape | Guiderail | Notes |
|-----------|-----------|-------|
| `id` | `id` | Direct |
| `name` | `label` | Rename |
| `domain_id` | `domainId` | Case convert |
| `description` | `description` | Direct |
| `journey_ids` | `journeyIds` | Case convert |
| `value_stream_ids` | — | Dropped (guiderail has no field) |
| `process_ids` | — | Dropped |
| `risk_ids` | — | Dropped |
| `control_ids` | — | Dropped |
| `provider_ids` | — | Dropped |
| `system_ids` | — | Dropped |
| `confidence` | `provenance.confidence` | Into provenance |

### Journey
| Landscape | Guiderail | Notes |
|-----------|-----------|-------|
| `id` | `id` | Direct |
| `name` | `label` | Rename |
| `description` | `description` | Direct |
| `entry_capability_id` | `entryCapabilityId` | Case convert |
| `capability_ids` | `capabilityIds` | Case convert |
| `actor` | `metadata.actor` | Into metadata |
| `domain_ids` | `metadata.domainIds` | Into metadata |
| `process_id` | `metadata.processId` | Into metadata |
| `entry_points` | `entryConditions` | Semantic map |
| `outcomes` | `exitConditions` | Semantic map |

### Process
| Landscape | Guiderail | Notes |
|-----------|-----------|-------|
| `id` | `id` | Direct |
| `name` | `label` | Rename |
| `description` | `description` | Direct |
| `stage_ids` | `stageIds` | Case convert |
| `journey_id` | `metadata.journeyId` | Into metadata |
| `risk_ids` | — | Dropped |
| `control_ids` | — | Dropped |
| `system_ids` | — | Dropped |

### ProcessStage
| Landscape | Guiderail | Notes |
|-----------|-----------|-------|
| `id` | `id` | Direct |
| `name` | `label` | Rename |
| `process_id` | `processId` | Case convert |
| `description` | `description` | Direct |
| `order` | `sequenceNumber` | Rename |
| `risk_ids` | `controlPoints` | Map risk+control IDs into controlPoints |
| `control_ids` | `controlPoints` | Merge with risk_ids |
| `system_ids` | — | Dropped |

### Provider
| Landscape | Guiderail | Notes |
|-----------|-----------|-------|
| `id` | `id` | Direct |
| `name` | `label` | Rename |
| `summary` | `description` | Rename |
| `category` | `category` | Direct |
| `tags` | `tags` | Direct |

## Landscape-only entities (not ingested by guiderail)

These landscape entity types have no guiderail schema counterpart. They remain in the landscape as canonical model enrichments:
- Risk (`risk.*`)
- Control (`ctrl.*`)
- System (`sys.*`)
- ProviderMapping (`map.prov.*`)

Guiderail may later add schemas for these if navigation of risk/control topology becomes a product feature.

## Guiderail-only entities (not produced by landscape)

These guiderail entities are authored or generated within guiderail, not imported from landscape:
- Step (journey walk-through steps)
- Node / Edge (graph topology)
- Sequence (interface/message sequences)
- Scene (UI state snapshots)
- StoryRoute / StoryWaypoint (narrative traversal)
- Perspective / Layer (rendering configuration)
- Workspace / Source (runtime configuration)

## Alignment status

Last audited: 2026-03-27

The landscape YAML structure is compatible with guiderail ingestion via the transformer rules documented above. No landscape schema changes are required beyond the `entry_capability_id` addition (completed).
