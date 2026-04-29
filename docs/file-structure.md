# File Structure & Conventions

This document outlines the required file locations and linking conventions for the LLM-maintained knowledge base.

## Directories

- `raw/`: **Immutable** files (papers, articles, transcripts). These must NEVER be modified.
- `wiki/summaries/`: Dedicated summary pages generated from the raw sources.
- `wiki/concepts/`: Individual Markdown files for extracted concepts and entities.
- `wiki/derived/`: Derived outputs requested by the user, such as Marp slide decks or matplotlib charts.

> [!IMPORTANT]
> All generated files MUST be placed in their designated subdirectories and never directly in the project root.

## Linking Conventions

- **Relative Paths Only**: Link between pages using standard Markdown relative paths (e.g., `[Title](../concepts/Concept_Name.md)`).
- **CRITICAL**: Do NOT use Obsidian-style `[[WikiLinks]]`. These will break the expected markdown routing.

## Maintenance Policy

- Avoid deleting history unless instructed; prefer to update and document evolving understanding.
- Ensure all wiki pages have a clear, descriptive title.

## Log Conventions (`log.md`)

- Every `ingest` entry MUST include a relative Markdown link to its corresponding summary page.
- **Required format:** `## [YYYY-MM-DD] ingest | [Source Title](wiki/summaries/<filename>.md)`
- `lint` and `setup` entries do not require a link.
