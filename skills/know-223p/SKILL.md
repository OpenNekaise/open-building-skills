---
name: know-223p
description: ASHRAE Standard 223P and the Open223 ecosystem — modeling, querying, validation, and interop with QUDT, Brick, and RealEstateCore for semantic building data. Use this skill whenever the user mentions 223P, s223, Open223, Connectables, ConnectionPoints, Domain/PhysicalSpace, building equipment connectivity in a semantic context, or Turtle/RDF/SHACL/SPARQL for building automation — even when they don't say "223P" by name but are modeling equipment topology, space hierarchies, attaching BACnet telemetry to a semantic model, or deciding how to layer Brick or RealEstateCore on 223P. Also trigger for drafting or reviewing 223P Turtle, debugging inference or SHACL validation, and modeling specific systems (VAV, AHU, chilled-water loop) in 223P.
---

# Know 223P

Authoritative guidance for ASHRAE 223P modeling, review, querying, validation, and interop. The deep content lives in `references/s223-reference.md`; this file keeps the workflow and routing compact so you can jump to the right section fast.

## Start here

1. Identify the user's mode:
   - explain the ontology or teach concepts
   - write or review Turtle/RDF/SHACL/SPARQL
   - model a building system or space hierarchy
   - attach telemetry or BACnet references
   - layer Brick or RealEstateCore on top of 223P
   - debug inference or validation
2. Jump to the matching section(s) of `references/s223-reference.md` (see routing below).
3. Answer in exact 223 vocabulary first, then add a minimal example when useful.

## Reading the reference

`references/s223-reference.md` has a full table of contents; route by task:

- **§1–§2 What 223P is, five lenses** — when explaining the ontology from scratch
- **§3 Semantic-web stack** — when the user is unsure about the RDF/SHACL/SPARQL roles
- **§4 Core structure, §5 Topology** — when modeling equipment and connectivity
- **§6 Properties, sensors, telemetry, functions** — when attaching measurements, commands, or derived values
- **§7 Media, constituents, mixtures** — when assigning media to connection points or checking compatibility
- **§8 Inference and validation** — when debugging SHACL errors or asserted-vs-inferred confusion
- **§9 QUDT / Brick / RealEstateCore interop** — when layering ontologies or adding units
- **§10 Modeling workflow, §11 Minimal Turtle patterns** — when authoring a new model
- **§12 Common mistakes** — before finalizing any non-trivial answer
- **§13–§14 Canonical resources, source-priority rules** — when citing or resolving conflicts between public sources

`assets/223p.ttl` is a local copy of the 223P ontology. Reach for it as the canonical source when you need to confirm a class or relation exists, inspect its parents, or validate offline — prefer it over guessing names from memory.

## Non-negotiable stance

- Treat 223P as an evolving proposed standard unless the user supplies a newer normative source. Public docs still describe it as proposed, so don't claim final-standard status.
- Treat `docs.open223.info`, `defs.open223.info`, `models.open223.info`, `query.open223.info`, and `open223.info` as the best public implementation aids, not the final normative ASHRAE text.
- Prefer exact ontology names and relations over informal synonyms — portability depends on it.
- Distinguish asserted triples from inferred triples; conflating them is one of the most common sources of "why doesn't my query return what I expect" bugs.

## Rules to preserve in every model

- Model connectivity with `Connectable`, `ConnectionPoint`, and `Connection`. Do not collapse real topology into a single vague edge — that throws away the information 223P exists to preserve.
- Prefer asserting `s223:cnx` and let inference derive higher-level connection relations when appropriate.
- Put media on `ConnectionPoint`s and `Connection`s, and keep them compatible. Incompatible media at a junction is the single most common validation failure.
- Use `s223:hasProperty` to associate properties to concepts or connection points.
- Treat 223 `Property` instances as the source and context of a measurement, command, or characteristic — not as a detached physical phenomenon.
- Model a multi-sensor device as `Equipment` containing multiple `Sensor` instances, not as one sensor with multiple outputs.
- Model derived or computed values with `s223:Function` or `FunctionBlock` patterns, not as directly observed properties.
- Use `qudt:hasQuantityKind` and `qudt:hasUnit` for quantifiable properties so units are machine-checkable.
- Use Brick and RealEstateCore as additive annotations, not replacements for 223 topology. They answer different questions.
- Do not place time-series telemetry inside the 223 graph; link out using external references. The graph describes *what is measured and how*, not *the measurements themselves*.

## Default output shape

When helping with 223P, usually provide:

1. The modeling decision or explanation.
2. The exact 223 concepts and relations involved.
3. A small Turtle or SPARQL example when it sharpens the answer.
4. Validation or inference consequences — what SHACL will flag, what reasoners will add.
5. Interop notes if QUDT, Brick, REC, or BACnet are relevant.

## Writing or reviewing Turtle

- Declare prefixes explicitly.
- Keep instance names readable and role-based (`vav_201_zn_temp`, not `sensor_17`).
- Prefer minimal valid examples over huge graphs — readers can extend; they can't easily compress.
- Call out what is asserted now versus what should appear after inference.
- Flag likely mistakes: missing media on connection points, missing connection points on connectables, wrong sensor-property pairing, misuse of `hasValue` for telemetry.

## Quick explanation mental model

If the user wants a high-level pitch, lead with this:

- 223P describes what building things exist, how they connect, what spaces they affect, what properties they expose, and how those properties are measured, commanded, or derived.
- Its power comes from RDF graphs plus SHACL validation and inference, QUDT units, and optional interop with Brick and RealEstateCore.

## Script quick start

`scripts/ontology_tool.py` handles parsing, querying, validation, and exploration for 223P graphs (it also works for Brick — the 223P-specific workflows are below).

### Parse and validate a 223P model

```bash
python scripts/ontology_tool.py parse model.ttl
python scripts/ontology_tool.py validate model.ttl
```

### Explore an unfamiliar 223P graph

```bash
python scripts/ontology_tool.py topology model.ttl
python scripts/ontology_tool.py describe model.ttl <entity-uri>
python scripts/ontology_tool.py list-classes model.ttl
python scripts/ontology_tool.py list-instances model.ttl <class-uri>
```

### Run a SPARQL query

```bash
python scripts/ontology_tool.py query model.ttl "SELECT ?s WHERE { ?s a s223:Equipment } LIMIT 20"
```

### Export to another RDF serialization

```bash
python scripts/ontology_tool.py export model.ttl json-ld out.jsonld
```

### Refresh the bundled ontology

```bash
python scripts/ontology_tool.py fetch-223p
```

## Example requests

- "Explain the difference between `PhysicalSpace`, `DomainSpace`, `Zone`, and `System`."
- "Model a VAV with a damper, discharge-temperature sensor, and zone in 223P Turtle."
- "Debug a medium or connection-point validation error in this graph."
- "Attach BACnet telemetry to a 223 property."
- "Layer Brick points and equipment onto a 223 model."
