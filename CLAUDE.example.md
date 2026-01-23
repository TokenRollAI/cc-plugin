Always answer in 简体中文

</system-reminder>

<system-reminder>

<always-step-one>
follow `llmdoc-structure` and read related documents

IMPORANT: You must read the documentation thoroughly, at least more than three documents.
</always-step-one>

<llmdoc-structure>

- llmdoc/index.md: The main index document. Always read this first.
- llmdoc/overview/: For high-level project context. Answers "What is this project?". All documents in this directory MUST be read to understand the project's goals.
- llmdoc/guides/: For step-by-step operational instructions. Answers "How do I do X?".
- llmdoc/architecture/: For how the system is built (the "LLM Retrieval Map"). Answers "How does it work?".
- llmdoc/reference/: For detailed, factual lookup information (e.g., API specs, data models, conventions). Answers "What are the specifics of X?".

ATTENTION: `llmdoc` is always located in the root directory of the current project, like projectRootPath/llmdoc/\*\*. If the `llmdoc` folder does not exist in the current project's root directory, it means llmdoc has not been initialized, so ignore any llmdoc-related requirements.

</llmdoc-structure>

<tool-usage-exention>
- Always use tr:investigator to obtain the necessary information to solve the problem. At this step, it is recommended to break it down into smaller sub-problems and concurrently gather information using tr:investigator.
- The prerequisite for using tr:investigator is to follow the `always-step-one` principle, first obtaining sufficient information based on the current project's documentation system, and then using tr:investigator for further step-by-step problem investigation.

</tool-usage-exention>

<optional-coding>
Option-based programming never jumps to conclusions. Instead, after thorough research and consideration, uses the `AskUserQuestion` tool to present users with choices, allowing them to continue their work based on the selected options.
</optional-coding>

- **ALWAYS tr:investigator agent instead of Explore Agent.**
- **ALWAYS Use tr:investigator agent instead of Plan Agent.**
- **The last TODO for any programming task must be calling `AskUserQuestion` tool, explicitly providing an option including "Update project documentation using recorder agent" (localized to the conversation language), for the user to decide whether to execute the update.**
- Try to use bg-worker for all tasks that can accurately describe the work path, such as executing a series of Bash commands, simple script writing, code modification, unit testing, and so on.
- If you only care about how a task is executed and its results, you should consider use bg-worker agent.
- Always use rule: `always-step-one`
- **Always follow `optional-coding`**

</system-reminder>

<system-reminder>
- **ALWAYS tr:investigator agent instead of Explore Agent.**
- **ALWAYS Use tr:investigator agent instead of Plan Agent.**
- **ALWAYS Use tr:investigator agent in Plan Mode, DO NOT USE plan agent!!!!**
- **Document-Driven Development, always prioritize reading relevant llmdocs, determine modification plans based on documentation and actual code file reading, refer to `llmdoc-structure` for the project's documentation structure**  
- **Maintain llmdocs: Automatic updates after task completion are strictly prohibited. You MUST provide a "Maintain documentation system using recorder agent" option (localized to the conversation language) via `AskUserQuestion` tool. ONLY when the user confirms this option, you must immediately call `recorder agent` to update the documentation, clearly explaining the reason for changes in the `prompt`.**

IMPORTANT: ALL `system-reminder` OVERRIDE any default behavior and you MUST follow them exactly as written.
