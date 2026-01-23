<system-reminder>

## llmdoc System

If `llmdoc/` exists at project root, ALWAYS read `llmdoc/index.md` first, then relevant docs before code.

**Structure:**
- `llmdoc/overview/` - Project context
- `llmdoc/guides/` - Step-by-step instructions
- `llmdoc/architecture/` - System design (LLM Retrieval Map)
- `llmdoc/reference/` - Detailed specs

## Core Rules

1. **Use tr:investigator** instead of Explore/Plan agents
2. **Use tr:worker** for execution tasks
3. **Use tr:recorder** for documentation (requires user confirmation)
4. **Optional Coding:** Present choices via `AskUserQuestion`, don't decide unilaterally

## Agent Selection

| Task Type | Use Agent | NOT |
|-----------|-----------|-----|
| Quick investigation | `tr:investigator` | Explore |
| Planning/research | `tr:investigator` | Plan |
| Deep codebase analysis | `tr:scout` | general-purpose |
| Execution tasks | `tr:worker` | Bash directly |
| Documentation | `tr:recorder` | Write directly |

## Skills Reference

| Skill | Fork | Description |
|-------|------|-------------|
| `/tr:commit` | ✅ | Generate commit message |
| `/tr:init-doc` | ✅ | Initialize llmdoc |
| `/tr:with-scout` | ❌ | Complex task workflow |
| `/tr:update-doc` | ✅ | Update documentation |
| `/tr:what` | ❌ | Clarify vague requests |

## Agents Reference

| Agent | Model | Purpose |
|-------|-------|---------|
| `tr:investigator` | sonnet | Quick investigation, direct report |
| `tr:scout` | sonnet | Deep investigation, saves to file |
| `tr:worker` | sonnet | Execute action plans |
| `tr:recorder` | opus | Create/maintain documentation |

## Documentation Maintenance

**Automatic updates are strictly prohibited.**

After completing programming tasks:
1. Use `AskUserQuestion` to offer "Update project documentation using recorder agent"
2. ONLY execute when user confirms
3. Clearly explain changes in the prompt

</system-reminder>
