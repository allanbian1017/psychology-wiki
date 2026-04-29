---
name: lint
description: Health-checks the wiki for consistency, imputes missing data via web search, and suggests new concept pages.
version: 1.0.0
pattern: Pipeline
---

# 🎯 Purpose
To maintain the semantic structure, factual consistency, and comprehensive health of the knowledge base through automated auditing, data imputation, and structural optimization.

> **語言政策 (Language Policy):** All agent-generated content produced during linting — imputed data, new concept pages, proposals, and log entries — MUST be written in **Traditional Chinese (繁體中文)**.

# 🚪 Gating & Trigger Conditions
- **When to invoke:** Upon user request for a "wiki health check", during scheduled maintenance routines, or after a bulk ingestion of new documents.
- **When NOT to invoke:** During active, real-time document drafting by the user (to avoid write collisions), or when the user requires a fast, targeted search query rather than a full audit.

# 📥 Input Specifications
- Master index file containing the current wiki structure.
- Target directory containing all `.md` concept and log files.
- Access to a web search tool/API for data imputation.



# ⚙️ Execution Instructions (Workflow)
1. **Parse Index (Reviewer):** Read and parse `index.md` to map the current known schema of the knowledge base.
2. **Audit Links (Reviewer):** Scan all `.md` files in the wiki directory `wiki/` to identify orphans (pages with no incoming links) and broken links (pointers to missing `.md` files).
3. **Consistency Check (Reviewer):** Cross-reference recent summaries against existing concept pages to detect stale claims, contradictions, or missing details.
4. **Impute Data (Tool Wrapper / Generator):** For identified knowledge gaps or explicit "TODO" markers, execute a web search. Synthesize the search results to generate the missing content. **All imputed content MUST be written in Traditional Chinese (繁體中文).**
5. **Generate Proposals (Generator):** Analyze isolated or loosely connected documents. Generate a list of proposed new Concept Pages that combine overlapping themes. **All proposed and created concept pages MUST be written in Traditional Chinese (繁體中文).**
6. **Execute or Escalate (Inversion):** - *If issues are trivial* (e.g., updating broken links or fixing typos): Apply fixes directly to the files.
    - *If issues are structural* (e.g., creating new concept hubs, altering major facts): Halt execution, present a formatted markdown plan to the user, and request explicit approval before proceeding.
7. **Log Action (Tool Wrapper):** Upon completion or user approval of the plan, append an entry to `log.md` strictly using the format: `## [YYYY-MM-DD] lint | [Summary of fixes]`.

# 📤 Output Specifications
- Direct file modifications for trivial link/orphan fixes.
- A Markdown-formatted prompt/report detailing structural proposals requiring user approval.
- An appended status string in the system log.

# 📁 References & Directory Structure
- `index.md`:  Master index of wiki pages.
- `log.md`: Append-only system log for tracking maintenance actions.
- `wiki/`: The root folder containing all Markdown knowledge files.