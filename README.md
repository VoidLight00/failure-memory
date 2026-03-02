# 🧠 Failure Memory

> **Never repeat the same mistake twice.** An automatic failure pattern recording & recall system for [OpenClaw](https://openclaw.ai) agents.

[![ClawHub](https://img.shields.io/badge/ClawHub-failure--memory--log-blue?style=flat-square)](https://clawhub.com/skills/failure-memory-log)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

---

## Why?

AI agents make mistakes — misconfigurations, wrong flags, API quirks, permission errors. Without a structured way to log and recall these failures, **the same mistakes get repeated across sessions.**

Failure Memory solves this by:

1. **Recording** every failure with context, root cause, and resolution
2. **Recalling** relevant past failures before starting similar tasks
3. **Reporting** patterns so systemic issues get fixed

## How It Works

```
Error occurs → Log to memory/failures.md → Next similar task → Auto-recall → Avoid repeat
```

### Recording Format

```markdown
## [2026-03-02 12:30] Docker permission denied on socket

- **Category:** permissions
- **Context:** Trying to run `docker compose up` for local dev
- **Error:** `Got permission denied while trying to connect to the Docker daemon socket`
- **Root Cause:** User not in docker group after fresh OS install
- **Resolution:** `sudo usermod -aG docker $USER && newgrp docker`
- **Prevention:** Check docker group membership in setup scripts
- **Tags:** docker, permissions, linux, setup
```

### Pre-Task Recall

Before starting any significant task, the agent searches `memory/failures.md` for relevant history and surfaces known pitfalls:

> ⚠️ Known pitfall: Docker permission denied — check docker group membership first

## Installation

### Via ClawHub (recommended)

```bash
clawhub install failure-memory-log
```

### Via Git

```bash
git clone https://github.com/VoidLight00/failure-memory.git
cp -r failure-memory /path/to/your/openclaw/workspace/skills/
```

### Initialize

```bash
bash skills/failure-memory/scripts/init.sh
```

## Features

| Feature | Description |
|---------|-------------|
| 🔴 Auto-Record | Logs failures when commands exit non-zero |
| 🔍 Pre-Task Recall | Searches past failures before similar work |
| 📊 Failure Reports | Groups by category, identifies repeat patterns |
| 🏷️ Tag System | Keyword tags for precise grep/search |
| 📁 Zero Dependencies | Plain markdown — no databases, no APIs |
| 🔗 Vector Search | Works with `memory_search` when available |

## Failure Categories

| Category | Examples |
|----------|---------|
| `build` | Compile errors, missing deps, version conflicts |
| `deploy` | Failed deploys, env misconfig, port conflicts |
| `config` | Wrong settings, missing env vars, path issues |
| `api` | Rate limits, auth failures, schema changes |
| `permissions` | File access, docker socket, sudo issues |
| `data` | Corruption, migration failures, encoding |
| `logic` | Wrong assumptions, race conditions, edge cases |
| `network` | DNS, SSL, timeout, proxy issues |
| `dependency` | Breaking updates, missing packages, conflicts |

## Failure Report Example

```markdown
# Failure Report — 2026-03-02

## Stats
- Total: 23 failures recorded
- Top category: config (8 occurrences)
- Repeat offenders: 3 patterns seen 2+ times

## Repeat Patterns
### ENV var not loaded in production
- Seen: 4 times
- Root cause: .env not copied to deploy target
- Systemic fix: Add .env validation to CI pipeline
```

## Design Philosophy

- **Append-only** — Never delete failure records, they're all lessons
- **Grep-friendly** — Structured markdown that works with `grep -i`
- **Zero overhead** — No external services, databases, or API keys
- **Agent-native** — Designed for AI agents, works with any OpenClaw setup

## Requirements

- [OpenClaw](https://openclaw.ai) (any version)
- No external dependencies

## License

MIT

---

Built with 🐙 by [VoidLight00](https://github.com/VoidLight00)
