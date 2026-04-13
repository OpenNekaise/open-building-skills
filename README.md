# OpenBuildingSkills

A growing collection of open-source LLM agent skills that encode domain knowledge of smart building standards — ontologies, automation protocols, and control languages.

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

## License

[MIT](LICENSE)
