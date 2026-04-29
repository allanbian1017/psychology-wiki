---
name: query
description: Queries the local wiki index to answer complex questions, synthesizes derived outputs, and proposes saving novel insights back to the knowledge base.
version: 1.0.0
pattern: Pipeline
---

# 🎯 Purpose
To conduct complex research and synthesis strictly against a local wiki knowledge base, generate derived artifacts (like presentations or charts) upon request, and iteratively expand the wiki by proposing and logging novel insights.

# 🚪 Gating & Trigger Conditions
- **When to invoke:** The user asks a complex question requiring synthesis of internal documentation, or specifically requests data/presentations based on the local wiki.
- **When NOT to invoke:** The user asks general knowledge questions requiring external web search, or requests simple edits to a single known file.

# 📥 Input Specifications
- **User Query:** The specific research question or topic.
- **Output Format Request (Optional):** Explicit requests for specific derived artifacts (e.g., Marp markdown presentation, Python `matplotlib` script).

# ⚙️ Execution Instructions (Workflow)
1. **Map Knowledge Base:** Read `index.md` to identify the core concepts and file paths relevant to the user's query.
   - *Fallback:* If `index.md` is not found or empty, alert the user that the wiki index is missing and halt execution.
2. **Fetch Source Context:** Retrieve and read the specific markdown files identified in Step 1.
3. **Synthesize & Render (Generator):** Generate the answer relying strictly on the fetched internal wiki files, prioritizing internal citations over generic external knowledge. 
   - If the user requested a specific artifact, format the output accordingly (e.g., wrap Marp code in standard markdown code blocks, or output the `matplotlib` Python script).
4. **Evaluate for Novelty:** Analyze the synthesized answer. If it establishes new connections or substantial analytical depth not present in the source files, proceed to Step 5. Otherwise, terminate the workflow and output the answer.
5. **Propose File-back (Inversion):** Pause execution and ask the user for explicit approval to save the novel synthesis. Example prompt: *"This analysis generated new insights. Would you like me to save this as `<topic>_analysis.md` and add it to the index?"*
6. **Execute Save & Log:** Upon user approval:
   - Create `wiki/derived/<topic>_analysis.md`.
   - Add a reference link to the new file in `index.md`.
   - Append a creation/query event log to `log.md`.
   - *Fallback:* If write permissions fail, output the raw markdown so the user can save it manually.

# 📤 Output Specifications
- **Primary Output:** Structured Markdown answer containing the synthesis, or code blocks containing requested derived artifacts.
- **Secondary Output:** A clear, interactive yes/no prompt if a novel insight is detected (Inversion step).

# 📁 References & Directory Structure
- `index.md`: The central directory file used for mapping concepts to wiki pages.
- `wiki/`: The root directory containing the detailed markdown topic pages.
- `log.md`: The append-only file used for tracking queries and newly generated wiki files.