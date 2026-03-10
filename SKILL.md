---
name: token-burn-monitor
description: "Real-time token consumption monitoring dashboard for OpenClaw agents. Tracks per-agent token usage, cost breakdown by model, cache hit rates, cron job status, and 30-day historical trends. Use when setting up cost monitoring for agents, checking daily token burn or cost, debugging which prompts cost the most, or reviewing cron job health. Features expandable user prompt tracking in per-call breakdowns."
---

# Token Burn Monitor

Zero-dependency Node.js dashboard for monitoring OpenClaw agent token consumption.

## Quick Start

```bash
bash start.sh            # Start (default port 3847)
bash start.sh status     # Check status
bash start.sh restart    # Restart
bash start.sh stop       # Stop
```

Dashboard: `http://localhost:3847`

## Configuration

Copy `config.default.json` → `config.json` to customize:

```json
{
  "port": 3847,
  "agents": {
    "main": { "name": "Karl", "icon": "/assets/karl.png" },
    "helper": { "name": "Helper Bot", "icon": null }
  },
  "modelPricing": {
    "custom/model": { "input": 2, "output": 8, "cacheRead": 0.2, "cacheWrite": 2, "provider": "Custom" }
  }
}
```

- **agents**: Display names and icons. Agents are auto-discovered from the agents directory; config only overrides display.
- **port**: Dashboard port. Also settable via `PORT` env var.
- **modelPricing**: Add/override model pricing ($ per 1M tokens).

Set `OPENCLAW_AGENTS_DIR` env var to override the default agents directory (`/home/node/.openclaw/agents`).

## Dashboard Features

- **Overview stats**: Total tokens, queries, messages, cache hits, daily cost
- **Agent cards**: Per-agent breakdown with input/output/cache metrics, model costs, last active time
- **Breakdown modal**: Click an agent card → per-call table with time, model, token distribution, tool calls, cost
- **Prompt tracking**: Click a row in breakdown → expand to see the user prompt that triggered it
- **Cost history**: Click the cost stat → 30-day history with per-agent drill-down
- **Cron jobs**: Scheduled job overview grouped by agent, with run history

## Agent Icons

Place custom icons in `public/assets/` and reference them in config:
```json
{ "main": { "name": "Karl", "icon": "/assets/karl.png" } }
```

Agents without icons get auto-generated initials with deterministic colors.

## Troubleshooting

- **No data**: Check that `OPENCLAW_AGENTS_DIR` points to the correct directory with `.jsonl` session files.
- **Port in use**: Change port in config.json or `PORT=4000 bash start.sh`
- **Stale process**: `bash start.sh stop && bash start.sh start`
