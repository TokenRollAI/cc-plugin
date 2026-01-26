# TokenRoll Claude Code Plugin

<div align="center">

**llmdoc + SubAgent RAG: Solve the Context Floor Problem**

[![GitHub](https://img.shields.io/badge/GitHub-TokenRollAI%2Fcc--plugin-blue?logo=github)](https://github.com/TokenRollAI/cc-plugin)

[English](README.md) | [简体中文](README.zh-CN.md)

</div>

---

## The Problem: Context Floor

In serious production environments, AI Coding Agents face a fundamental challenge: **they don't truly understand your codebase**. They achieve understanding through CLAUDE.md + massive code file reading, which leads to:

- Endless tool calls before reaching sufficient context
- High token consumption with low information density
- Slow Time to Context Ready (TTCR)

We call the "minimum context richness required for an Agent to solve a task" the **Context Floor**.

### Existing Solutions Fall Short

| Approach | Tool Calls | Token Usage | Info Density | Effectiveness |
|----------|-----------|-------------|--------------|---------------|
| LSP MCP | High | Medium | High | Good, but slow |
| ACE / RAG | Low | Low | Sparse | Poor correlation |
| Agentic RAG (Explorer) | Medium | Low | High | Good, but TTCR too slow |

## Our Solution: llmdoc + SubAgent RAG

**Fast. High-density. Low main-agent token usage. Strongly correlated with tasks.**

### llmdoc

A documentation system designed from the ground up for AI to quickly acquire high-density information while remaining human-readable.

Based on [Diataxis](https://diataxis.fr/), optimized for LLM retrieval:

```
llmdoc/
├── index.md          # Entry point - always read first
├── overview/         # "What is this project?" - MUST read all
├── guides/           # "How do I do X?" - step-by-step instructions
├── architecture/     # "How does it work?" - LLM retrieval map
└── reference/        # "What are the specifics?" - API specs, conventions
```

**Key Design Principles:**
- Leverages Agent's ability to batch-read files quickly
- Documents retain critical file paths + module descriptions
- Project overview + architecture + topic-linked guides + references

Example: [TokenRoll/minicc/llmdoc](https://github.com/TokenRollAI/minicc/tree/main/llmdoc)

### SubAgent RAG

Two primary functions:
1. **Investigation**: Based on llmdoc + existing code, investigate decomposed tasks as prerequisites
2. **Recording**: After completing coding tasks, automatically maintain llmdoc

---

## Quick Start

### Step 1: Install Plugin

```bash
# Add TokenRoll plugin marketplace
/plugin marketplace add https://github.com/TokenRollAI/cc-plugin

# Install tr plugin
/plugin install tr@cc-plugin
```

### Step 2: Configure System Prompt

Copy the contents of [`CLAUDE.example.md`](CLAUDE.example.md) into your `~/.claude/CLAUDE.md` file.

**That's it.** Once configured, all behaviors activate automatically:

- Agent will **always read llmdoc first** before any action
- Investigation uses **documentation-first approach**
- After coding tasks, Agent will **ask if you want to update docs**
- All skills trigger automatically based on context

### Update Plugin

```bash
/plugin marketplace update https://github.com/TokenRollAI/cc-plugin
```

---

## How It Works

### Automatic Behaviors (No Commands Needed)

Once `CLAUDE.example.md` is configured, these behaviors are **always active**:

| Behavior | What Happens |
|----------|--------------|
| **Documentation First** | Agent reads `llmdoc/` before any action |
| **Smart Investigation** | Uses `investigator` agent instead of generic exploration |
| **Option-Based Coding** | Never jumps to conclusions; presents choices via questions |
| **Doc Maintenance Prompt** | After coding, asks if you want to update documentation |

### Available Skills (Auto-Triggered)

These skills activate automatically based on your prompts:

| Skill | Triggers | Description |
|-------|----------|-------------|
| `/investigate` | "what is", "how does X work", "analyze" | Quick codebase investigation |
| `/commit` | "commit", "save changes" | Generate commit message |
| `/update-doc` | "update docs", "sync documentation" | Update llmdoc |
| `/read-doc` | "understand project", "read the docs" | Read llmdoc overview |

### Commands (When You Need Control)

| Command | Description |
|---------|-------------|
| `/tr:initDoc` | Initialize llmdoc for a new project |
| `/tr:withScout` | Complex tasks: deep investigation first, then execute |
| `/tr:what` | Clarify vague requests with structured questions |

---

## Recommended Workflow

### For New Projects

```bash
# Initialize documentation system
/tr:initDoc
```

### For Daily Development

Just talk naturally. The system handles the rest:

```
"How does the auth system work?"
# -> Auto-triggers /investigate, reads llmdoc first

"Add a new API endpoint for user profiles"
# -> Reads llmdoc, investigates, implements, asks about doc update

"commit"
# -> Auto-triggers /commit with intelligent message
```

---

## Cost & Effectiveness

**Honest assessment**: This approach costs approximately **1.5x more** to achieve a jump from 85 to 90 points in task completion quality.

- Simple projects: Marginal benefit
- Complex projects: Significant benefit
- Production codebases (100k+ lines): Excellent results

In our production backend (100k lines of code):
- Task completion cost: **$1-5 per feature**
- Human intervention: **Significantly reduced**
- Output quality: **Ready for review and minor adjustments**

---

## Internal Agents

| Agent | Purpose |
|-------|---------|
| `worker` | Execute well-defined plans with precision |
| `investigator` | Rapid, stateless codebase analysis |
| `recorder` | Create and maintain llmdoc documentation |
| `scout` | Deep investigation for initDoc |

---

<div align="center">

Made with care by **DJJ** & **Danniel** for the TokenRoll team

</div>
