---
name: what
description: "Clarifies a vague user request by asking clarifying questions."
disable-model-invocation: true
---

# /what

This skill is used internally when a user's request is too vague to be acted upon. It reads the project documentation to understand the context and then asks the user targeted, option-based questions to clarify their intent.

## When to use

- **Use when:** This skill is typically used by the main assistant AI, not directly by the user. It's triggered when the user's prompt is ambiguous (e.g., "fix it", "add a thing").
- **Goal:** To turn a vague request into a concrete, actionable plan.

## Actions

1. **Step 1: Gather Context**
   - Read the documentation index at `<projectRootPath>/llmdoc/index.md` and other high-level documents to understand the project's purpose, architecture, and features.

2. **Step 2: Formulate Clarifying Questions**
   - Based on the documentation and the user's vague request, formulate a set of clarifying questions.
   - The questions should be option-based whenever possible to guide the user toward a specific outcome. For example, instead of "What do you want to do?", ask "Are you trying to: (a) Add a new API endpoint, (b) Modify an existing feature, or (c) Fix a bug?".

3. **Step 3: Ask the User**
   - Use the `AskUserQuestion` tool to present the questions to the user.

4. **Step 4: Formulate Investigation Task**
   - Based on the user's clarified response, your goal is to formulate a set of concrete **investigation questions**.
   - **Do NOT jump to a solution.** The purpose of this skill is to clarify "what the user wants to know", not "how to implement it".
   - Invoke the `/withScout` skill with the clear, factual questions you have formulated. For example, if the user now wants to "add a user endpoint", the next step is to ask `/withScout` to investigate "What is the current API routing structure?" and "What conventions are used for defining data models?".
