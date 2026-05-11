# claude-ai-automations
production automation stack — runs 24/7, no human operator required

![Stack](https://img.shields.io/badge/stack-LaunchAgent_%7C_n8n_%7C_Python_%7C_cron-blue?style=flat&labelColor=000) ![LaunchAgents](https://img.shields.io/badge/LaunchAgents-5_permanent-orange?style=flat&labelColor=555) ![Scheduled](https://img.shields.io/badge/scheduled_agents-6_autonomous-green?style=flat&labelColor=555) ![n8n](https://img.shields.io/badge/n8n-self--hosted-red?style=flat&labelColor=555)

DigiMinds full automation stack — LaunchAgents that never stop, Claude scheduled tasks that run even when the machine is off, n8n workflows for API integrations, and Python scripts for data operations. Everything from lead generation to client invoicing to competitor monitoring runs without HMZ touching a keyboard.

[LaunchAgents](#launchagents) · [Scheduled Agents](#scheduled) · [n8n Workflows](#n8n) · [Python Scripts](#python) · [Health Monitoring](#health) · [Tips](#tips) · [Gotchas](#gotchas)

## 🧠 AUTOMATION LAYERS

| Layer | Technology | Always-on | Trigger | Failure Mode |
|---|---|---|---|---|
| System Services | macOS LaunchAgent | Yes | Boot/crash restart | Auto-restart via KeepAlive |
| AI Agents | Claude Scheduled Tasks | Yes (cloud) | Cron schedule | Anthropic infrastructure |
| API Integrations | n8n (self-hosted) | Yes | Webhook/cron | n8n error log + Slack alert |
| Batch Processing | Python scripts | No | Manual / cron | Exit code + log |
| Data Extraction | Apify Actors | On demand | API call | Actor run log |

<a id="launchagents"></a>
## ⚙️ LAUNCHAGENTS (Always-on system services)

| LaunchAgent | Port | Working Dir | Auto-Restart |
|---|---|---|---|
| `ai.hmz.paperclip` | 3100 | `~/installed-repos/paperclip` | Yes (KeepAlive) |
| `ai.openclaw.gateway` | 51827 | `~/installed-repos/openclaw` | Yes (KeepAlive) |
| `ai.hmz.ollama` | 11434 | system | Yes (KeepAlive) |
| `ai.hmz.github-portfolio-sync` | — | `~/` | No (daily cron) |
| `ai.hmz.open-design` | 51827 | `~/installed-repos/open-design` | Yes (KeepAlive) |

**Check all services:**
```bash
launchctl list | grep ai.hmz
# Format: PID  ExitCode  Label
# PID = number → running
# PID = "-"    → stopped
# ExitCode > 0 → crashed
```

**Restart a crashed service:**
```bash
launchctl stop ai.hmz.paperclip
launchctl start ai.hmz.paperclip
# OR force reload:
launchctl unload ~/Library/LaunchAgents/ai.hmz.paperclip.plist
launchctl load ~/Library/LaunchAgents/ai.hmz.paperclip.plist
```

**Paperclip plist key settings:**
```xml
<key>KeepAlive</key><true/>           ← auto-restart on crash
<key>RunAtLoad</key><true/>           ← start on boot
<key>ThrottleInterval</key><integer>5</integer>  ← min 5s between restarts
<key>StandardOutPath</key>
  <string>~/.paperclip/instances/default/logs/server.log</string>
<key>StandardErrorPath</key>
  <string>~/.paperclip/instances/default/logs/server-error.log</string>
```

<a id="scheduled"></a>
## 💡 CLAUDE SCHEDULED AGENTS

Six agents run on Anthropic infrastructure — no machine required:

■ **CEO Strategy Loop** (every 6h)
```
Schedule: 0 */6 * * *
Action:   Pull company state from Paperclip API
          Scan Groq/Gemini for market intel (Tier 0)
          Create 3+ priority tasks for the day
          Hire new agent if skill gap detected
          Log decisions to ~/.paperclip/ceo-decisions.log
Output:   3+ tasks in Paperclip, decision log entry
```

■ **Lead Enrichment Engine** (7:30 AM daily)
```
Schedule: 30 7 * * *
Action:   Source 10+ leads from LinkedIn/Reddit/Indeed signals
          Score each 0-100 (budget + fit + timing + decision-maker)
          Enrich top 10 via Apollo/web search
          Create Paperclip BDM tasks for high-score leads
          Write outreach drafts → ~/Downloads/outreach-{date}.txt
Output:   10+ enriched leads, BDM tasks, outreach file
```

■ **LinkedIn Content Engine** (8:00 AM daily)
```
Schedule: 0 8 * * *
Action:   Find today's trending topic in digital marketing/AI
          Generate LinkedIn post (hook + value + CTA structure)
          Format for 1,300-char optimal length
          Save → ~/Downloads/linkedin-post-{date}.txt
Output:   Ready-to-post LinkedIn content
```

■ **Competitor Intel Engine** (10:00 AM daily)
```
Schedule: 0 10 * * *
Action:   Monitor top 5 competitor websites for pricing/service changes
          Track their job postings (signals for expansion)
          Generate 3 strategic recommendations
          Log to Paperclip AI Automation project
Output:   Intel report + 3 strategic tasks
```

■ **KPI Health Monitor** (6:00 PM daily)
```
Schedule: 0 18 * * *
Action:   Pull all KPI metrics from Paperclip
          Compare to targets (leads, revenue, ROAS, content)
          Auto-create corrective tasks for any KPI miss
          Generate daily CEO summary
          Save → ~/Downloads/paperclip-daily-summary-{date}.txt
Output:   Corrective tasks + daily summary file
```

■ **Market Trends Scanner** (Mon/Wed/Fri 6AM)
```
Schedule: 0 6 * * 1,3,5
Action:   Scan 8 topics: AI tools, PPC changes, content trends,
          platform algorithm updates, competitor funding, hiring trends,
          regulation changes, client industry news
          Create opportunity tasks for actionable signals
          Auto-add new Goals for major revenue opportunities
Output:   8 trend briefs + opportunity tasks
```

<a id="n8n"></a>
## 🔧 N8N WORKFLOW INVENTORY

| Workflow | Trigger | Integration Chain | Output |
|---|---|---|---|
| Lead Intake → CRM | Webhook (Typeform) | Typeform → Apollo enrich → HubSpot create → Slack notify | Contact in HubSpot + Slack alert |
| Weekly Client Report | Cron (Mon 9AM) | GA4 → Google Ads → ReportLab PDF → Gmail → Notion log | PDF email + Notion entry |
| Invoice Processor | Email trigger | Gmail → Stripe lookup → QuickBooks → Slack | Categorized invoice + payment status |
| Content Publisher | Paperclip task | Draft file → LinkedIn API → Twitter/X → tracking | Posts published + engagement logged |
| Competitor Price Monitor | Daily cron | Web scrape → compare prev → Notion update → Slack if changed | Price history + change alerts |
| Lead Score Updater | Daily cron | HubSpot → score recalc → priority tags update | Updated CRM pipeline |

<a id="python"></a>
## 🐍 PYTHON AUTOMATION SCRIPTS

| Script | Location | Runtime | Purpose |
|---|---|---|---|
| `digiminds_audit_v3_pdf.py` | `~/` | ~45s | 360° client audit → 11-page ReportLab PDF |
| `create_ads_from_scratch.py` | `~/` | ~30s | Google + Meta ad creation from brief |
| `populate_ads.py` | `~/` | ~20s | Bulk ad population from XLSX |
| `oztechwork_audit.py` | `~/` | ~60s | Per-client audit runner (oztechwork) |
| `saifee_audit.py` | `~/` | ~60s | Per-client audit runner (saifee) |
| `digiminds_audit.py` | `~/` | ~30s | Core audit logic |

<a id="health"></a>
## 📊 HEALTH MONITORING

**Auto-troubleshoot hook** (fires every session via UserPromptSubmit):
```bash
~/.claude/bin/auto-troubleshoot
# Checks: all LaunchAgent exit codes
# Reports: any with exit > 0 → tells Claude to investigate
```

**Manual health check:**
```bash
# All LaunchAgents
launchctl list | grep ai.hmz | awk '{print $1, $2, $3}'
# PID ExitCode Label — exit 256 = crash

# Paperclip API
curl -s http://127.0.0.1:3100/api/health | jq .

# Ollama
curl -s http://localhost:11434/api/tags | jq '.models[].name'

# OpenClaw
curl -s http://127.0.0.1:51827/health

# Scheduled tasks (via Claude)
# /schedule list
```

**Log locations:**
```bash
~/.paperclip/instances/default/logs/server.log       # Paperclip stdout
~/.paperclip/instances/default/logs/server-error.log # Paperclip stderr
~/.paperclip/ceo-decisions.log                       # CEO loop decisions
~/Library/Logs/                                       # LaunchAgent system logs
```

<a id="tips"></a>
## 💡 TIPS

| Tip | Note |
|---|---|
| Always use `KeepAlive` + `ThrottleInterval` together — KeepAlive without throttle causes rapid crash loops on config errors | ThrottleInterval=5 minimum |
| Scheduled agents should write state to disk after each run — they start fresh each time, no memory between runs | `~/Downloads/agent-state-{date}.json` |
| n8n webhook nodes need `https` for external triggers — local `http` only works for internal calls | Use ngrok tunnel or Vercel Edge Function |
| Python scripts in `~/` are NOT auto-uploaded to GitHub by github-sync — explicitly add to tracked list | Edit `github-sync` script |
| If a LaunchAgent exits with code 256, check the plist WorkingDirectory path — most 256 exits are "directory not found" | |

## ☠️ GOTCHAS

| Gotcha | Fix |
|---|---|
| `launchctl load` succeeds even with invalid plist — errors only appear when the service tries to start | Run `plutil -lint ~/Library/LaunchAgents/<plist>` before loading |
| Scheduled tasks timeout at 5min — network-heavy tasks (web scraping, API calls) fail silently | Break into sub-tasks or use Apify actors for scraping |
| n8n workflows lose Slack OAuth on token expiry (~60 days) — all workflows using Slack fail silently | Add token refresh monitoring in n8n health workflow |
| CEO Loop creates tasks with no `projectId` when Paperclip API is slow to respond | Add 3s retry before creating tasks |
| github-sync uploads `settings.local.json` which may contain device-specific paths — breaks on other machines | Add settings.local.json to .gitignore |
