---
name: commit
description: "Analyzes code changes and generates a conventional commit message."
disable-model-invocation: true
context: fork
agent: investigator
allowed-tools: Bash(git:*), Read, Grep, Glob, AskUserQuestion
---

# /commit

This skill analyzes staged and unstaged changes to generate a high-quality commit message that follows the project's existing style.

## Pre-fetched Context

- **Staged changes:** !`git diff --staged 2>/dev/null || echo "No staged changes"`
- **Unstaged changes:** !`git diff 2>/dev/null || echo "No unstaged changes"`
- **Current status:** !`git status --short 2>/dev/null`
- **Recent commits:** !`git log --oneline -10 2>/dev/null`

## Actions

1. **Step 1: Analyze Changes**
   - Review the pre-fetched git diff information above.
   - If there are no changes, inform the user and stop.
   - If there are only unstaged changes, ask the user if they want to stage files first.

2. **Step 2: Generate Commit Message**
   - Based on the git information, generate a commit message that:
     - Follows the project's historical style (e.g., conventional commits, emoji usage).
     - Accurately and concisely describes the changes.
     - Explains the "why" behind the change, not just the "what".

3. **Step 3: Propose and Commit**
   - Use the `AskUserQuestion` tool to present the generated message to the user.
   - Ask if they want to use the message to commit, edit it, or cancel.
   - If the user agrees to commit, run the `git commit -m "<message>"` command.
