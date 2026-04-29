# LLM Wiki Schema

This document defines the schema, structure, and operations for this LLM-maintained knowledge base.

## Core Principles

- **Raw Sources (`raw/`)**: Immutable files (papers, articles, transcripts). The LLM reads these but NEVER modifies them.
- **The Wiki**: A structured collection of markdown files maintained entirely by the LLM.

## Documentation

For detailed rules and guidelines, refer to the following documents:
- [File Structure & Conventions](docs/file-structure.md): Rules for creating, linking, and organizing files.

## 🛑 Critical Constraints

- **No WikiLinks**: ALWAYS use standard Markdown relative links (e.g., `[Title](../concepts/File.md)`). DO NOT use `[[WikiLinks]]`.
- **Absolute Paths**: When using tools, always use absolute paths provided in the environment.
- **Immutable Raw**: NEVER modify files in the `raw/` directory.
- **Link Verification**: ALWAYS verify the existence of concept files in `wiki/concepts/` before linking. If a concept is important but missing, create it immediately rather than using a placeholder or incorrect link.

## Operations (Agent Skills)

The wiki is maintained using specific agent skills. Please use the relevant skill when performing operations:

- **[Ingest](.agents/skills/ingest/SKILL.md)**: Process new files in `raw/`, write summaries, and extract concepts.
- **[Query](.agents/skills/query/SKILL.md)**: Query the local wiki index to synthesize knowledge and answer complex questions.
- **[Lint](.agents/skills/lint/SKILL.md)**: Health-check the wiki for consistency, orphan pages, and contradictions.
- **Atomic Operations**: Every turn that results in a new file in `wiki/concepts/` or `wiki/summaries/` MUST conclude with an update to `index.md` and `log.md` before the turn ends.

## Log Rules

Every `ingest` entry in `log.md` MUST include a Markdown link to the corresponding summary page.
See [File Structure & Conventions](docs/file-structure.md#log-conventions-logmd) for the required format.

## Schema Changes

When a structural rule is introduced or updated (e.g., a new required field in `log.md`), apply it **retroactively** to all existing entries unless explicitly told otherwise.
