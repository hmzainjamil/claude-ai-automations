# claude-ai-automations
> LaunchAgents, cron jobs, and scheduled workflows — 12 persistent macOS automations running 24/7.

[![launchagents](https://img.shields.io/badge/launchagents-12-blue?style=flat&labelColor=555)](launchagents/)
[![workflows](https://img.shields.io/badge/workflows-8-green?style=flat&labelColor=555)](workflows/)
[![plist](https://img.shields.io/badge/plist-macOS-orange?style=flat&labelColor=555)](launchagents/)
[![mae](https://img.shields.io/badge/MAE-daily--ops-purple?style=flat&labelColor=555)](workflows/)
[![license](https://img.shields.io/badge/license-MIT-lightgrey?style=flat&labelColor=555)](LICENSE)

[concepts](#concepts) · [architecture](#architecture) · [tips](#tips) · [startups](#startups) · [star](#star)

---

## 🧠 CONCEPTS <a id="concepts"></a>

| Feature | Location | Description |
|---|---|---|
| [**ai.hmz.mae-daily**](launchagents/ai.hmz.mae-daily.plist) | `launchagents/` | Runs `mae daily` at 8am every day — full DigiMinds ops automation |
| [**ai.hmz.sys-optimize-daily**](launchagents/ai.hmz.sys-optimize-daily.plist) | `launchagents/` | Cache cleanup at 3am daily — npm, pip, brew, logs, TCC old tasks |
| [**ai.hmz.ollama**](launchagents/ai.hmz.ollama.plist) | `launchagents/` | Keeps Ollama running permanently — KeepAlive=true, GPU-accelerated |
| [**ai.hmz.paperclip**](launchagents/ai.hmz.paperclip.plist) | `launchagents/` | Paperclip CEO layer at http://127.0.0.1:3100 — on-demand |
| [**ai.hmz.openclaw.gateway**](launchagents/ai.openclaw.gateway.plist) | `launchagents/` | OpenClaw gateway — persistent agent relay |
| [**ai.hmz.github-portfolio-sync**](launchagents/ai.hmz.github-portfolio-sync.plist) | `launchagents/` | Auto-syncs new scripts/skills to GitHub repos daily |
| [**digiminds-daily workflow**](workflows/digiminds-daily.json) | `workflows/` | 4-step daily ops: check_systems → queue_tasks → run_pending → status_report |
| [**n8n workflows**](workflows/n8n/) | `workflows/n8n/` | 8,159 n8n workflow JSONs indexed and ready to import |

### 🔥 Hot

| Feature | Location | Description |
|---|---|---|
| [**ai.hmz.agency-email-pickup**](launchagents/) | `launchagents/` | SessionStart hook — auto-reads new agency emails and queues responses |
| [**ai.hmz.hmz-bdm-catchup**](launchagents/) | `launchagents/` | SessionStart hook — fetches new BDM leads from LinkedIn/Indeed |
| [**MAE workflow DAG**](workflows/digiminds-daily.json) | `workflows/` | JSON-defined workflow — each step is a TCC task with model routing |

---

## ⚙️ ARCHITECTURE <a id="architecture"></a>

```
macOS launchd
    │
    ├── 3:00am  →  sys-optimize-daily (cache cleanup)
    ├── 8:00am  →  mae-daily (full DigiMinds ops)
    ├── Permanent →  Ollama (GPU model server)
    ├── Permanent →  OpenClaw gateway
    └── SessionStart → paperclip, email-pickup, bdm-catchup

mae daily
    │
    ├── Step 1: check_systems (health check all Tier 0 LLMs)
    ├── Step 2: queue_daily_tasks (add today's tasks to TCC)
    ├── Step 3: run_pending (fire all pending tasks via MAE swarm)
    └── Step 4: status_report (summary → tcc-logs/mae-DATE.md)
```

| LaunchAgent | Schedule | Purpose |
|---|---|---|
| ai.hmz.mae-daily | 8:00am daily | Full DigiMinds ops |
| ai.hmz.sys-optimize-daily | 3:00am daily | Cache cleanup |
| ai.hmz.ollama | Permanent | Local LLM server |
| ai.hmz.github-portfolio-sync | Daily | GitHub sync |
| ai.openclaw.gateway | Permanent | Agent relay |

---

## 💡 TIPS AND TRICKS (12) <a id="tips"></a>

[launchagent-ops](#tips-la) · [workflow-design](#tips-wf) · [debugging](#tips-debug)

<a id="tips-la"></a>
■ **LaunchAgent Operations (4)**

| Tip | Source |
|---|---|
| Always set both `KeepAlive=true` AND `RunAtLoad=true` — missing either causes silent failures | [Apple launchd docs](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLaunchdJobs.html) |
| `EnvironmentVariables` dict must include `HOME` and `PATH` — plist jobs have no shell env | [hmzainjamil](https://github.com/hmzainjamil) |
| `launchctl list | grep ai.hmz` shows all loaded agents and their exit codes | [hmzainjamil](https://github.com/hmzainjamil) |
| Exit code 256 = permissions error or wrong binary path — check `which` in terminal vs plist | [hmzainjamil](https://github.com/hmzainjamil) |

<a id="tips-wf"></a>
■ **Workflow Design (4)**

| Tip | Source |
|---|---|
| Workflow JSON format: `{"name":"","steps":[{"id":"","task":"","agent":"","model":""}]}` | [hmzainjamil](https://github.com/hmzainjamil) |
| Each workflow step becomes a TCC task — `tcc fire all` executes all steps in parallel | [hmzainjamil](https://github.com/hmzainjamil) |
| Use `mae deploy <workflow-name>` to run a specific workflow JSON end-to-end | [hmzainjamil](https://github.com/hmzainjamil) |
| `grep -i "topic" ~/installed-repos/n8nworkflows.xyz/workflow_index.txt` finds relevant n8n flows | [n8nworkflows.xyz](https://n8nworkflows.xyz) |

<a id="tips-debug"></a>
■ **Debugging (4)**

| Tip | Source |
|---|---|
| `tail -f ~/Library/Logs/ai.hmz.mae-daily.log` watches LaunchAgent stdout in real-time | [hmzainjamil](https://github.com/hmzainjamil) |
| `launchctl unload && launchctl load` to reload a plist after editing — no restart needed | [Apple](https://developer.apple.com) |
| `mae status` checks all 12 LaunchAgents, Tier 0 LLMs, and Paperclip in one command | [hmzainjamil](https://github.com/hmzainjamil) |
| Test plist in shell first: `bash -c "$(ProgramArguments from plist)"` before loading | [hmzainjamil](https://github.com/hmzainjamil) |

---

## ☠️ STARTUPS / BUSINESSES <a id="startups"></a>

| Feature | Replaced |
|---|---|
| **12 always-on LaunchAgents** | [Zapier](https://zapier.com), [Make.com](https://make.com), [Pipedream](https://pipedream.com) |
| **Daily MAE ops automation** | [Monday.com AI](https://monday.com), [ClickUp Automations](https://clickup.com) |
| **Persistent Ollama LLM server** | [Replicate](https://replicate.com), [Together AI](https://together.ai), [Fireworks AI](https://fireworks.ai) |
| **n8n workflow library (8,159)** | [Zapier Templates](https://zapier.com/templates), [Make.com templates](https://make.com) |
| **Cache auto-cleanup** | [CleanMyMac X](https://cleanmymac.com), [Disk Diag](https://diskdiag.com) |

---

## Star History <a id="star"></a>

[![Star History Chart](https://api.star-history.com/svg?repos=hmzainjamil/claude-ai-automations&type=Date)](https://star-history.com/#hmzainjamil/claude-ai-automations&Date)
