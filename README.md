# Token Burn Monitor

Real-time token consumption monitoring dashboard for [OpenClaw](https://openclaw.ai) agents.

Track per-agent token usage, cost breakdown by model, cache hit rates, cron job health, and 30-day historical trends — all from a zero-dependency Node.js server with swappable frontend themes.

![Node.js](https://img.shields.io/badge/Node.js-18%2B-339933?logo=node.js&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue)
![Version](https://img.shields.io/badge/version-5.0.0-brightgreen)

## Features

- **Per-agent breakdown** — token counts, cost, model usage, cache hit rates
- **Per-call drill-down** — expandable rows showing user prompt, tool calls, and token split
- **30-day cost history** — daily bar chart with agent-level stacking
- **Cron job monitoring** — schedule, last run status, consecutive errors
- **Model pricing table** — built-in pricing for major providers, override via config
- **Theme system** — swap the entire frontend by dropping an `index.html` in `themes/<name>/`
- **Zero dependencies** — pure Node.js stdlib, no `npm install` needed

## Quick Start

```bash
# Start the dashboard (default port 3847)
bash start.sh

# Check status
bash start.sh status

# Restart after config change
bash start.sh restart

# Stop
bash start.sh stop
```

Dashboard: **http://localhost:3847**

## Configuration

Copy `config.default.json` to `config.json` and edit:

```json
{
  "port": 3847,
  "theme": "default",
  "agents": {
    "main": { "name": "Karl", "icon": "/assets/karl.png" }
  },
  "modelPricing": {}
}
```

| Key | Description |
|---|---|
| `port` | Server port. Also settable via `PORT` env var. |
| `theme` | Directory name under `themes/`. |
| `agents` | Display name & icon overrides. Agents are auto-discovered. |
| `modelPricing` | Override/add model pricing ($/1M tokens). |

Set `OPENCLAW_AGENTS_DIR` to point to your agents directory (default: `/home/node/.openclaw/agents`).

## Architecture

```
server.js              Core API server (stable, don't modify)
themes/default/        Default dark dashboard theme
themes/<custom>/       Drop your own theme here
API.md                 Full API contract for theme developers
config.default.json    Default configuration template
start.sh               Service management (start/stop/restart/status)
setup.sh               Post-install info
```

## API

All endpoints return JSON. Full reference in [API.md](./API.md).

| Endpoint | Description |
|---|---|
| `GET /api/config` | Agent names and icons |
| `GET /api/stats?date=` | All agents aggregated for a date |
| `GET /api/agent/:id?date=` | Single agent with per-call breakdown |
| `GET /api/history?days=` | Daily cost history (default 30 days) |
| `GET /api/pricing` | Model pricing table |
| `GET /api/crons` | Scheduled cron jobs |
| `GET /api/cron/:id/runs` | Job run history |

## Custom Themes

Create `themes/my-theme/index.html`, set `"theme": "my-theme"` in config, restart. Your theme fetches data from the API endpoints — see `themes/default/` as a reference implementation.

## Install

**Via [skills.sh](https://skills.sh):**

```bash
npx skills add KasparChen/burn-monitor-skill
```

**Via [ClawHub](https://clawhub.com):**

```bash
npx clawhub install token-burn-monitor
```

## License

MIT
