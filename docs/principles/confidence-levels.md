# Confidence Levels

Confidence captures how certain we are about an artifact's content or a relationship between artifacts. It is especially important for:
- provider mappings (providers often blur across capabilities)
- inferred process/risk/control relationships
- system/interface/message placement
- future ingestion outputs

## Values

| Level | Meaning | When to use |
|-------|---------|-------------|
| `high` | Well-sourced, reviewed, and stable | Directly observed, documented, or confirmed through multiple sources |
| `medium` | Reasonable interpretation with some evidence | Inferred from documentation, demos, or partial observation |
| `low` | Plausible but weakly supported | Based on marketing material, indirect inference, or limited evidence |
| `candidate` | Proposed but not yet validated | Suggested by pattern or analogy, needs research |
| `disputed` | Conflicting evidence or active disagreement | Multiple sources disagree, or the mapping is under active debate |

## Usage

- `confidence` is a field on YAML model artifacts and markdown frontmatter
- In early phases, confidence is **allowed but optional** — include it when you have a basis for it
- The field must be present in all templates so the shape is established from the start
- When confidence is omitted, consumers should treat the artifact as `candidate` by default

## Why this matters

This keeps the model honest. Without explicit confidence:
- marketing materials can be mistaken for architectural truth
- partial observations can calcify into assumed facts
- provider placements can appear more settled than they are

Confidence is not a quality judgment on the author — it is an honest signal about the strength of the evidence behind the claim.
