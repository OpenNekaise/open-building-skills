# OpenBuildingSkills

**Built for AI agents working with smart buildings.**

OpenBuildingSkills is a growing collection of open-source, plug-and-play skills that give LLM agents working domain knowledge of smart-building standards — ontologies, automation protocols, and control languages — so your agent shows up with the context it needs instead of learning it from scratch in every conversation.

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

A skill for working with the Brick Schema — modeling, querying, validating, and mapping BAS/BMS labels to Brick classes, with guidance on BuildingMOTIF workflows. Built on open-source material from the Brick Schema community.

[Brick Schema](https://brickschema.org/) · [BrickSchema on GitHub](https://github.com/BrickSchema)

### know-223p

A skill for working with ASHRAE Standard 223P and the Open223 ecosystem — modeling equipment topology, attaching properties and telemetry, validating with SHACL, and layering interop with QUDT, Brick, and RealEstateCore. Built on open-source material from the Open223 project.

[Open223](https://open223.info/) · [Open223 on GitHub](https://github.com/open223)

## License

[MIT](LICENSE)
