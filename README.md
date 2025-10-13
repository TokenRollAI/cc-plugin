# TokenRoll Claude Code Plugin

<div align="center">

**Solve Claude Code's context explosion problem with sub-agent architecture**

Save 83% context | Research-driven development | Intelligent workflows

[![GitHub](https://img.shields.io/badge/GitHub-TokenRollAI%2Fcc--plugin-blue?logo=github)](https://github.com/TokenRollAI/cc-plugin)

[English](README.md) | [简体中文](README.zh-CN.md)

</div>

---

## About

A powerful Claude Code plugin developed by **DJJ** and **Danniel** for the TokenRoll team. This plugin transforms your development workflow with intelligent Git automation, research-first development patterns, and creative ideation tools.

## ✨ Core Features

### 🤖 Intelligent Git Workflow
**`/tr:commit`** - Smart commit message generator that learns from your project's Git history
- Analyzes your commit style (emoji usage, conventional commits, language)
- Generates contextual messages describing the "why", not just the "what"
- Follows your team's existing conventions automatically

### 🔍 Context-Efficient Research (Core Innovation)
**`/tr:withScout`** + **Scout Agent** - **Save 83% context with sub-agent architecture**

**The Problem:**
Claude Code struggles with medium-to-large projects due to context explosion from full-file reads.
- Standard approach: **83K context** → Easy to hit limits ❌

**The Solution:**
Sub-agent performs heavy reads → Delivers concise summaries → Main agent stays clean
- withScout approach: **29K context** → Same quality, 83% less tokens ✅

**Key advantages:**
- 🎯 **83% context reduction** in real-world usage (29K vs 83K tokens)
- 🔒 Independent context isolation in sub-agents
- 📚 Massive read operations don't pollute main context
- ⚡ Parallel research across multiple areas
- 💎 Structured, high-value outputs only

**Ideal for:** refactoring, bug fixing, feature planning, documentation in medium-to-large codebases

*Based on:* [Claude Code Sub-agents](https://docs.claude.com/en/docs/claude-code/sub-agents) | [Discussion](https://linux.do/t/topic/1031049)

### 💡 Viral Product Ideation
**Super-Idea Agent** - Transform trending topics into viral product concepts
- Analyzes viral mechanics and emotional triggers
- Designs AI-powered product concepts optimized for sharing
- Prioritizes novelty and explosive growth potential
- Perfect for brainstorming sessions and innovation sprints

### 💬 Interactive Thinking Partner
**`/tr:discuss`** + **Discuss Agent** - Think through complex topics via structured dialogue
- **Versatile:** Requirements, architecture, debugging, decisions, processes
- **Adaptive:** Questions adjust based on your discussion goal
- **Efficient:** 80% multiple-choice questions, 3-5 rounds typical
- **Context-aware:** Can integrate with scout for existing codebase discussions
- **Output-focused:** Produces structured summaries ready for action

**The Challenge:**
Jumping into implementation without thinking leads to unclear requirements, unexamined trade-offs, and costly rework.

**The Solution:**
Invest 5-10 minutes in structured dialogue → Clarify thinking → Act with confidence

**Perfect for:** Requirements clarification, architecture decisions, debugging strategies, naming conventions, workflow design, or anytime you need to think something through before acting

---

## 🚀 Quick Start

### Installation

Run this command in Claude Code:

```bash
# Add the TokenRoll plugin marketplace
/plugin marketplace add https://github.com/TokenRollAI/cc-plugin

# Install the plugin
/plugin install tr@cc-plugin
```

After installation, **restart Claude Code** to activate the plugin.

### Basic Usage

```bash
# Generate an intelligent commit message
/tr:commit

# Research-first workflow: understand before implementing
/tr:withScout I want to refactor the authentication system

# For viral ideation, invoke the super-idea agent directly in conversation
```

---

## 🧠 How withScout Solves Context Explosion

### The Context Problem in Claude Code

Claude Code excels at small projects but struggles with medium-to-large codebases due to a fundamental issue: **context explosion from full-file reads**.

**The standard workflow:**
1. User asks: "What's the role of Operator? How is data stored and transferred?"
2. Claude reads 50+ files to understand the codebase
3. All file contents loaded into conversation context
4. **Result: 83K tokens consumed** (excluding 17K initial context)
5. Context limit approaches quickly, conversation restarts needed

[Reference discussion on Linux.do →](https://linux.do/t/topic/1031049)

### The Sub-Agent Solution

**withScout leverages Claude Code's sub-agent architecture to isolate heavy operations:**

```
┌─────────────────────────────────────────────────────┐
│  Main Agent (Your Conversation)                     │
│  Context: Clean & Focused                           │
│  ✅ Receives only structured summaries               │
└─────────────────────────────────────────────────────┘
              ↓ Delegates research
┌─────────────────────────────────────────────────────┐
│  Scout Sub-Agent (Isolated Context)                 │
│  • Reads 50+ files across the codebase              │
│  • Greps patterns and searches symbols              │
│  • Performs web searches for documentation          │
│  • Synthesizes findings into concise report         │
│  Context: Heavy operations isolated here ⚡          │
└─────────────────────────────────────────────────────┘
              ↓ Returns summary only
┌─────────────────────────────────────────────────────┐
│  Main Agent receives:                               │
│  ✅ 【Research Summary】 - Key findings              │
│  ✅ 【Recommendations】 - Actionable next steps      │
│  ❌ NOT: 50 full file contents dumped in context    │
└─────────────────────────────────────────────────────┘
```

**Key principle:** Sub-agents have independent context that doesn't pollute the main conversation. Perfect for read-heavy → summarize workflows.

### Real-World Impact

**Test case:** *"What's the role of Operator in this project? How is data stored and transferred?"*

| Approach | Main Agent Context | Context Breakdown | Savings |
|----------|-------------------|-------------------|---------|
| **Without sub-agent** | 83K tokens | Initial: 17K + Research: **70K** | - |
| **With `/tr:withScout`** | 29K tokens | Initial: 17K + Summary: **12K** | **83%** |

**Calculation:**
- Standard approach adds **70K context** to main agent (full file reads)
- withScout adds only **12K context** to main agent (structured summary)
- **Savings: 58K tokens = 83% reduction**

### Why This Matters

✅ **Handle larger projects** - Work with codebases that would normally blow up context
✅ **Longer conversations** - Continue working without hitting token limits
✅ **Parallel research** - Launch multiple scouts without context pollution
✅ **Cleaner experience** - Main agent stays focused on your actual task
✅ **Cost efficiency** - Use fewer tokens while maintaining quality

### Implementation Details

- **Scout Agent:** Specialized sub-agent with Read, Glob, Grep, WebSearch, WebFetch tools ([source](https://github.com/TokenRollAI/cc-plugin/blob/main/agents/scout.md))
- **withScout Command:** Workflow orchestrator that invokes scout(s) and processes results ([source](https://github.com/TokenRollAI/cc-plugin/blob/main/commands/withScout.md))
- **Architecture:** Built on [Claude Code's sub-agents](https://docs.claude.com/en/docs/claude-code/sub-agents) with independent context isolation

---

## 📖 Features in Detail

### Commands

#### `/tr:commit` - Intelligent Commit Message Generator

Automates the entire commit workflow with style awareness:

**What it does:**
1. Gathers git information (diff, status, log) in parallel
2. Analyzes your project's commit history to learn style patterns
3. Generates a commit message that matches your conventions
4. Offers to commit, modify, or regenerate

**Example workflow:**
```bash
/tr:commit

# Output:
# Analyzing changes...
# - Found 3 modified files in src/auth/
# - Detected conventional commits style with emoji
# - Recent commits use "feat:", "fix:", "refactor:" prefixes
#
# Suggested commit message:
# ✨ feat: add JWT token refresh mechanism
#
# - Implement automatic token refresh before expiration
# - Add refresh token rotation for security
# - Handle edge cases for offline token refresh
#
# [Commit] [Modify] [Regenerate]
```

**Key features:**
- Learns emoji usage patterns from your git log
- Detects conventional commits, gitmoji, or custom formats
- Respects character limits (50-72 for English)
- Describes motivation and impact, not just changes

---

#### `/tr:withScout` - Research-First Workflow

A meta-workflow that researches before implementing:

**Pattern:**
```
User request → Scout research → Implementation → Summary
```

**Use cases:**

```bash
# Scenario 1: Refactoring
/tr:withScout I want to refactor the user authentication system
# → Scout gathers: current auth implementation, dependencies, patterns
# → You receive: structured findings + recommended approach
# → Then: implement with full context

# Scenario 2: Bug fixing
/tr:withScout Fix the cache invalidation issue in Redis
# → Scout investigates: cache implementation, Redis config, known issues
# → You receive: root cause analysis + fix recommendations
# → Then: apply the fix confidently

# Scenario 3: Documentation
/tr:withScout Create API documentation for the payment endpoints
# → Scout collects: endpoint definitions, request/response schemas, auth requirements
# → You receive: comprehensive endpoint information
# → Then: generate accurate documentation
```

**Why use it?**
- Saves time by automating research
- Ensures you have full context before coding
- Reduces trial-and-error iterations
- Supports parallel research for complex tasks

**Context efficiency:**
- 🎯 **83% context reduction** compared to standard approach (29K vs 83K tokens)
- Scout performs heavy reads in isolated sub-agent context
- Main agent receives only high-value summaries, not raw file dumps
- Enables exploration of large codebases without hitting context limits
- Perfect for medium-to-large projects where context management is critical

---

#### `/tr:discuss` - Interactive Thinking Partner

Think through complex topics via structured, adaptive dialogue before taking action.

**The Problem:**
Jumping into implementation without thinking leads to:
- Unclear requirements → wasted iterations
- Unexamined trade-offs → regrettable decisions
- Missed edge cases → bugs and rework
- Fuzzy goals → scope creep

**The Solution:**
Invest 5-10 minutes in structured dialogue to clarify your thinking before acting. The discuss agent adapts its questioning strategy based on your topic.

**Versatile Discussion Types:**

```bash
# Requirements clarification
/tr:discuss clarify requirements for user authentication feature

# Architecture decisions
/tr:discuss decide between microservices vs monolith

# Debugging strategy
/tr:discuss troubleshoot why API responses are slow

# Naming/conventions
/tr:discuss naming conventions for React components

# Process design
/tr:discuss define our code review workflow

# Performance optimization
/tr:discuss optimize database query performance

# Security considerations
/tr:discuss security implications of storing user data

# Testing approach
/tr:discuss testing strategy for payment integration
```

**Adaptive Frameworks:**

The agent selects the appropriate questioning framework based on your topic:

- **Framework A (Requirements):** Project context → Functional scope → Technical details → Non-functional needs
- **Framework B (Architecture):** Constraints → Trade-offs → Alternatives → Risks
- **Framework C (Debugging):** Problem definition → Context → Investigation → Hypotheses
- **Framework D (Decisions):** Options → Criteria → Constraints → Implications
- **Framework E (Process):** Goal → Stakeholders → Steps → Edge cases

**Key Features:**

1. **80/20 Question Strategy**
   - 80% multiple-choice (fast, accurate responses)
   - 20% open-ended (when options unpredictable)
   - Always includes "Other" option for nuance

2. **Progressive Depth**
   - Starts with 2-3 high-level questions
   - Narrows based on your answers
   - Typically 3-5 rounds to full clarity

3. **Active Listening**
   - Summarizes understanding before each round
   - Confirms alignment as dialogue progresses

4. **Scout Integration**
   - For existing codebase topics, may suggest scout first
   - Scout gathers context → Discuss asks targeted questions
   - Example: "Let me use scout to understand current auth implementation, then we'll discuss the refactoring approach"

**Example Output Formats:**

Discuss produces structured summaries adapted to your topic:

- **Requirements:** User stories, acceptance criteria, technical specs, implementation roadmap
- **Architecture:** Options table, pros/cons, decision criteria, recommendation
- **Debugging:** Problem definition, hypotheses, investigation plan, next actions
- **Decisions:** Context, options evaluated, trade-offs, recommendation with rationale
- **Process:** Workflow steps, roles/responsibilities, edge case handling

**Why Use It:**

- ✅ Avoid costly rework by thinking first
- ✅ 5-10 minutes of dialogue saves hours of iteration
- ✅ Multiple-choice format is fast and easy
- ✅ Adaptive strategy fits any discussion type
- ✅ Produces actionable documentation
- ✅ Especially valuable for complex or unfamiliar topics

**When to Use:**

- ✅ Fuzzy ideas that need structure
- ✅ Decisions with multiple options
- ✅ Need to examine trade-offs
- ✅ Complex debugging requiring systematic approach
- ✅ Process or workflow design

**When to Skip:**

- ❌ Requirements already crystal clear
- ❌ Trivial, one-step tasks
- ❌ Pure research (use scout instead)
- ❌ Just exploring/learning (not deciding)

---

### Agents

#### `tr:scout` - Professional Research Specialist

A specialized agent for information gathering from codebase and web.

**Available tools:** Read, Glob, Grep, WebSearch, WebFetch

**Core capabilities:**
- Deep codebase analysis (file search, content grep, pattern matching)
- Web research for documentation, best practices, and solutions
- Context-aware research (avoids duplicate work when given known context)
- Parallel research support (multiple scouts can work simultaneously)

**Output format:**
```
【Research Summary】
Brief overview of findings

【Key Findings】
• Critical discovery 1
• Critical discovery 2
• Critical discovery 3

【Detailed Analysis】
In-depth investigation results...

【Recommendations】
Actionable next steps...

【Sources】
Files and URLs examined
```

**Advanced usage - Parallel scouting:**
```bash
# Launch multiple scouts for complex research
# Scout 1: Backend architecture
# Scout 2: Frontend components
# Scout 3: Database schema
# Scout 4: API integrations
# Scout 5: Web research on best practices
```

---

#### `tr:discuss` - Interactive Thinking Partner

A versatile discussion facilitator that helps you think through complex topics via structured, adaptive dialogue.

**Available tools:** Read, Glob, Grep (for codebase context when needed)

**Core Philosophy:**
- Invest 5-10 minutes in structured thinking → Avoid hours of rework
- Adaptive questioning based on discussion type
- 80% multiple-choice questions → Fast and accurate
- Scout integration → Context-aware for existing projects

**Adaptive Frameworks:**

The agent selects the right questioning strategy based on your topic:

1. **Requirements Clarification**
   - Project context → Functional scope → Technical details → Non-functional needs
   - Example: "Clarify requirements for user authentication"

2. **Architecture Discussion**
   - Constraints → Trade-offs → Alternatives → Risks
   - Example: "Decide between microservices vs monolith"

3. **Debugging Strategy**
   - Problem definition → Context → Investigation → Hypotheses
   - Example: "Troubleshoot why API responses are slow"

4. **Decision Making**
   - Options → Criteria → Constraints → Implications
   - Example: "Choose between library A and library B"

5. **Process Design**
   - Goal → Stakeholders → Steps → Edge cases
   - Example: "Define our code review workflow"

**Dialogue Flow:**

```
User: "/tr:discuss [topic]"
        ↓
Agent: Analyzes topic, selects appropriate framework
        ↓
Agent: Asks 2-3 high-level questions (Round 1)
        ↓
User: Answers (mostly multiple choice)
        ↓
Agent: Summarizes understanding, asks deeper questions (Round 2-4)
        ↓
User: Continues answering
        ↓
Agent: Presents structured summary
        ↓
User: Confirms or adjusts
        ↓
Agent: Outputs final summary + suggested next steps
```

**Output Formats (Adapted to Topic):**

- **Requirements:** User stories, acceptance criteria, tech specs, roadmap
- **Architecture:** Options table, pros/cons, decision criteria, recommendation
- **Debugging:** Problem definition, hypotheses, investigation plan
- **Decision:** Context, options evaluated, trade-offs, recommendation
- **Process:** Workflow steps, roles, exception handling

**Key Features:**

1. **80/20 Question Strategy**
   - 80% multiple-choice (3-5 options + "Other")
   - 20% open-ended (when options unpredictable)

2. **Progressive Depth**
   - Starts broad, narrows based on answers
   - 3-5 rounds typical
   - User can stop anytime

3. **Active Listening**
   - Summarizes understanding before each round
   - Confirms alignment throughout

4. **Scout Integration**
   - For existing codebase topics, may suggest scout first
   - Scout gathers context → Discuss asks targeted questions

**Versatile Discussion Scope:**

**Technical topics:**
- Feature requirements
- Architecture decisions
- Debugging strategies
- Performance optimization
- Security considerations
- Testing approaches

**Process topics:**
- Workflow design
- Team conventions
- Review processes
- Deployment strategies

**Decision topics:**
- Technology choices
- Trade-off evaluations
- Priority setting
- Refactoring plans

**Success Metrics:**

A successful discussion:
- ✅ 3-5 rounds to full clarity
- ✅ 80%+ questions are multiple-choice
- ✅ Structured, actionable output
- ✅ User feels heard and understood
- ✅ Ready to act with confidence

---

#### `tr:super-idea` - Viral Product Idea Generator

Transforms trending topics into explosive product concepts.

**Available tools:** Read, Write, Grep, Glob

**5-Step methodology:**
1. **Deconstruct the Trend** - Identify emotional triggers (humor, anger, curiosity, pride)
2. **Design Core Mechanics** - Create simple, addictive interactions (15-second attention capture)
3. **Inject AI Capabilities** - Leverage AIGC, AI recommendations, or AI analysis
4. **Engineer Viral Triggers** - Design explosive sharing mechanisms
5. **Output the Concept** - Structured, actionable format

**Output format:**
```
【Concept Name】
Catchy, memorable product name

【One-Line Pitch】
"[Product] is [description] that [unique value]"

【Core Mechanics】
• How users interact (15 seconds to hook)
• Key features (3-5 bullet points)

【AI Integration】
• How AI powers the experience
• Specific AI capabilities used

【Viral Trigger Analysis】
• Why users will share
• Psychological hooks
• Network effects
```

**Philosophy:**
- Virality first, business model later
- Reject validated markets, pursue novelty
- Design for explosive sharing, not steady growth
- Perfect for brainstorming, not production planning

---

## 🛠️ Development Guide

### Project Structure

```
cc-plugin/
├── .claude-plugin/
│   ├── marketplace.json  # Marketplace manifest
│   └── plugin.json       # Plugin configuration (id: "tr")
├── commands/             # Custom slash commands
│   ├── hello.md         # Demo command
│   ├── commit.md        # Git workflow automation
│   ├── withScout.md     # Research-driven workflow
│   └── discuss.md       # Requirement clarification workflow
├── agents/               # Custom agents
│   ├── helper.md        # General purpose assistant
│   ├── scout.md         # Research specialist
│   ├── discuss.md       # Requirement clarification expert
│   └── super-idea.md    # Viral ideation tool
└── hooks/                # Event hooks
    └── hooks.json       # Hook configuration
```

### Adding New Commands

Create a new `.md` file in the `commands/` directory:

```markdown
---
description: Brief command description
---

# Command Title

Detailed instructions for Claude Code on how to execute this command.
Include step-by-step workflow, expected inputs, and outputs.
```

### Adding New Agents

Create a new `.md` file in the `agents/` directory:

```markdown
You are [agent name], a [role description].

## Core Capabilities
- Capability 1
- Capability 2

## Workflow
1. Step 1
2. Step 2

## Output Format
Specify the expected output structure...
```

### Configuring Hooks

Edit `hooks/hooks.json` to add event hooks that trigger on specific actions (tool calls, user prompts, etc.).

---

## 📚 Resources

- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [Plugin Development Guide](https://docs.claude.com/en/docs/claude-code/plugins)
- [Plugin Marketplaces](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)

---

## 📄 License

Internal use only - TokenRoll Team

---

<div align="center">

Made with ❤️ by DJJ & Danniel

</div>
