---
name: ingest
description: Processes and injects raw source files into the Wiki, extracting concepts, updating indices, and logging the ingestion.
version: 1.0.0
pattern: Pipeline
---

# 🎯 Purpose
To systematically process uningested raw source files, extract their core arguments and concepts, integrate them into a structured markdown wiki, and maintain an accurate log of ingested materials.

# 🚪 Gating & Trigger Conditions
- **When to invoke:** A user requests to ingest new files, or when unlogged files are detected in the `raw/` directory.
- **When NOT to invoke:** The target file already has a corresponding entry in `log.md`, or the file type is entirely unsupported by the agent's reading tools (e.g., binaries, executables).

# 📥 Input Specifications
- **Target Directory:** `raw/` (containing target source files such as `.pdf`, `.md`, `.txt`).
- **Log State:** The current contents of `log.md` to determine previously ingested files.

# ⚙️ Execution Instructions (Workflow)

1. **Identify Unprocessed Files:**
   - Scan the `raw/` directory.
   - Parse `log.md` for existing entries matching the format: `## [YYYY-MM-DD] ingest | [File Name]`.
   - Create an internal queue of files present in `raw/` but missing from `log.md`.
   - *Fallback:* If `log.md` does not exist, assume all files in `raw/` are unprocessed and create `log.md`.

2. **Process Incrementally (Sequential Loop):**
   - For each file in the queue, execute steps 2a through 2e synchronously. Do not begin the next file until the current file's pipeline is complete.

   - **a. Read & Extract:**
     - Utilize file-reading tools to parse the document's full contents.
     - Extract core arguments, context, and key entities/concepts.
     - *Fallback:* If the file is unreadable or corrupted, append an error note to `log.md` (e.g., `## [YYYY-MM-DD] FAILED ingest | [File Name]`) and proceed to the next file.

   - **b. Draft Summary Page:**
     - Generate a new markdown file in `wiki/summaries/` named appropriately based on the source.
     - Structure the breakdown using `AGENTS.md` conventions (clear titles, relative links). 

   - **c. Update Central Index:**
     - Read `index.md`.
     - Append a relative link to the newly created summary page under the `### Sources` section.
     - Include a one-line summary describing the source's main point adjacent to the link.

   - **d. Update Concept Nodes:**
     - Identify distinct concepts from the extracted data.
     - For each concept, check if a corresponding file exists in `wiki/concepts/<concept name>.md` or is listed in `index.md`.
     - *If it exists:* Read the existing concept page, synthesize the new findings into the existing text, and add a backlink to the new summary page.
     - *If it does NOT exist:* Create `wiki/concepts/<concept name>.md` containing the concept definition and a backlink to the source summary page.

   - **e. Log Action:**
     - Append an entry to `log.md` using the exact format: `## [YYYY-MM-DD] ingest | [Exact File Name](wiki/summaries/<summary_filename>.md)`.
     - The source title MUST be a relative Markdown link pointing to the newly created summary page in `wiki/summaries/`.
     - Ensure the file name exactly matches the item in `raw/` to prevent duplicate future ingestions.

# 📤 Output Specifications
- **Summaries:** Markdown files written to `wiki/summaries/`.
- **Concepts:** Markdown files created or updated in `wiki/concepts/`.
- **Index:** Updated `index.md` with new source links.
- **Log:** Updated `log.md` with timestamped ingestion records.

# 📁 References & Directory Structure
- `raw/`: Directory for un-processed source documents.
- `wiki/summaries/`: Directory for generated document summaries.
- `wiki/concepts/`: Directory for isolated, atomic concept pages.
- `index.md`: The central directory and routing page for the wiki.
- `log.md`: The ledger tracking all historical file ingestions.
- `AGENTS.md`: Reference file dictating styling conventions, titles, and relative linking rules.