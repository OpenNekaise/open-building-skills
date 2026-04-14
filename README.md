# OpenBuildingSkills

**Built for AI agents working on smart-building problems.**

OpenBuildingSkills is a growing collection of open-source, plug-and-play skills that give LLM agents working domain knowledge of smart-building standards — ontologies, automation protocols, and control languages — so your agent shows up with the context it needs instead of learning it from scratch in every conversation.

Drop a skill into a Claude Code, Claude.ai, or Anthropic API project and the agent can immediately map BAS labels to Brick classes, author valid 223P Turtle, or reason about BACnet topology — with bundled scripts, offline ontologies, and task-routed references to back it up.

## What Are Skills?

Skills are modular, filesystem-based capability packages that extend what LLM agents can do. Each skill is a folder containing a `SKILL.md` file (Markdown with YAML frontmatter) plus optional scripts, templates, and reference documents. Skills teach agents *how* to perform domain-specific tasks so users don't have to re-explain the same guidance in every conversation.

Skills use progressive disclosure across three levels:

- **Level 1 (metadata)** — always loaded; just the skill's name and description so the agent knows it exists.
- **Level 2 (instructions)** — loaded on-demand when a user request matches the skill's trigger.
- **Level 3 (resources)** — bundled scripts, references, and templates loaded only as needed during execution.

This lets agents carry many skills without bloating context. Think of skills as the *knowledge* layer — complementing tools (MCP) that provide *connectivity*.

Learn more:

- [The Complete Guide to Building Skills for Claude (PDF)](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf)
- [Agent Skills Overview — Claude Platform Docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)

## Skills

### know-brick

Turns messy building metadata — BAS/BMS point labels, equipment inventories, ad-hoc Turtle — into Brick-aligned models, SPARQL queries, and workflow guidance. Reach for it when you need to map BACnet or Haystack labels to Brick classes, draft or repair Brick TTL, validate a graph, scaffold a starter model from an inventory, or decide between raw TTL and BuildingMOTIF templates.

**Bundled resources:**

- Topic-routed reference covering class selection, the `hasPoint`/`hasPart`/`feeds`/`hasLocation` decision, SPARQL patterns, BAS point mapping, and common pitfalls
- `brick_tool.py` — validate, summarize, run builtin or custom SPARQL, scaffold a starter model from JSON
- `label_inventory.py` — normalize delimiters and surface repeated abbreviations in BAS label exports
- `ontology_tool.py` — topology, describe, list-classes/instances for exploring unfamiliar graphs
- `Brick.ttl` — full Brick ontology bundled for offline class lookup and validation

**Example prompts:**

- "Map these BAS point labels to Brick classes."
- "Repair this Brick Turtle model and tell me what's semantically wrong."
- "Give me a minimal Brick model for one AHU, two VAVs, and three rooms."
- "Write SPARQL to find all temperature sensors and the equipment they monitor."

**External resources:** [Brick Schema](https://brickschema.org/) · [BrickSchema on GitHub](https://github.com/BrickSchema)

### know-223p

Authoritative guidance for ASHRAE Standard 223P and the Open223 ecosystem — modeling equipment topology with `Connectable`s and `ConnectionPoint`s, attaching properties and telemetry, validating with SHACL, and layering interop with QUDT, Brick, and RealEstateCore. Reach for it when you're authoring or reviewing 223P Turtle, debugging inference or SHACL errors, modeling specific systems (VAV, AHU, chilled-water loop), or deciding how Brick/REC should sit alongside 223P.

**Bundled resources:**

- Deep reference routed by task — topology and connectivity, properties and telemetry, media and mixtures, inference and validation, QUDT/Brick/REC interop, common mistakes
- `ontology_tool.py` — parse, validate, topology, describe, SPARQL query, export, and refresh the 223P ontology
- `223p.ttl` — full 223P ontology bundled for offline class and relation lookup

**Example prompts:**

- "Explain the difference between `PhysicalSpace`, `DomainSpace`, `Zone`, and `System`."
- "Model a VAV with a damper, discharge-temperature sensor, and zone in 223P Turtle."
- "Debug a medium or connection-point validation error in this graph."
- "Attach BACnet telemetry to a 223 property."

**External resources:** [Open223](https://open223.info/) · [Open223 on GitHub](https://github.com/open223)

## License

[MIT](LICENSE)
