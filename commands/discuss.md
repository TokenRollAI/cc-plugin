---
description: Facilitate structured thinking through dialogue - adapts to any discussion topic from requirements to architecture to debugging strategies
---

# Discuss Command - Interactive Thinking Partner

Think through complex topics via structured dialogue before taking action.

## Usage

```bash
/tr:discuss [your topic]
```

## Why Use This

**Problem**: Jumping into implementation without thinking → unclear requirements, bad decisions, costly rework

**Solution**: 5-10 minutes of structured dialogue → clarify thinking → act with confidence

## Discussion Types

The agent adapts to your topic automatically:

```bash
# Requirements
/tr:discuss clarify requirements for user authentication

# Architecture
/tr:discuss decide between microservices vs monolith

# Debugging
/tr:discuss troubleshoot slow API responses

# Decisions
/tr:discuss naming conventions for React components

# Process
/tr:discuss define code review workflow
```

## How It Works

1. **You state your topic** → Agent selects questioning framework
2. **Round 1-2**: High-level questions (mostly multiple-choice)
3. **Round 3-4**: Deep dive into specifics
4. **Final**: Structured summary + next steps

**Typical duration**: 3-5 rounds, 5-10 minutes

## Key Features

- **80% multiple-choice questions** (fast, accurate)
- **Adaptive frameworks** (5 types: Requirements, Architecture, Debugging, Decision, Process)
- **Active listening** (summarizes understanding each round)
- **Scout integration** (can gather codebase context first)

## Output Examples

**Requirements Discussion** → User stories, acceptance criteria, roadmap

**Architecture Discussion** → Options comparison, pros/cons, recommendation

**Debugging Discussion** → Problem definition, hypotheses, investigation plan

**Decision Discussion** → Context, trade-offs, recommendation with rationale

**Process Discussion** → Workflow steps, roles, exception handling

## Workflow Options

### Standard Flow
```
/tr:discuss [topic]
→ Discuss asks questions (3-5 rounds)
→ Structured summary
→ Ready to implement
```

### With Scout (for existing codebases)
```
/tr:discuss refactor authentication
→ Main agent detects codebase involvement
→ Scout investigates current implementation
→ Discuss asks targeted questions based on findings
→ Structured summary
```

## When to Use

✅ **Use when:**
- Fuzzy ideas need structure
- Multiple options to weigh
- Complex debugging needed
- Process/workflow design
- Want to avoid costly rework

❌ **Skip when:**
- Requirements crystal clear
- Trivial one-step task
- Pure research (use scout)
- Just exploring/learning

## Tips

1. **Provide context upfront**: "We're using React 18, need dark mode, not sure about persistence"
2. **Answer honestly**: Pick "Other" if needed, say "I don't know" if unsure
3. **Stop when ready**: You control depth, can say "let's start" anytime
4. **Combine with scout**: For existing projects, let agent suggest scout first

## Implementation Notes (for Main Agent)

When user runs `/tr:discuss [topic]`:

1. **Analyze topic**: Identify discussion type
2. **Check if scout needed**: Keywords like "refactor"/"existing" → suggest scout
3. **Invoke discuss agent**: Pass topic + scout results (if any)
4. **After completion**: Review summary, ask if user wants to proceed with implementation
