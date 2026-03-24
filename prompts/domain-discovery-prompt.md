# Domain Discovery Prompt

```text
You are helping create a canonical business architecture knowledge repo called nebulus-landscape.

Your task is to produce a domain artifact for the domain: [DOMAIN NAME].

Goal:
Produce a high-quality domain document and a matching structured domain object that fits a banking/fintech landscape model.

Modeling rules:
- Treat the domain as a durable business area, not a vendor category.
- Separate domain from capability.
- Do not collapse journeys or processes into the domain definition.
- Use banking/fintech language that is concrete and practically useful.
- Surface ambiguity explicitly.
- Do not pretend completeness if tradeoffs exist.

Output 1: Markdown domain artifact with these sections:
1. Definition
2. Business purpose
3. Scope
4. Included capabilities
5. Excluded capabilities
6. Upstream dependencies
7. Downstream dependencies
8. Related journeys
9. Typical risks and controls
10. Typical systems/providers involved
11. Open questions
12. Provenance notes

Output 2: YAML object with:
- id
- name
- description
- capability_ids
- related_domain_ids
- source_refs
- status

Additional requirements:
- Propose durable capability names under the domain
- Avoid vendor-first framing
- Favor enduring concepts over current implementation details
- Where uncertainty exists, add an "Open questions" subsection
```
