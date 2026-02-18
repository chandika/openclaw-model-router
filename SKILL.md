---
name: model-router
description: Intelligent model routing for OpenClaw. Route tasks to Sonnet 4.6 or Opus 4.6 based on task complexity — faster responses, fewer rate limits, and your Claude sub lasts longer.
version: 1.0.0
homepage: https://github.com/chandika/openclaw-model-router
metadata: {"clawdbot":{"emoji":"🧭"}}
---

# Model Router for OpenClaw

Route the right model to the right job. Sonnet 4.6 for the everyday work, Opus for the hard stuff.

## Why I Built This

My Claude subscription was teetering on usage limits. Sonnet 4.6 shipped with 1M context and near-parity with Opus on most tasks. Using the same expensive model for everything stopped making sense.

So I asked OpenClaw to solve the problem itself — gave it the benchmarks, told it to figure out routing logic, and this skill is the result.

## How to Read Benchmark Tables

Don't read them as "which model is better." Read them as a **routing table**. Each row tells you which model to assign to which job.

### Sonnet 4.6 Wins

| Benchmark | What It Measures | Sonnet 4.6 | Opus 4.6 |
|-----------|-----------------|------------|----------|
| OSWorld | Computer use / browser | **72.5%** | 66.3% |
| Finance Agent v1.1 | Financial analysis | **63.3%** | 60.1% |
| GDPval-AA Elo | Office tasks | **1633** | 1606 |
| Pace Insurance | Computer use accuracy | **94%** | — |

**→ Route to Sonnet:** Browser automation, file operations, research, drafts, scheduled jobs, computer use, financial analysis, office work.

### Opus 4.6 Wins

| Benchmark | What It Measures | Sonnet 4.6 | Opus 4.6 |
|-----------|-----------------|------------|----------|
| Terminal-Bench 2.0 | Terminal coding | 59.1% | **65.4%** |
| BrowseComp | Agentic search | 74.7% | **84.0%** |
| ARC-AGI-2 | Novel problem-solving | 58.3% | **68.8%** |
| GPQA Diamond | Graduate reasoning | 89.9% | **91.3%** |

**→ Route to Opus:** Architecture decisions, deep debugging, complex multi-step planning, novel problems, hard search tasks.

### Basically a Tie

| Benchmark | What It Measures | Sonnet 4.6 | Opus 4.6 |
|-----------|-----------------|------------|----------|
| SWE-bench Verified | Agentic coding | 79.6% | 80.8% |
| OfficeQA | Document QA | Match | Match |

**→ Either works.** Default to Sonnet (faster, lower rate limit impact).

## Pricing Context

- **Sonnet 4.6**: $3 input / $15 output per million tokens
- **Opus 4.6**: $15 input / $75 output per million tokens
- **Sonnet is 5x cheaper** — but for Claude Pro/Max subscribers, the real win is speed and rate limits, not cost.

## OpenClaw Setup

### Recommended: Opus Main + Sonnet Subagents

Keep Opus for your direct conversation (where you need the best reasoning). Route background agents, cron jobs, and spawned tasks to Sonnet.

Add to your `openclaw.json`:

```json
{
  "agents": {
    "defaults": {
      "subagents": {
        "model": "anthropic/claude-sonnet-4-6"
      }
    }
  }
}
```

Or ask your agent: "Set subagents to Sonnet 4.6"

That's it. One config change and your agents start routing intelligently.

### Alternative: Sonnet Default + Opus On-Demand

If you're on a $20 Pro plan and want maximum stretch:

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-sonnet-4-6"
      }
    }
  }
}
```

Then use `/model anthropic/claude-opus-4-6` when you need deep reasoning, `/model default` to switch back.

## The Meta Point

Skills and models are now good enough to be self-reliant on picking the right tool. If you give an agent a selection of models and a framework for choosing, it picks well.

Use the bigger model to pick the smaller models for the job. That's what this skill enables.

## Two Contexts

**Production systems** (APIs, SaaS products): Run proper evals. Measure model fit per task with real data. No shortcuts.

**Personal agent workflows** (OpenClaw, daily use): If benchmarks are within a few points, vibes are fine. Close enough is good enough.

This skill is for the second case. For production, build your own eval suite.
