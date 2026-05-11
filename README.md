# claude-ai-automations

![Automations](https://img.shields.io/badge/DigiMinds-Production_Automations-2ECC71?style=flat&labelColor=000) ![Stack](https://img.shields.io/badge/stack-n8n_%7C_LaunchAgent_%7C_Python_%7C_cron-blue?style=flat&labelColor=555) ![Status](https://img.shields.io/badge/status-24%2F7_running-green?style=flat&labelColor=555) ![Agents](https://img.shields.io/badge/agents-6_autonomous-orange?style=flat&labelColor=555)

Production automation stack powering DigiMinds (digiminds.org) — n8n workflows, macOS LaunchAgents, Python scripts, and Claude scheduled tasks running 24/7 without human intervention. Everything from lead generation to client reporting to CEO strategy loops.

## 🧠 AUTOMATION LAYERS

| Layer | Technology | Always-on? | Purpose |
|---|---|---|---|
| LaunchAgents | macOS plist | Yes | System-level persistent processes |
| Claude Scheduled Tasks | Remote cron | Yes | AI agents running even when machine is off |
| n8n Workflows | Self-hosted n8n | Yes | API integrations, data pipelines |
| Python Scripts | cron / manual | No | One-off batch jobs, data exports |
| Apify Actors | Cloud | On demand | Web scraping, data extraction |

## ⚙️ ALWAYS-ON LAUNCHAGENTS

```
~/Library/LaunchAgents/
├── ai.hmz.paperclip.plist         ← Paperclip CEO control plane (port 3100)
├── ai.openclaw.gateway.plist      ← OpenClaw MCP gateway (port 51827)
├── ai.hmz.ollama.plist            ← Local LLM server (Llama 3, GPU)
├── ai.hmz.github-portfolio-sync.plist  ← Daily repo sync to GitHub
└── ai.hmz.open-design.plist       ← Open Design server (port 51827)
```

**Check all LaunchAgent status:**
```bash
launchctl list | grep ai.hmz
# PID shows running; "-" means stopped
```

## 💡 CLAUDE AUTONOMOUS AGENTS (Remote Scheduled)

Six agents running on schedule via `mcp__scheduled-tasks`:

| Agent | cron | Output |
|---|---|---|
| `paperclip-ceo-loop` | `0 */6 * * *` | 3+ tasks/session, agent hiring decisions, decisions.log |
| `paperclip-lead-engine` | `30 7 * * *` | 10+ enriched leads, outreach drafts → ~/Downloads/ |
| `paperclip-content-engine` | `0 8 * * *` | LinkedIn post draft → ~/Downloads/ |
| `paperclip-intel-engine` | `0 10 * * *` | 3 competitor insights → Paperclip AI Automation project |
| `paperclip-kpi-monitor` | `0 18 * * *` | KPI audit + corrective tasks + daily summary |
| `paperclip-trends-scanner` | `0 6 * * 1,3,5` | 8 trend signals + opportunity tasks |

## 🔧 N8N WORKFLOW INVENTORY

| Workflow | Trigger | Integration Chain |
|---|---|---|
| Lead Intake → CRM | Webhook | Typeform → Apollo → HubSpot → Slack notification |
| Client Report Generator | Weekly cron | GA4 + Google Ads → ReportLab PDF → Gmail |
| Invoice Processor | Email trigger | Gmail → Stripe lookup → QuickBooks → Slack |
| Content Publisher | Paperclip task | Draft file → LinkedIn + Twitter/X → engagement tracking |
| Competitor Price Monitor | Daily cron | Web scrape → compare → Notion update → alert if change |

## 🐍 PYTHON AUTOMATION SCRIPTS

| Script | Location | Purpose |
|---|---|---|
| `digiminds_audit_v3_pdf.py` | `~/` | 360° client audit → ReportLab 11-page PDF |
| `create_ads_from_scratch.py` | `~/` | Google/Meta ad creation from brief |
| `populate_ads.py` | `~/` | Bulk ad population from spreadsheet |
| `oztechwork_audit.py` | `~/` | Per-client audit runner |

## ☠️ AUTOMATION FAILURE HANDLING

All automations write to structured logs:
```bash
~/.paperclip/instances/default/logs/server.log   # Paperclip
~/.paperclip/ceo-decisions.log                   # CEO loop decisions
~/Library/Logs/ai.hmz.*/                         # All LaunchAgent logs
```

Auto-troubleshoot hook fires every session:
```bash
~/.claude/hooks/UserPromptSubmit/auto-troubleshoot.sh
# → checks all LaunchAgent exit codes
# → reports any with exit > 0 to Claude for investigation
```
