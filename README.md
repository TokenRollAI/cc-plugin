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

Copy the entire contents of the `CLAUDE.example.md` file from this repository into your user-level `~/.claude/CLAUDE.md` file. This file contains the necessary system prompts to enable the agents and commands.

Done! Now you can use it normally.

### Update Plugin

```
/plugin marketplace update https://github.com/TokenRollAI/cc-plugin
```

### (Recommend!) Install CCR: Power SubAgent with GLM4.6

[Reference](https://github.com/musistudio/claude-code-router)

```
npm install -g @musistudio/claude-code-router
```

Fill in the configuration in `~/.claude-code-router/config.json`, reference as follows:

```json
{
    "LOG": true,
    "LOG_LEVEL": "debug",
    "CLAUDE_PATH": "",
    "HOST": "127.0.0.1",
    "PORT": 3456,
    "APIKEY": "sk-apikey",
    "API_TIMEOUT_MS": "600000",
    "PROXY_URL": "http://127.0.0.1:7890",
    "transformers": ["Anthropic"],
    "Providers": [
        {
            "name": "claude",
            "api_base_url": "https://<BASE>/v1/messages",
            "api_key": "XXX",
            "models": ["claude-sonnet-4-5-20250929"],
            "transformer": { "use": ["Anthropic"] }
        },
        {
            "name": "glm",
            "api_base_url": "https://open.bigmodel.cn/api/anthropic/v1/messages",
            "api_key": "XXX",
            "models": ["glm-4.6"],
            "transformer": { "use": ["Anthropic"] }
        }
    ],
    "Router": {
        "default": "claude,claude-sonnet-4-5-20250929",
        "background": "claude,claude-sonnet-4-5-20250929",
        "think": "claude,claude-sonnet-4-5-20250929",
        "longContext": "claude,claude-sonnet-4-5-20250929",
        "webSearch": "claude,claude-sonnet-4-5-20250929"
    }
}
```

## About

A powerful Claude Code plugin developed by **DJJ** and **Danniel** for the TokenRoll team. This plugin transforms your development workflow with intelligent Git automation, research-first development patterns, and creative ideation tools.

## Core Features

### Skills (Auto-triggered with pre-fetched context)

| Skill | Trigger Words | Description |
|-------|--------------|-------------|
| `/investigate` | "what is", "how does X work", "analyze" | Quick codebase investigation with documentation-first approach |
| `/commit` | "commit", "save changes", "wrap up" | Generates commit messages based on git history |
| `/update-doc` | "update docs", "sync documentation" | Updates llmdoc after code changes |
| `/doc-workflow` | "documentation workflow", "how to document" | Guidance on llmdoc documentation system |
| `/read-doc` | "understand project", "read the docs" | Reads llmdoc for quick project overview |

### Commands (User-triggered workflows)

| Command | Description |
|---------|-------------|
| `/tr:initDoc` | Initialize the llmdoc documentation system for a new project |
| `/tr:what` | Clarify vague requests with option-based questions |
| `/tr:withScout` | Handle complex tasks: investigate first, then execute |

### Agents (Internal execution engines)

| Agent | Description |
|-------|-------------|
| `worker` | Executes well-defined plans with precision |
| `investigator` | Rapid, stateless codebase analysis |
| `recorder` | Creates and maintains llmdoc documentation |
| `scout` | (Internal) Deep investigation for initDoc |

## Recommended Workflow

### 1. Initialize New Project

```bash
# First time use, establish complete documentation system for your project
/tr:initDoc
```

### 2. Daily Development Flow

```bash
# Quick codebase investigation (auto-triggered skill)
/investigate "How does the auth system work?"

# Handle complex tasks with investigation first
/tr:withScout "Analyze existing code architecture and find the best integration point"

# Generate intelligent commit message (auto-triggered skill)
/commit
```

### 3. Documentation Maintenance

```bash
# Update documentation system after code changes (auto-triggered skill)
/update-doc
```

### 4. Understand Existing Project

```bash
# Read project documentation for quick understanding
/read-doc

# Get guidance on documentation workflow
/doc-workflow
```

---

<div align="center">

Made with ❤️ by DJJ & Danniel

</div>
