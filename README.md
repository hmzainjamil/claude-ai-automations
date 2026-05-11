# claude-ai-automations
Production LaunchAgent daemons + Python automation pipelines running DigiMinds 24/7 — lead gen, content, KPI monitoring, GitHub sync, competitor intel.

![daemons](https://img.shields.io/badge/LaunchAgents-8_always_on-blue?style=flat&labelColor=555) ![scheduled](https://img.shields.io/badge/scheduled_engines-6-green?style=flat&labelColor=555) ![python](https://img.shields.io/badge/python_scripts-12-orange?style=flat&labelColor=555) ![company](https://img.shields.io/badge/DigiMinds-agency-red?style=flat&labelColor=555)

[Concepts](#-concepts) · [Hot](#-hot) · [Schedule](#️-schedule) · [Tips](#-tips-and-tricks-22) · [Replaced](#️-startups--businesses) · [Stars](#star-history)

---

## 🧠 CONCEPTS

| Feature | Location | Description |
|---------|----------|-------------|
| [**LaunchAgent Daemons**](launchagents/) | `~/Library/LaunchAgents/ai.hmz.*.plist` | 8 always-on macOS daemons — KeepAlive=true auto-restarts within 30s of crash |
| [**GitHub Portfolio Sync**](launchagents/ai.hmz.github-portfolio-sync.plist) | `ai.hmz.github-portfolio-sync.plist` | Daily 6:30 AM — syncs all 45 bin scripts, skills, agents, LaunchAgents to GitHub |
| [**OpenClaw Gateway**](launchagents/ai.openclaw.gateway.plist) | `ai.openclaw.gateway.plist` | Always-on bridge: Composio + MCP + Paperclip — persistent session state |
| [**Paperclip CEO**](launchagents/ai.hmz.paperclip.plist) | `ai.hmz.paperclip.plist` | CEO loop daemon at port 3100 — RunAtLoad=true, KeepAlive=true |
| [**Lead Engine**](launchagents/ai.hmz.paperclip-lead-engine.plist) | `ai.hmz.paperclip-lead-engine.plist` | Daily 7:30 AM — Apollo+LinkedIn scrape → enrich → ICP score → CRM |
| [**Content Engine**](launchagents/ai.hmz.paperclip-content-engine.plist) | `ai.hmz.paperclip-content-engine.plist` | Daily 8:00 AM — trends → angle → generate → quality gate → LinkedIn schedule |
| [**KPI Monitor**](launchagents/ai.hmz.paperclip-kpi-monitor.plist) | `ai.hmz.paperclip-kpi-monitor.plist` | Daily 6:00 PM — 28 KPI health checks, threshold alerts, CEO escalation |
| [**Trends Scanner**](launchagents/ai.hmz.paperclip-trends-scanner.plist) | `ai.hmz.paperclip-trends-scanner.plist` | Mon/Wed/Fri 6 AM — platform changes, market signals, macro trends |
| [**Python Scripts**](scripts/) | `scripts/` | `digiminds_audit.py` `create_ads.py` `populate_ads.py` `per-prospect-pdf.py` — offline pipeline tools |

### 🔥 Hot

| Feature | Location | Description |
|---------|----------|-------------|
| [**auto-github-push**](../claude-ai-system/automations/bin/auto-github-push) | PostToolUse hook | Every Write/Edit to `~/.claude/bin/` auto-pushes to GitHub — replaces manual github-sync for new scripts |
| [**KeepAlive + RunAtLoad**](launchagents/) | All `.plist` files | Both flags required — KeepAlive alone doesn't start on boot, RunAtLoad alone doesn't restart on crash |
| [**Token scrubbing**](../claude-ai-system/automations/bin/github-sync) | `github-sync` | Strips `ghp_*` `sk-*` `pat*` tokens from LaunchAgent plists before GitHub commit — automatic OPSEC |

---

## ⚙️ SCHEDULE

| Daemon | Schedule | Runtime | Output |
|--------|----------|---------|--------|
| GitHub Portfolio Sync | Daily 6:30 AM | ~3 min | Updated claude-ai-system repo |
| Paperclip CEO Loop | Every 6h | ~2 min | Decisions log, task assignments |
| Lead Engine | Daily 7:30 AM | ~6 min | 10-30 ICP-scored leads in CRM |
| Content Engine | Daily 8:00 AM | ~3 min | LinkedIn post scheduled 10 AM |
| Intel Engine | Daily 10:00 AM | ~5 min | Competitor intel brief |
| KPI Monitor | Daily 6:00 PM | ~2 min | 28-KPI scorecard, alerts |
| Trends Scanner | Mon/Wed/Fri 6 AM | ~4 min | Trends brief → CEO context |
| OpenClaw Gateway | Always-on | — | Composio + MCP bridge |

---

## 💡 TIPS AND TRICKS (22)

[LaunchAgent](#tips-la) · [Scheduling](#tips-sched) · [Scripts](#tips-scripts) · [Pipelines](#tips-pipe) · [Debug](#tips-debug)

<a id="tips-la"></a>■ **LaunchAgent Design (6)**

| Tip | Source |
|-----|--------|
| Always use both `KeepAlive=true` AND `RunAtLoad=true` — never one without the other | [macOS SOP](launchagents/) |
| `StartCalendarInterval` is more reliable than cron on macOS — survives sleep/wake | [macOS docs](launchagents/) |
| Set `StandardOutPath` and `StandardErrorPath` — silent failures are the worst failures | [Ops rule](launchagents/) |
| `EnvironmentVariables` in plist must include `HOME` + `PATH` — LaunchAgents don't inherit shell env | [Common gotcha](launchagents/) |
| Scrub tokens from plist before github-sync commits — use `sed` on `GITHUB_TOKEN` patterns | [OPSEC](../claude-ai-system/automations/bin/github-sync) |
| `launchctl list \| grep ai.hmz` — verify all 8 daemons running at every session start | [auto-troubleshoot](../claude-ai-system/automations/bin/auto-troubleshoot) |

<a id="tips-sched"></a>■ **Scheduling (4)**

| Tip | Source |
|-----|--------|
| Lead engine at 7:30 AM not 7:00 AM — gives CEO loop (6h, starting midnight) time to finish | [Schedule logic](launchagents/) |
| Content engine at 8:00 AM publishes at 10 AM — 2h buffer for quality gate failures | [Schedule logic](launchagents/) |
| Trends scanner Mon/Wed/Fri only — daily would waste Apify credits on low-signal days | [Cost optimization](launchagents/) |
| KPI monitor at 6 PM — after business hours data is fully populated for the day | [Data logic](launchagents/) |

<a id="tips-scripts"></a>■ **Python Scripts (5)**

| Tip | Source |
|-----|--------|
| All scripts use `set -e` or Python `sys.exit(1)` on error — LaunchAgent catches exit code | [Error handling](scripts/) |
| Save all PDF outputs to `~/Downloads/` — never Desktop | [HMZ rule](scripts/) |
| ReportLab PDFs: always `save_document()` before any export step | [reportlab-pdf-master](../claude-ai-skills/) |
| Use Apify actors for all scraping inside scripts — never raw requests | [Tool routing](scripts/) |
| Add `time.sleep(1)` between Apollo API calls — bulk operations hit rate limits silently | [Apollo gotcha](scripts/) |

<a id="tips-pipe"></a>■ **Pipelines (4)**

| Tip | Source |
|-----|--------|
| Every pipeline step must be idempotent — safe to re-run without creating duplicates | [Design principle](scripts/) |
| Log every step: `echo "[engine] step completed at $(date)" >> ~/Library/Logs/x.log` | [Ops rule](scripts/) |
| Paperclip API at port 3100 = pipeline state store — don't use files for intermediate state | [Architecture](../hmz-digiminds-ceo/) |
| Tier 0 models for all LLM calls inside pipelines — Groq for fast, Gemini for analysis | [G0DM0D3 routing](../hmz-g0dm0d3/) |

<a id="tips-debug"></a>■ **Debug (3)**

| Tip | Source |
|-----|--------|
| LaunchAgent exit=256 = script returned error — check `~/Library/Logs/<name>-error.log` | [Debug SOP](../claude-ai-system/automations/bin/auto-troubleshoot) |
| `launchctl start ai.hmz.<name>` to test-trigger without waiting for schedule | [Debug](launchagents/) |
| Mutex lock for long scripts: `[ -f /tmp/script.lock ] && exit 0; touch /tmp/script.lock` | [Concurrency](scripts/) |

---

## ☠️ STARTUPS / BUSINESSES

| Feature | Replaced |
|-|-|
| **macOS LaunchAgent daemons** | [n8n](https://n8n.io), [Zapier](https://zapier.com), [Make](https://make.com) — cloud-only, not local-first, monthly fees |
| **Lead engine (daily 7:30 AM)** | [Clay](https://clay.com), [Instantly](https://instantly.ai) — manual trigger, per-lead pricing |
| **Content engine (daily 8 AM)** | [Buffer](https://buffer.com), [Hootsuite](https://hootsuite.com), [Taplio](https://taplio.com) — scheduling only, no AI generation |
| **Competitor intel (daily 10 AM)** | [Crayon](https://crayon.co), [Klue](https://klue.com) — $500+/mo, no ad library integration |
| **KPI monitor (daily 6 PM)** | [Databox](https://databox.com), [Klipfolio](https://klipfolio.com) — passive dashboards, no escalation |
| **GitHub auto-sync** | Manual `git push`, [GitHub Desktop](https://desktop.github.com) |

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hmzainjamil/claude-ai-automations&type=Date)](https://star-history.com/#hmzainjamil/claude-ai-automations&Date)