---
name: model-router
description: Intelligent model routing for OpenClaw. Route tasks to Sonnet 4.6 or Opus 4.6 based on complexity, saving cost without sacrificing quality.
version: 1.0.0
homepage: https://github.com/chandika/openclaw-model-router
metadata: {"clawdbot":{"emoji":"🧭"}}
---

# Model Router for OpenClaw

Route tasks to the right Claude model automatically. Sonnet 4.6 handles 80% of tasks at 1/5th the cost of Opus. Opus handles the 20% that need deep reasoning.

## Why

Sonnet 4.6 (released Feb 17, 2026) now approaches Opus-level performance on most tasks:
- **SWE-bench Verified**: Sonnet 79.6% vs Opus 83.7%
- **OSWorld**: Sonnet 72.5% vs Opus 66.3% (Sonnet WINS)
- **GPQA Diamond**: Sonnet 89.9% vs Opus 91.3%
- **ARC-AGI-2**: Sonnet 60.4% vs Opus 68.8%
- **Terminal-Bench**: Sonnet 59.1% vs Opus 65.4%
- **Pricing**: Sonnet $3/$15 vs Opus $15/$75 per million tokens (5x cheaper)

Developers preferred Sonnet 4.6 over Opus 4.5 in 59% of head-to-head tests.

## When to Use Opus

Opus 4.6 is worth the 5x cost premium ONLY for:

1. **Deep codebase refactoring** — multi-file architectural changes requiring understanding of entire systems
2. **Multi-agent coordination** — orchestrating complex workflows across multiple agents
3. **Novel problem solving** — tasks requiring creative leaps, not pattern matching
4. **High-stakes reasoning** — where "getting it just right" matters more than speed (legal analysis, financial modeling, security audits)
5. **Very long multi-step plans** — 10+ step agent workflows where error compounding is a risk

## When Sonnet 4.6 is Better (or Equal)

- **Code generation** — writing new code, tests, components
- **Code review** — reading and analyzing existing code
- **Content creation** — writing, editing, summarizing
- **Data analysis** — parsing, transforming, extracting
- **Research & search** — web searches, document analysis
- **Routine agent tasks** — file operations, API calls, standard workflows
- **Long-context work** — 1M token context window matches Opus
- **Computer use / browser automation** — Sonnet actually scores HIGHER on OSWorld

## OpenClaw Configuration

### Option 1: Sonnet Default + Opus Fallback
Set Sonnet 4.6 as primary, Opus as fallback for when you need it:

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-sonnet-4-6",
        "fallbacks": ["anthropic/claude-opus-4-6"]
      }
    }
  }
}
```

Then use `/model opus` in chat when you need deep reasoning, or `/model default` to switch back.

### Option 2: Task-Based Routing via Subagents
Keep Opus for main session (direct chat needs best reasoning), use Sonnet for subagents (background tasks, research, drafts):

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-opus-4-6"
      },
      "subagents": {
        "model": "anthropic/claude-sonnet-4-6"
      }
    }
  }
}
```

This is the recommended setup. Your direct conversation gets Opus-quality reasoning. Background tasks (spawned subagents) use Sonnet at 1/5th cost.

### Option 3: Cron + Heartbeat on Sonnet
Periodic tasks (daily dashboard, heartbeat checks, scheduled jobs) don't need Opus:

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-opus-4-6"
      },
      "subagents": {
        "model": "anthropic/claude-sonnet-4-6"
      },
      "heartbeat": {
        "model": "anthropic/claude-sonnet-4-6"
      }
    }
  }
}
```

Cron jobs with `sessionTarget: "isolated"` will also use the subagent model.

## Cost Comparison (Estimated)

For a typical OpenClaw user doing ~50 interactions/day:

| Setup | Monthly Est. | Savings |
|-------|-------------|---------|
| All Opus | ~$150-300 | baseline |
| Opus main + Sonnet subagents | ~$80-150 | ~50% |
| Sonnet default + Opus on-demand | ~$40-80 | ~70% |

## Quick Apply

To switch your OpenClaw to the recommended setup (Option 2), ask your agent:

> "Set my main model to Opus 4.6 and subagents to Sonnet 4.6"

Or apply the config patch directly via the gateway config tool.

## The Meta Point

The model routing question isn't "which model is better?" It's "which model is better *for this specific task at this specific cost*?"

Sonnet 4.6 makes this question matter. Before, the gap between tiers was obvious. Now, 80% of what you'd use Opus for, Sonnet handles equally well — at 1/5th the price.

The smartest AI teams in 2026 aren't using one model. They're routing intelligently.
