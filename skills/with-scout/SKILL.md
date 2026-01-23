---
name: with-scout
description: "Handles a complex task by first investigating the codebase, then executing a plan."
argument-hint: "[A complex goal or task]"
---

# /withScout

This skill handles complex tasks by breaking them down into an investigation phase and an execution phase. It uses `investigator` agents to gather information before deciding on a plan of action.

## When to use

- **Use when:** The user has a complex request that requires understanding the codebase before changes can be made.
- **Suggest when:** A user's request cannot be fulfilled without first gathering information from multiple files or parts of the codebase.
- **Example:** "User: Add a JWT token refresh feature."
- **Example:** "User: Figure out our project's auth logic and then add a new endpoint."

## User Task

$ARGUMENTS

## Actions

This skill follows an **Investigate -> Synthesize -> Iterate/Execute** workflow.

1. **Step 1: Deconstruct & Plan**
   - Break down the user's primary goal into a set of clear, independently investigable questions.
   - Assign each set of questions to a different `investigator` agent (e.g., Frontend Investigator, Backend Investigator).

2. **Step 2: Parallel Investigation**
   - Use the `Task` tool to launch multiple `investigator` agents concurrently.
   - Each investigator will research its assigned questions and return a direct markdown report.

3. **Step 3: Synthesize & Evaluate**
   - Combine the reports from all investigators to form a holistic view of the system.
   - Identify key connections, knowledge gaps, or conflicts in the information.

4. **Step 4: Iterate or Execute**
   - **If information is insufficient (Iterate):** Formulate a new, more specific round of research questions and go back to Step 2.
   - **If information is sufficient (Execute):** Proceed to the Action Phase (Step 5).

5. **Step 5: Action Phase**
   - Based on the comprehensive information gathered, create a plan and use `worker` agents to execute the user's final request (e.g., implement the feature, fix the bug).

6. **Step 6: Summarize & Report**
   - When delivering the final result, explain the investigation process, the key findings, and the actions taken to achieve the outcome.
