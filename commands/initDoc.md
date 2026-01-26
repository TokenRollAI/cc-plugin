---
description: Generate great doc system for this project
---

## Actions

0. STEP 0:
   - Obtain the current project structure.
   - Read key files, such as various README.md / package.json / go.mod / pyproject.toml ...
   - **Do NOT** read project dependency directories, such as `node_modules` / `venv` / `target` / `build` ...

1. **Step 1: Global Investigation (using `scout`)**
   - Launch concurrent `scout` agents to explore the codebase and produce reports.
   - **At most use 4 `scout` agents to explore!**
   - **At most use 4 `scout` agents to explore!**
   - NEVER RUN `scout` background, NERVER USE get task output get running `scout` output, JUST RUN AND WAIT!

2. **Step 2: Propose Core Concepts & Get User Selection**
   - After scouting is complete, perform a synthesis step: Read all scout reports and generate a list of _candidate_ core concepts (e.g., "Authentication", "Billing Engine", "API Gateway").
   - Use the `AskUserQuestion` tool to present this list to the user as a multiple-choice question: "I've analyzed the project and found these potential core concepts. Please select the ones you want to document now:".

3. **Step 3: Generate Concise Foundational Documents**
   - In parallel, launch dedicated `recorder` agents to create essential, project-wide documents.
   - **Task for Recorder A (Project Overview):** "Create `overview/project-overview.md`. Analyze all scout reports to define the project's purpose, primary function, and tech stack."
   - **Task for Recorder B (Coding Conventions):** "Create a _concise_ `reference/coding-conventions.md`. Analyze project config files (`.eslintrc`, `.prettierrc`) and extract only the most important, high-level rules."
   - **Task for Recorder C (Git Conventions):** "Create a _concise_ `reference/git-conventions.md`. Analyze `git log` to infer and document the primary branch strategy and commit message format."
   - **Mode:** These recorders MUST operate in `content-only` mode.

4. **Step 4: Document User-Selected Concepts**
   - Based on the user's selection from Step 2, for each _selected_ concept, concurrently invoke a `recorder` agent.
   - The prompt for this `recorder` will be highly specific to control scope and detail:
     "**Task:** Holistically document the **`<selected_concept_name>`**.
     **1. Read all relevant scout reports and source code...**
     **2. Generate a small, hierarchical set of documents:**
     - **Optionally, create ONE `overview` document** if the concept is large enough to require its own high-level summary (e.g., `overview/authentication-overview.md`).
     - **Create 1-2 primary `architecture` documents.** This is mandatory and should be the core 'LLM Retrieval Map'.
     - **Create 1-2 primary `guide` documents** that explain the most common workflow for this concept (e.g., `how-to-authenticate-a-user.md`).
     - **Optionally, create 1-2 concise `reference` documents** ONLY if there are critical, well-defined data structures or API specs. Do not create reference docs for minor details.
       **3. Operate in `content-only` mode.**"

5. **Step 5: Cleanup**
   - Delete the temporary scout reports in `/llmdoc/agent/`.

6. **Step 6: Final Indexing**
   - After all `recorder` agents from both Step 3 and Step 4 have completed, invoke a single `recorder` in `full` mode to build the final `index.md` from scratch.
