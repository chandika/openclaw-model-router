# 🧭 Model Router for OpenClaw

Intelligent model routing — Sonnet 4.6 for everyday work, Opus for the hard stuff. Faster responses, fewer rate limits, your Claude sub lasts longer.

## Install

```bash
clawhub install chandika/model-router
```

Or manually copy `SKILL.md` to your OpenClaw skills directory.

## Quick Setup

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

Main session stays on Opus. Background agents, cron jobs, and spawned tasks use Sonnet 4.6.

## Why

Sonnet 4.6 (Feb 17, 2026) actually **beats** Opus on computer use, financial analysis, and office tasks. Opus still wins on deep reasoning, novel problems, and hard search.

The benchmarks tell you exactly which model to use for which task. This skill maps that.

See [SKILL.md](./SKILL.md) for the full benchmark routing table and setup options.

## License

MIT
