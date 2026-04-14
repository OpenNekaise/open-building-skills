---
name: know-brick
description: Brick Schema modeling, querying, validation, and BuildingMOTIF workflow support for semantic building metadata. Use this skill whenever the user mentions Brick, BAS/BMS point labels, building equipment or room inventories, Turtle/RDF/SPARQL/SHACL in a buildings context, BuildingMOTIF templates, or BACnet-to-Brick and Haystack-to-Brick pipelines — even when they don't say "Brick" by name but are clearly trying to model building metadata, map equipment or point labels to a semantic schema, or query a building graph. Also trigger for drafting or repairing Brick TTL, validating Brick graphs, generating example models, or deciding between raw TTL and BuildingMOTIF.
---

# Know Brick

Turn messy building metadata into clear Brick-aligned models, queries, and workflow guidance.

## Working style

- Start by identifying the user artifact:
  - conceptual question
  - label or equipment inventory
  - RDF, Turtle, SHACL, or SPARQL file
  - application or integration workflow
- Prefer explicit Brick classes and relationships over informal tag-only descriptions. Tags are annotations and discovery aids, not the primary type system; classes are what makes a graph portable across tools and vendors.
- Keep the model semantically sufficient for the target application. Over-modeling wastes authoring effort and makes validation noisier; model only the slice the application actually queries.
- When multiple Brick classes are plausible, rank the best 2–4 candidates and explain the discriminator.
- When a source label or abbreviation is ambiguous, preserve the ambiguity. Ask a narrow follow-up only if it blocks correctness; otherwise make the smallest defensible assumption and mark it clearly in the output.

## Reading the references

`references/brick-reference.md` is organized by topic — jump to the right section instead of reading the whole file:

- **§2 Core concepts, §3 Core class hierarchy** — when picking a class for an entity
- **§4 Relationship decision table** — when choosing between `hasPoint` / `hasPart` / `feeds` / `hasLocation`
- **§5 Practical modeling workflow, §6 Minimal modeling patterns** — when building a new model
- **§7 SPARQL snippets** — when writing queries
- **§8 BMS point mapping guide** — when translating BAS labels
- **§9 Common pitfalls, §10 Modeling checklist** — before finalizing an answer

`references/sources.md` has authoritative links and citations when the user asks where a rule comes from.

`assets/Brick.ttl` is a local copy of the full Brick ontology. Reach for it as the canonical source when you need to confirm a class exists, inspect its parents, or validate a model offline — prefer it over guessing class names from memory.

## Workflow decision tree

### 1. Concept or mapping request

- Map the building concept to Brick classes and relationships.
- Return: recommended class(es), required relationships, a minimal TTL example, and unresolved ambiguities.

### 2. Existing RDF, Turtle, SPARQL, or SHACL

- Parse the file first.
- Run `scripts/brick_tool.py validate ...` for syntax and Brick-aware checks.
- For data extraction or debugging, run `scripts/brick_tool.py query ...` with a builtin or custom SPARQL query.
- For structural exploration of an unfamiliar graph, use `scripts/ontology_tool.py topology`, `describe`, or `list-instances` to orient yourself before proposing changes.
- Propose concrete repairs with before/after TTL, not just high-level commentary.

### 3. Label inventory or BAS export

- Run `scripts/label_inventory.py` to normalize delimiters, inventory tokens, and surface repeated abbreviations.
- Convert recurring patterns into Brick mapping candidates.
- Prefer stable equipment and point patterns over one-off labels — a mapping that covers 80% of a site cleanly is more valuable than one that twists itself around outliers.

### 4. New model from scratch

- Start with spaces, major equipment, key points, and only the relationships needed by the target application.
- Use `scripts/brick_tool.py scaffold ...` when the user provides structured inventory data.
- Validate before presenting the model.

### 5. BuildingMOTIF workflow

- Prefer BuildingMOTIF when the task involves templates, shape-driven authoring, repeated patterns across buildings, CSV ingress, or BACnet-to-Brick pipelines.
- Explain whether the user should use raw TTL, BuildingMOTIF templates, or a mixed approach, and say why.

## Default output pattern

Use this structure unless the user asks for something else.

### Recommendation
State the best Brick classes, relationships, or workflow choice.

### Why this fits
Explain the semantic reasoning and any reasonable alternatives.

### Minimal example
Provide a short TTL, SPARQL, or BuildingMOTIF example.

### Validation notes
Call out syntax, shape, and modeling checks.

### Open questions
List only ambiguities that materially affect correctness.

## Modeling rules that matter

- Prefer classes to tags for typing entities. Tags are annotations and discovery aids, not the primary type system.
- Use `brick:hasPoint` / `brick:isPointOf` for telemetry and command association with equipment or zones.
- Use `brick:hasLocation` / `brick:isLocationOf` for physical placement.
- Use `brick:hasPart` / `brick:isPartOf` for composition — a fan is part of an AHU; an AHU is not part of a room.
- Use `brick:feeds` / `brick:isFedBy` for distribution flow (air, water, electricity).
- Do not use `hasPart` when the real meaning is location; this is one of the most common and most damaging modeling errors because it silently breaks containment queries.
- Avoid inventing custom classes when an existing Brick class plus clear relationships is enough.
- When the exact point class is unknown, use a safe superclass such as `brick:Point`, `brick:Sensor`, `brick:Setpoint`, or `brick:Command` and say what information would let you specialize it.

## Script quick start

### Validate and summarize a graph

```bash
python scripts/brick_tool.py validate --input model.ttl
python scripts/brick_tool.py summarize --input model.ttl
```

### Run a builtin or custom query

```bash
python scripts/brick_tool.py query --input model.ttl --builtin equipment-points
python scripts/brick_tool.py query --input model.ttl --query-file my_query.rq
python scripts/brick_tool.py query --input model.ttl --query "SELECT ..."
```

### Scaffold a starter model from JSON

```bash
python scripts/brick_tool.py scaffold --input site.json --output starter.ttl
```

### Inventory label patterns

```bash
python scripts/label_inventory.py labels.csv
python scripts/label_inventory.py labels.txt --column point_name
```

### Explore an unfamiliar graph

```bash
python scripts/ontology_tool.py topology model.ttl
python scripts/ontology_tool.py describe model.ttl <entity-uri>
python scripts/ontology_tool.py list-classes model.ttl
python scripts/ontology_tool.py list-instances model.ttl <class-uri>
```

### Refresh the bundled ontology

```bash
python scripts/ontology_tool.py fetch-brick
```

## Example user requests

- "Map these BAS point labels to Brick classes."
- "Repair this Brick Turtle model and tell me what is semantically wrong."
- "Give me a minimal Brick model for one AHU, two VAVs, and three rooms."
- "Should this repeated pattern be raw TTL or a BuildingMOTIF template?"
- "Write SPARQL to find all temperature sensors and the equipment or spaces they are about."
