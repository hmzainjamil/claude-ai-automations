# claude-ai-automations
macOS LaunchAgent automations — AI-powered cron jobs, sync daemons, and background workers.

![launchagents](https://img.shields.io/badge/LaunchAgents-12%2B-blue?style=flat&labelColor=555)
![platform](https://img.shields.io/badge/platform-macOS-lightgrey?style=flat&labelColor=555)
![always-on](https://img.shields.io/badge/mode-always_on-green?style=flat&labelColor=555)
![zero-cost](https://img.shields.io/badge/runtime_cost-zero-brightgreen?style=flat&labelColor=555)
![license](https://img.shields.io/badge/license-MIT-blue?style=flat&labelColor=555)

[Concepts](#-concepts) · [Architecture](#️-architecture) · [Tips](#-tips-and-tricks-20) · [Kills](#️-startups--businesses) · [Stars](#star-history)

## 🧠 CONCEPTS

| Feature | Location | Description |
|---------|----------|-------------|
| [**GitHub Portfolio Sync**](launchagents/ai.hmz.github-portfolio-sync.plist) | `launchagents/ai.hmz.github-portfolio-sync.plist` | Daily 6:30 AM — syncs all local scripts to GitHub repos via Contents API [![daily](https://img.shields.io/badge/schedule-6%3A30_AM-blue?style=flat&labelColor=555)] |
| [**OpenClaw Gateway**](launchagents/ai.openclaw.gateway.plist) | `launchagents/ai.openclaw.gateway.plist` | KeepAlive daemon — Open Design local MCP at 127.0.0.1:51827 |
| [**Ollama LLM Server**](launchagents/ai.hmz.ollama.plist) | `launchagents/ai.hmz.ollama.plist` | Permanent Ollama server — Llama 3 on GPU, zero API cost |
| [**Paperclip CEO**](launchagents/ai.hmz.paperclip-ceo.plist) | `launchagents/ai.hmz.paperclip-ceo.plist` | DigiMinds AI CEO loop — 127.0.0.1:3100, company `c5066522` |
| [**Reddit Poster**](launchagents/ai.hmz.reddit-poster.plist) | `launchagents/ai.hmz.reddit-poster.plist` | Throttled Reddit posting — max 1 post/day (bot-watch compliance) |
| [**BDM Sweep**](launchagents/ai.hmz.bdm-sweep.plist) | `launchagents/ai.hmz.bdm-sweep.plist` | Morning LinkedIn + Indeed job scan — LinkedIn/Indeed only (no Upwork) |
| [**Log Rotate**](launchagents/ai.hmz.log-rotate.plist) | `launchagents/ai.hmz.log-rotate.plist` | Weekly cleanup — prunes ~/.claude/logs older than 30 days |
| [**Auto Troubleshoot**](launchagents/ai.hmz.auto-troubleshoot.plist) | `launchagents/ai.hmz.auto-troubleshoot.plist` | SessionStart check — flags stale LaunchAgents, missing env vars |

### 🔥 Hot

| Feature | Location | Description |
|---------|----------|-------------|
| [**Tier 0 Health Check**](launchagents/ai.hmz.tier0-health.plist) | `launchagents/ai.hmz.tier0-health.plist` | Hourly ping to Groq/Gemini/Ollama — logs availability for routing decisions |
| [**Session Queue Processor**](launchagents/ai.hmz.session-queue.plist) | `launchagents/ai.hmz.session-queue.plist` | Stop hook companion — converts session-queue.jsonl to memory files |

## ⚙️ ARCHITECTURE

```
LaunchAgent lifecycle:
  ~/Library/LaunchAgents/ai.hmz.*.plist
          │
          ▼ launchctl load
  System daemon (KeepAlive=true)
          │
          ├─ RunAtLoad: fires immediately on load
          ├─ StartCalendarInterval: cron-style schedule
          ├─ KeepAlive: restarts on crash
          └─ EnvironmentVariables: HOME, PATH, GITHUB_TOKEN
```

| Agent | Schedule | Runtime | Deps |
|-------|---------|---------|------|
| github-portfolio-sync | 06:30 daily | ~30s | gh, python3 |
| openclaw-gateway | KeepAlive | permanent | node |
| ollama | KeepAlive | permanent | ollama |
| paperclip-ceo | KeepAlive | permanent | node |
| bdm-sweep | 08:00 daily | ~5m | python3, gh |
| log-rotate | Sunday 02:00 | ~5s | bash |

## 💡 TIPS AND TRICKS (20)

[plist](#tips-plist) · [debug](#tips-debug) · [env](#tips-env) · [schedule](#tips-schedule)

<a id="tips-plist"></a>■ **Plist Authoring (6)**

| Tip | Source |
|-----|--------|
| Always include both `KeepAlive` AND `RunAtLoad` — missing either breaks persistence | [HMZ](https://github.com/hmzainjamil) |
| `Label` must match filename exactly — `ai.hmz.foo` → `ai.hmz.foo.plist` | [Apple Docs](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLaunchdJobs.html) |
| Use `StandardOutPath` + `StandardErrorPath` — logs are your only debug channel | [HMZ](https://github.com/hmzainjamil) |
| `EnvironmentVariables` must include `HOME` + `PATH` — shell profile not sourced | [HMZ](https://github.com/hmzainjamil) |
| Never use relative paths in plist — always absolute paths for all executables | [HMZ](https://github.com/hmzainjamil) |
| Test with `launchctl load -w ~/Library/LaunchAgents/ai.hmz.foo.plist` before committing | [HMZ](https://github.com/hmzainjamil) |

<a id="tips-debug"></a>■ **Debugging (5)**

| Tip | Source |
|-----|--------|
| `launchctl list | grep hmz` — check all running agents in one command | [HMZ](https://github.com/hmzainjamil) |
| Exit code `256` = script not found or permission denied (chmod +x required) | [HMZ](https://github.com/hmzainjamil) |
| `tail -f ~/.claude/logs/<agent>.log` — real-time agent output during testing | [HMZ](https://github.com/hmzainjamil) |
| `launchctl unload + load` restarts cleanly — faster than rebooting for testing | [HMZ](https://github.com/hmzainjamil) |
| `auto-troubleshoot` SessionStart hook catches most common plist issues automatically | [HMZ](https://github.com/hmzainjamil) |

<a id="tips-env"></a>■ **Environment Variables (4)**

| Tip | Source |
|-----|--------|
| `GITHUB_TOKEN` in plist — never rely on keychain from LaunchAgent context | [HMZ](https://github.com/hmzainjamil) |
| `PATH=/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin` — cover Apple Silicon + Intel | [HMZ](https://github.com/hmzainjamil) |
| Rotate tokens by updating plist + `launchctl unload/load` — no reboot needed | [HMZ](https://github.com/hmzainjamil) |
| Keep secrets out of plist in git — use `.env` file sourced by wrapper script | [HMZ](https://github.com/hmzainjamil) |

<a id="tips-schedule"></a>■ **Scheduling (5)**

| Tip | Source |
|-----|--------|
| `StartCalendarInterval` beats `StartInterval` — cron-style is more predictable | [Apple Docs](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLaunchdJobs.html) |
| `{"Hour":6,"Minute":30}` = 6:30 AM daily — use array for multiple times | [HMZ](https://github.com/hmzainjamil) |
| If machine is asleep at scheduled time, job runs on wake (if `KeepAlive=false`) | [HMZ](https://github.com/hmzainjamil) |
| KeepAlive daemons restart within 10s of crash — use for MCP servers | [HMZ](https://github.com/hmzainjamil) |
| Avoid scheduling multiple heavy jobs at same time — stagger by 5-10 minutes | [HMZ](https://github.com/hmzainjamil) |

## ☠️ STARTUPS / BUSINESSES

| Feature | Replaced |
|-|-|
| **LaunchAgent Automation** | [Zapier](https://zapier.com), [Make.com](https://make.com), [IFTTT](https://ifttt.com) |
| **Always-On MCP Daemons** | [Railway](https://railway.app), [Render](https://render.com) — runs locally at $0 |
| **Scheduled AI Jobs** | [n8n Cloud](https://n8n.io), [Activepieces](https://activepieces.com) |
| **Portfolio Sync** | [GitHub Actions](https://github.com/features/actions) — no cloud runner needed |
| **Log Management** | [Datadog](https://datadoghq.com), [Papertrail](https://papertrailapp.com) |

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hmzainjamil/claude-ai-automations&type=Date)](https://star-history.com/#hmzainjamil/claude-ai-automations&Date)
