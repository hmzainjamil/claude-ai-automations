# claude-ai-automations

![Version](https://img.shields.io/badge/version-2.0-blue?style=flat&labelColor=555)
![Workflows](https://img.shields.io/badge/n8n_workflows-8159-purple?style=flat&labelColor=555)
![Status](https://img.shields.io/badge/status-active-brightgreen?style=flat&labelColor=555)
![License](https://img.shields.io/badge/license-MIT-orange?style=flat&labelColor=555)

> **8,159 n8n workflows + MAE automation engine** — every business process automated. Email, CRM, social, Slack, Shopify, data, reporting, lead gen, and more. Zero-human ops layer.

---

## 🧠 CONCEPTS

| Feature | Location | Description |
|---|---|---|
| [n8n Workflows](workflows/) | `workflows/` | 8,159 JSON workflow files covering every automation category |
| [Workflow Index](workflow_index.txt) | `workflow_index.txt` | Full-text searchable index of all 8,159 workflows |
| [MAE Engine](bin/mae) | `bin/mae` | Master Automation Engine — 12-agent swarm + synthesis |
| [TCC Queue](bin/tcc) | `bin/tcc` | Task queue — add/fire/list/retry/purge/blast tasks |
| [DigiMinds Daily](workflows/digiminds-daily.json) | `workflows/digiminds-daily.json` | Full daily ops workflow — all divisions run in sequence |
| [Email Automation](workflows/email/) | `workflows/email/` | 874 Gmail/email automation workflows |
| [Slack Automation](workflows/slack/) | `workflows/slack/` | 328 Slack notification and bot workflows |
| [Social Automation](workflows/social/) | `workflows/social/` | 197 Instagram/Twitter/LinkedIn/TikTok workflows |
| [Lead Gen Automation](workflows/leads/) | `workflows/leads/` | 121 lead flows — Apollo, Hunter, LinkedIn scraping |
| [Shopify Automation](workflows/shopify/) | `workflows/shopify/` | 82 Shopify/WooCommerce order and inventory workflows |
| [Telegram Automation](workflows/telegram/) | `workflows/telegram/` | 309 Telegram bot and notification workflows |
| [Data Automation](workflows/data/) | `workflows/data/` | Sheets, Airtable, NocoDB, Notion sync pipelines |
| [CRM Automation](workflows/crm/) | `workflows/crm/) | HubSpot, Salesforce, Pipedrive sync workflows |
| [Codex Agents](bin/codex-agents/) | `bin/codex-agents/` | 10 Codex-style agent scripts for full SaaS/ops automation |
| [Agent Registry](agents/registry.json) | `agents/registry.json` | 18 registered automation agents with routing rules |
| [n8n Routes](tcc-routes/routes.json) | `tcc-routes/routes.json` | Task → automation agent routing map |
| [LaunchAgent Plists](launchd/) | `launchd/` | macOS LaunchAgent configs for always-on automations |
| [Webhook Triggers](webhooks/) | `webhooks/` | Inbound webhook handlers for external system triggers |
| [Cron Jobs](cron/) | `cron/` | Scheduled automation configs — hourly, daily, weekly |
| [AI Creative Pipeline](bin/ads-creative/) | `bin/ads-creative/` | uni1-image-ad + arcads + kie-ai + meta-ads-spy stack |
| [OpenCLI Adapters](bin/opencli/) | `bin/opencli/` | 90+ site adapters via OpenCLI — zero LLM cost per call |
| [Composio Actions](bin/composio/) | `bin/composio/` | 3000+ tool actions for any SaaS via Composio |
| [Paperclip Sync](bin/paperclip-sync.sh) | `bin/paperclip-sync.sh` | Auto-saves every automation run to Paperclip company OS |
| [MAE Status](bin/tcc-dashboard) | `bin/tcc-dashboard` | Full system status: queue, RAM, models, last run time |
| [Auto-Push Hook](hooks/auto-github-push) | `hooks/auto-github-push` | PostToolUse hook — new bin/ files auto-synced to GitHub |

### 🔥 Hot

| Feature | Location | Description |
|---|---|---|
| [MAE Daily Ops](workflows/digiminds-daily.json) | `workflows/digiminds-daily.json` | One command = full company runs itself for the day |
| [8159 n8n Workflows](workflow_index.txt) | `workflow_index.txt` | Every business process already automated — just search and deploy |
| [Codex Agent Stack](bin/codex-agents/) | `bin/codex-agents/` | 10 agents: SaaS builder, lead gen, ops, support, content, bug fixer |
| [OpenCLI 90+ Adapters](bin/opencli/) | `bin/opencli/` | Browse any site without LLM — GitHub, LinkedIn, Notion, Jira, more |
| [Composio 3000+ Actions](bin/composio/) | `bin/composio/` | Any SaaS tool action via single API — no per-integration setup |

---

## ⚙️ ARCHITECTURE

```
┌───────────────────────────────────────────────────────────────────┐
│                 CLAUDE-AI-AUTOMATIONS v2.0                        │
│                                                                   │
│  Trigger → TCC queue → MAE engine → agent swarm → output          │
│                                                                   │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐      │
│  │  Webhook │   │  Cron    │   │  Hook    │   │  Manual  │      │
│  │ triggers │   │  jobs    │   │  events  │   │  mae run │      │
│  └────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘      │
│       └──────────────┴──────────────┴──────────────┘             │
│                              │                                    │
│                    ┌─────────▼─────────┐                         │
│                    │    TCC QUEUE      │                         │
│                    │  task routing     │                         │
│                    └─────────┬─────────┘                         │
│                              │                                    │
│              ┌───────────────▼────────────────┐                  │
│              │       MAE SWARM (12 agents)     │                  │
│              │  Groq + Gemini + DeepSeek blast │                  │
│              └───────────────┬────────────────┘                  │
│                              │                                    │
│              8159 n8n workflows · OpenCLI · Composio              │
│              Paperclip sync · GitHub push · Slack notify          │
└───────────────────────────────────────────────────────────────────┘
```

| Layer | Technology | Count |
|---|---|---|
| Workflow library | n8n JSON workflows | 8,159 |
| Site adapters | OpenCLI | 90+ |
| SaaS actions | Composio | 3,000+ |
| Automation agents | MAE registry | 18 |
| Scheduled jobs | LaunchAgent + cron | Always-on |

---

## 🚀 Quick Start

```bash
# Run full DigiMinds daily ops
mae daily

# Search for a workflow
grep -i 'shopify' ~/installed-repos/n8nworkflows.xyz/workflow_index.txt

# Fire 3 parallel tasks
tcc blast "send weekly report" "update CRM" "post LinkedIn update"

# Deploy a workflow to n8n
curl -X POST http://localhost:5678/api/v1/workflows -d @workflows/my-flow.json

# Check system status
tcc-dashboard
```

---

## 💡 TIPS AND TRICKS (48)

<a id="tips-n8n"></a>
### ■ **n8n Workflows (6)**
| Tip | Source |
|---|---|
| 8159 workflows in index — grep before building anything new | [workflow_index.txt](https://github.com/hmzainjamil/claude-ai-automations) |
| Import workflow: n8n UI → Workflows → Import from File → paste JSON | [n8n docs](https://docs.n8n.io) |
| Webhook trigger node = instant external integration with any tool | [n8n](https://n8n.io) |
| Use Schedule trigger for cron-style daily/hourly automations | [n8n](https://n8n.io) |
| Error workflow: connect all nodes → send Slack alert on failure | [n8n](https://n8n.io) |
| Split In Batches node: process 1000s of records without memory issues | [n8n](https://n8n.io) |

<a id="tips-mae"></a>
### ■ **MAE Engine (6)**
| Tip | Source |
|---|---|
| `mae run "goal"` = 12 agents + synthesis in ~8s — default for all tasks | [mae](https://github.com/hmzainjamil/claude-ai-system) |
| `mae plan "goal"` = preview decomposition without executing | [mae](https://github.com/hmzainjamil/claude-ai-system) |
| `mae daily` = full DigiMinds ops — all divisions automated | [mae](https://github.com/hmzainjamil/claude-ai-system) |
| `mae status` = see running agents, queue depth, last synthesis | [mae](https://github.com/hmzainjamil/claude-ai-system) |
| Wave batching = cloud APIs first, local Ollama only if RAM > 2GB | [CLAUDE.md](https://github.com/hmzainjamil/claude-ai-system) |
| All MAE outputs auto-saved to ~/.claude/tcc-logs/ as timestamped MD | [tcc-logs](https://github.com/hmzainjamil/claude-ai-system) |

<a id="tips-codex"></a>
### ■ **Codex Agents (6)**
| Tip | Source |
|---|---|
| codex-saas-builder: architecture brief → full SaaS MVP with tests | [codex-agents](https://github.com/hmzainjamil/claude-ai-automations) |
| codex-lead-gen: ICP → scrape → enrich → email sequence → sent | [codex-agents](https://github.com/hmzainjamil/claude-ai-automations) |
| codex-bug-fixer: bug report → root cause → fix → tests → PR | [codex-agents](https://github.com/hmzainjamil/claude-ai-automations) |
| codex-content-engine: topic → reel + LinkedIn + YouTube + SEO | [codex-agents](https://github.com/hmzainjamil/claude-ai-automations) |
| codex-support-agent: knowledge base → AI support → escalation | [codex-agents](https://github.com/hmzainjamil/claude-ai-automations) |
| Codex loop: understand → plan → execute → test → observe → fix | [Codex](https://openai.com/codex) |

<a id="tips-opencli"></a>
### ■ **OpenCLI (6)**
| Tip | Source |
|---|---|
| 90+ site adapters — GitHub, LinkedIn, Notion, Jira, Figma, more | [OpenCLI](https://github.com/jackwener/opencli) |
| Zero LLM cost — Chrome session + adapter, no API calls | [OpenCLI](https://github.com/jackwener/opencli) |
| `opencli github list-prs --repo owner/repo` — list open PRs | [OpenCLI](https://github.com/jackwener/opencli) |
| `opencli linkedin search --query "SaaS founder"` — lead scraping | [OpenCLI](https://github.com/jackwener/opencli) |
| Persistent Chrome session — never re-logs in between calls | [OpenCLI](https://github.com/jackwener/opencli) |
| Version 1.7.18 installed at /Users/mc/.nvm/versions/node/v24.14.1/bin/ | [npm](https://npmjs.com) |

<a id="tips-composio"></a>
### ■ **Composio (6)**
| Tip | Source |
|---|---|
| 3000+ tool actions — Salesforce, HubSpot, Notion, Linear, GitHub | [Composio](https://composio.dev) |
| Single API for all tools — one auth flow, consistent response format | [Composio](https://composio.dev) |
| Combine Composio + MAE = full business automation in one command | [Composio](https://composio.dev) |
| composio-openclaw repo wires Composio actions into OpenCLI flow | [hmz-composio-openclaw](https://github.com/hmzainjamil/hmz-composio-openclaw) |
| Tool actions execute server-side — no browser needed | [Composio](https://composio.dev) |
| Built-in auth: OAuth, API key, basic — handles token refresh auto | [Composio](https://composio.dev) |

<a id="tips-launchd"></a>
### ■ **LaunchAgents (6)**
| Tip | Source |
|---|---|
| KeepAlive=true + RunAtLoad=true = always-on service on Mac | [macOS docs](https://developer.apple.com) |
| Set HOME + PATH in EnvironmentVariables — scripts find all tools | [launchd](https://developer.apple.com) |
| Load: `launchctl load ~/Library/LaunchAgents/com.xxx.plist` | [launchd](https://developer.apple.com) |
| Unload: `launchctl unload` then `load` to restart after config change | [launchd](https://developer.apple.com) |
| Log to /tmp/ — check logs if LaunchAgent fails silently | [launchd](https://developer.apple.com) |
| openclaw-bridge managed by LaunchAgent — gateway always running | [openclaw](https://github.com/hmzainjamil/hmz-openclaw) |

<a id="tips-hooks-auto"></a>
### ■ **Auto-Hooks (6)**
| Tip | Source |
|---|---|
| auto-github-push hook fires on every Write/Edit → routes to correct repo | [hooks](https://github.com/hmzainjamil/claude-ai-system) |
| bin/ files → claude-ai-system repo | skills/ files → claude-ai-skills | [hooks](https://github.com/hmzainjamil/claude-ai-system) |
| UserPromptSubmit → skill-auto-activate fires before every response | [settings.json](https://github.com/hmzainjamil/claude-ai-system) |
| Stop hook → session-queue → memory files → next session has full context | [hooks](https://github.com/hmzainjamil/claude-ai-system) |
| Paperclip sync hook fires on every MAE completion — zero-effort memory | [hooks](https://github.com/hmzainjamil/claude-ai-system) |
| All hooks run async — never block main conversation thread | [settings.json](https://github.com/hmzainjamil/claude-ai-system) |

<a id="tips-monitoring"></a>
### ■ **Monitoring (6)**
| Tip | Source |
|---|---|
| tcc-dashboard shows: queue depth, RAM, model health, last run time | [tcc-dashboard](https://github.com/hmzainjamil/claude-ai-system) |
| All MAE logs saved to ~/.claude/tcc-logs/ — grep for any past run | [tcc-logs](https://github.com/hmzainjamil/claude-ai-system) |
| Paperclip AI ingests all automation outputs — searchable forever | [Paperclip](https://paperclip.ai) |
| health.sh pings all models — identifies dead endpoints before blast | [health.sh](https://github.com/hmzainjamil/claude-ai-agents) |
| n8n built-in execution log — every workflow run logged with payload | [n8n](https://n8n.io) |
| Slack notify on automation failure — never miss a broken workflow | [slack](https://slack.com) |

---

## ☠️ STARTUPS / BUSINESSES

| Feature | Replaced |
|---|---|
| 8159 n8n workflows | [Zapier](https://zapier.com) 20-step plans |
| MAE 12-agent swarm | [Make.com](https://make.com) scenarios |
| OpenCLI 90+ adapters | [Browser.ai](https://browser.ai) |
| Composio 3000+ actions | [Pipedream](https://pipedream.com) |
| Codex agent automation | [Relevance AI](https://relevanceai.com) |
| DigiMinds daily ops | [Notion AI](https://notion.ai) tasks |
| LaunchAgent always-on | [Heroku](https://heroku.com) scheduler |
| Paperclip company OS | [Notion](https://notion.so) wiki |
| Auto-GitHub push hook | [GitHub Actions](https://github.com/features/actions) |
| Webhook trigger layer | [Segment](https://segment.com) |
| TCC task queue | [Linear](https://linear.app) |
| tcc-dashboard monitoring | [Datadog](https://datadoghq.com) |

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hmzainjamil/claude-ai-automations&type=Date)](https://star-history.com/#hmzainjamil/claude-ai-automations&Date)
