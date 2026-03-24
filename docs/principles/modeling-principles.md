# Modeling Principles

These principles govern how the `nebulus-landscape` canonical model is authored and maintained.

## 1. Capability-first, not vendor-first

The landscape is organized around durable business capabilities, not around provider categories or company names. Providers enrich the model. They do not define it.

## 2. Journey is not process

- **Journey** = the real user or business path
- **Process** = the operational execution of that path

They are linked but not interchangeable. A journey describes what the actor experiences. A process describes how the operation is carried out.

## 3. Value stream frames but does not dominate

Value streams are important as higher-order business flow framing, but they should not swallow capabilities or journeys. A value stream connects multiple capabilities and journeys under a business objective — it does not replace them.

## 4. Risk/control thinking is integral, not bolted on

Risks and controls should not be afterthoughts. They attach directly to processes, journeys, capabilities, and provider/system participation where relevant. They should be authored as part of the modeling pass, not as a separate compliance exercise.

## 5. Provenance matters

Every meaningful artifact should be traceable to:
- a source
- an author (or authoring method)
- a confidence level
- open questions where uncertainty exists

This keeps the model honest and prevents accidental over-certainty.

## 6. Structured model plus readable docs

The repo supports:
- human-readable narrative docs (`docs/`)
- structured YAML model artifacts (`models/`)
- explicit relationship mappings (`mappings/`)

It cannot be only prose and it cannot be only machine objects. Both layers must stay in sync.

## 7. Future ingestion should not warp the current model

Future ingestion from codebases, Figma, BPMN, API registries, or other sources should enrich the model without replacing the human-authored conceptual truth. The repo remains concept-first, not import-first.

## 8. Business truth first

The ordering principle for the landscape:

> Business truth first, process/control truth second, technical realization third, provider mapping fourth.

That order matters for authoring priority, not for artifact importance — all layers are valuable, but they should be built in this sequence.
