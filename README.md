# claude-ai-automations
Production automation stack — n8n workflows, LaunchAgents, Python scripts, and scheduled Claude tasks

![Automations](https://img.shields.io/badge/automations-active-brightgreen?style=flat&labelColor=555) ![n8n](https://img.shields.io/badge/n8n-integrated-red?style=flat&labelColor=555) ![LaunchAgents](https://img.shields.io/badge/LaunchAgents-macOS-blue?style=flat&labelColor=555)

Part of the [HMZ AI Infrastructure](https://github.com/hmzainjamil) stack — DigiMinds agency automation layer.

---

## 🧠 AUTOMATION LAYERS

| Layer | Technology | Purpose |
|-------|-----------|---------|
| System | macOS LaunchAgents | Always-on services, auto-restart on crash |
| Orchestration | Paperclip AI | Agent task routing, KPI monitoring |
| Workflows | n8n | Multi-step integrations, API chaining |
| Scripting | Python + bash | Data processing, reporting, audits |
| Scheduling | Claude Scheduled Tasks | AI-powered recurring agents |

## ⚙️ ALWAYS-ON SERVICES

| LaunchAgent | Purpose | Restart Policy |
|-------------|---------|----------------|
| `ai.hmz.paperclip` | Agency CEO + task orchestration | KeepAlive=true |
| `ai.openclaw.gateway` | OpenClaw API gateway | KeepAlive=true |
| `ollama` | Local LLM inference | KeepAlive=true |

## 💡 AUTOMATION SOPS

■ **BDM Lead Generation**
- 50 prospects researched/day via Apollo + LinkedIn
- 1,000 cold emails/week via Instantly (automated sequences)
- 10+ discovery calls booked/week target

■ **Paid Media Campaign Launch**
- Pre-launch QA checklist: 100% required
- GA4 + GTM + Pixel tracking verified before any launch
- 3 creative variants minimum per ad set

■ **Reporting**
- Monthly client reports auto-generated end of month
- KPI dashboard live at Paperclip API: `http://127.0.0.1:3100`
- Daily CEO summary saved to `~/Downloads/paperclip-daily-summary-{date}.txt`

---
Built by [HMZ](https://github.com/hmzainjamil) · [DigiMinds](https://digiminds.org)
