# TokenRoll Claude Code Plugin

<div align="center">

**TokenRoll Claude Code Plugin: Best Practices for CC Coding**

[![GitHub](https://img.shields.io/badge/GitHub-TokenRollAI%2Fcc--plugin-blue?logo=github)](https://github.com/TokenRollAI/cc-plugin)

[English](README.md) | [简体中文](README.zh-CN.md)

</div>

---

## Installation

### Step 1: Install Plugin

```
# Add TokenRoll plugin marketplace
/plugin marketplace add https://github.com/TokenRollAI/cc-plugin

# Install tr plugin
/plugin install tr@cc-plugin
```

### Step 2: Configure System Prompt

Copy the entire contents of the `CLAUDE.example.md` file from this repository into your user-level `~/.claude/CLAUDE.md` file. This file contains the necessary system prompts to enable the agents and skills.

Done! Now you can use it normally.

### Update Plugin

```
/plugin marketplace update https://github.com/TokenRollAI/cc-plugin
```

## About

A powerful Claude Code plugin developed by **DJJ** and **Danniel** for the TokenRoll team. This plugin transforms your development workflow with intelligent Git automation, research-first development patterns, and creative ideation tools.

## Architecture

This plugin uses the **Skills** system with **fork-context** capabilities for better context isolation and efficiency.

### 🤖 Multi-Agent System

Agents are reusable execution units that skills can invoke:

- **`worker`** - Execution agent: Executes a given plan of actions, such as running commands or modifying files.
- **`investigator`** - Investigation agent: Performs quick, focused investigations and reports findings directly.
- **`scout`** - Deep investigation agent: Performs thorough codebase analysis and saves detailed reports to files.
- **`recorder`** - Documentation agent: Creates and maintains high-quality technical documentation about the codebase.

### 🎯 Skills (with fork-context)

Skills that run in isolated context for better performance:

| Skill | Fork Context | Description |
|-------|-------------|-------------|
| `/tr:commit` | ✅ | Intelligent commit message generator with pre-fetched git context |
| `/tr:init-doc` | ✅ | Initialize comprehensive documentation system |
| `/tr:with-scout` | ❌ | Complex task handling with investigate-then-execute workflow |
| `/tr:update-doc` | ✅ | Update documentation based on code changes |
| `/tr:what` | ❌ | Clarify vague requests |

**Fork-context benefits:**
- Isolated execution keeps main conversation clean
- Pre-fetched context via `!`command`` syntax
- Independent tool access for each skill

## Recommended Workflow

### 1. Initialize New Project

```bash
# First time use, establish complete documentation system for your project
/tr:init-doc
```

### 2. Daily Development Flow

```bash
# Get clear programming guidance
/tr:what "I need to implement user authentication feature"

# Perform deep code analysis
/tr:with-scout "Analyze existing code architecture and find the best integration point"

# Generate intelligent commit message
/tr:commit
```

### 3. Documentation Maintenance

```bash
# Update documentation system after code changes
/tr:update-doc
```

## Directory Structure

```
├── skills/           # Skills with SKILL.md files
│   ├── commit/       # (fork-context)
│   ├── init-doc/     # (fork-context)
│   ├── with-scout/
│   ├── what/
│   └── update-doc/   # (fork-context)
├── agents/           # Reusable agent definitions
│   ├── worker.md
│   ├── investigator.md
│   ├── recorder.md
│   └── scout.md
└── CLAUDE.example.md # System prompt configuration
```

---

<div align="center">

Made with ❤️ by DJJ & Danniel

</div>
