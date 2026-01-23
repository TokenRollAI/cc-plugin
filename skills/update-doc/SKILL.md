---
name: update-doc
description: "Updates the documentation based on recent code changes."
argument-hint: "[Optional: specific update instructions]"
disable-model-invocation: true
context: fork
agent: recorder
allowed-tools: Bash(git:*), Read, Write, Edit, Grep, Glob, Task
---

# /updateDoc

This skill updates the project's documentation to reflect recent code changes. If specific instructions are provided, it follows them. Otherwise, it analyzes the latest `git diff` to determine what needs updating.

## Pre-fetched Context

- **Recent changes:** !`git diff HEAD --stat 2>/dev/null | head -30`

## When to use

- **Use when:** The user wants to update the documentation after making code changes.
- **Suggest when:** A feature has been implemented or a bug has been fixed, and the user indicates the task is complete.
- **Example:** "User: I've just pushed the changes, please update the docs."
- **Example:** "User: The refactor is done." -> Assistant suggests `/updateDoc`.

## User Instructions

$ARGUMENTS

## Actions

1. **Step 1: Analyze Changes**
   - If arguments (`$ARGUMENTS`) are provided, use them as the high-level description of what changed.
   - If no arguments are provided, run `git diff HEAD` to get recent code changes.

2. **Step 2: Synthesize Impacted Concepts**
   - Perform a synthesis step: Analyze the `git diff` or user's description to identify which core concepts, features, or foundational conventions have been affected.
   - For example, a change to `.eslintrc` impacts "Coding Conventions". A change to `src/services/authService.js` impacts the "Authentication System" concept.
   - Create a list of all impacted concepts/conventions.

3. **Step 3: Concurrent Document Updates (using `recorder` in `content-only` mode)**
   - For each impacted concept identified in Step 2, concurrently invoke a `recorder` agent.
   - The prompt for each will be:
     "**Task:** The **`<concept_name>`** has been updated.
     **1. Analyze these changes:** `<relevant part of git diff or user description>`.
     **2. Read the existing `llmdoc` documentation thoroughly** to understand what documents are impacted by the changes.
     **3. Holistically update all relevant documents** to reflect the changes, ensuring they remain accurate and consistent. You may need to create, modify, or even delete documents.
     **4. Apply the 'Principle of Minimality':** Your updates must be as concise as possible. Use the fewest words necessary to describe the change. Do not write long-winded explanations.
     **5. You MUST operate in `content-only` mode.**"

4. **Step 4: Final Indexing (using a single `recorder`)**
   - After all `recorder` agents from Step 3 have completed, invoke a **single** `recorder` agent with the task: "The documentation has been updated. Please re-scan the `/llmdoc` directory and ensure the `index.md` is fully consistent and up-to-date. Operate in `full` mode."
