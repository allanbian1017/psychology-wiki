# Psychology Wiki — Agent Schema

This document defines the schema, structure, and operations for this LLM-maintained **Psychology knowledge base**.

## 🧠 Domain & Purpose

This repository is a **curated wiki focused exclusively on Psychology**. Its goal is to collect, organize, and connect psychological research, theories, and concepts — spanning clinical psychology, cognitive science, social psychology, neuroscience, and related fields.

**Agent role**: Agents are responsible for maintaining this wiki with the same rigour as a professional encyclopedia. Every summary, concept page, and index entry should be accurate, well-linked, and grounded in the source material stored in `raw/`. Agents should:
- Favour depth and precision over brevity when synthesizing psychological content.
- Actively surface cross-disciplinary connections (e.g., between cognitive biases and clinical interventions).
- Use domain-appropriate language consistent with academic psychology.

## Core Principles

- **Domain Focus**: All content must be relevant to Psychology or closely related fields. Off-topic material should not be ingested.
- **Raw Sources (`raw/`)**: Immutable files (papers, articles, transcripts). The LLM reads these but NEVER modifies them.
- **The Wiki**: A structured collection of markdown files maintained entirely by the LLM — acting as an evolving, interconnected reference for psychological knowledge.
- **Language Policy**: All wiki-generated content — including summaries, concept pages, index entries, derived articles, and log descriptions — MUST be written in **Traditional Chinese (繁體中文)**. Raw source files in `raw/` are exempt (they are immutable and may be in any language), but every agent-generated output MUST be in Traditional Chinese.

## Documentation

For detailed rules and guidelines, refer to the following documents:
- [File Structure & Conventions](docs/file-structure.md): Rules for creating, linking, and organizing files.

## 🛑 Critical Constraints

- **Language — Traditional Chinese (繁體中文)**: ALL agent-generated wiki content MUST be written in Traditional Chinese. This includes summaries, concept pages, index entries, derived articles, and log descriptions. There are NO exceptions for agent-authored content.
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
