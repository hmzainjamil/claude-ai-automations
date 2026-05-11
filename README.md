<p align="center">
  <img src="https://img.shields.io/badge/HMZ-AUTOMATIONS-F86606?style=for-the-badge&logoColor=white" alt="HMZ Automations" height="60">
</p>

<h1 align="center">Claude AI Automations</h1>

<p align="center">
  <strong>Core automation scripts powering a zero-human AI agency — daily sync, model routing, multi-LLM burst, LaunchAgents</strong>
</p>

<p align="center">
  <a href="https://github.com/hmzainjamil"><img src="https://img.shields.io/badge/By-HMZ-6C3EE8?style=for-the-badge" alt="By HMZ"></a>
  <a href="#scripts"><img src="https://img.shields.io/badge/Scripts-25%2B-20A34E?style=for-the-badge" alt="25+ Scripts"></a>
  <a href="#launchagents"><img src="https://img.shields.io/badge/LaunchAgents-12-246DFF?style=for-the-badge" alt="12 LaunchAgents"></a>
  <a href="#"><img src="https://img.shields.io/badge/Runs%20Daily-6%3A30%20AM-F86606?style=for-the-badge" alt="Runs Daily"></a>
</p>

<p align="center">
  <a href="#overview">Overview</a> &bull;
  <a href="#scripts">Scripts</a> &bull;
  <a href="#launchagents">LaunchAgents</a> &bull;
  <a href="#quick-start">Quick Start</a> &bull;
  <a href="#use-cases">Use Cases</a> &bull;
  <a href="#installation">Installation</a>
</p>

---

## Overview

**Claude AI Automations** is the operational backbone of the HMZ AI system. These scripts run silently in the background every day, keeping the entire infrastructure fresh, synced, and optimized — without any manual intervention.

Key automations:
- **`github-sync`** — Syncs all skills, agents, workflows, and scripts to GitHub daily at 6:30 AM. Auto-discovers new locally installed repos and creates GitHub repos for them. Audits all README files for quality (15,000+ char threshold).
- **`llm-burst`** — Fires 15 AI models in parallel (Groq, Gemini, DeepSeek, GPT4All, Ollama, Kimi K2, and more). Judge picks winner. Claude tokens preserved.
- **`skill-auto-activate`** — Runs on every Claude Code prompt. Keyword-matches → auto-activates relevant skills before response.
- **`skill-on` / `skill-off`** — Activate and deactivate individual skills on demand.
- **`free-coding-models`** — Live-pings 170 free AI models across 16 providers. Returns fastest available. Auto-writes winner into coding tool config.

---

## Scripts

| Script | What it does | Runs |
|---|---|---|
| `github-sync` | Full system → GitHub push. README audit. New repo discovery. | Daily 6:30 AM |
| `llm-burst` | 15-model parallel burst. Judge scoring. Best answer wins. | On demand |
| `skill-auto-activate` | Keyword detect → auto-load skills before every prompt | Every prompt |
| `skill-on` | Activate a dormant skill | On demand |
| `skill-off` | Deactivate a skill after task | On demand |
| `skill-search` | Find skills by keyword across skills-archive | On demand |
| `free-coding-models` | Live-ping 170 free AI models across 16 providers | On demand |
| `agency-run` | Orchestrate all 210 agents for a complex task | On demand |
| `pdf-report` | Generate 11-page ReportLab audit PDF | On demand |
| `readme-audit` | Check all GitHub repos for README quality | Daily (in github-sync) |

---

## LaunchAgents

macOS LaunchAgent plists in `automations/launchagents/`:

| LaunchAgent | Schedule | What it does |
|---|---|---|
| `ai.hmz.github-portfolio-sync` | Daily 6:30 AM | Runs `github-sync` — full system push |
| `ai.openclaw.gateway` | At login | OpenClaw AI gateway — persistent |
| `ai.hmz.paperclip` | Manual | Paperclip AI zero-human company platform |
| `ai.hmz.ollama` | At login | Ollama local AI (llama3 on GPU) |
| `ai.hmz.n8n` | At login | n8n local workflow server |
| `ai.hmz.open-design` | At login | Open Design server (port 51827) |

---

## Quick Start

```bash
# Manual GitHub sync (normally runs automatically at 6:30 AM)
~/.claude/bin/github-sync

# Multi-LLM burst — 15 models, best answer wins
~/.claude/bin/llm-burst "Write a cold email for digital agency services"

# Find fastest free AI model right now
free-coding-models

# Activate a skill
~/.claude/bin/skill-on ads-strategy

# Search for skills by topic
~/.claude/bin/skill-search "lead generation"

# Run all 210 agents on a complex task
~/.claude/bin/agency-run "Full GTM strategy for SaaS product targeting SMBs"
```

---

## Use Cases

| Goal | Script | Command |
|---|---|---|
| **Push all changes to GitHub** | `github-sync` | `~/.claude/bin/github-sync` |
| **Get best AI answer cheaply** | `llm-burst` | `llm-burst "your prompt"` |
| **Find cheapest fast model** | `free-coding-models` | `free-coding-models` |
| **Activate paid media mode** | `skill-on` | `skill-on ads-strategy` |
| **Run 210 agents in parallel** | `agency-run` | `agency-run "task description"` |
| **Generate a PDF audit** | `pdf-report` | `pdf-report client.com` |
| **Check README quality** | `readme-audit` | Runs inside `github-sync` |

---

## github-sync Deep Dive

`github-sync` is the most critical automation in the system. On each run it:

1. Syncs active skills → `skills-active/`
2. Deduplicates and syncs skills archive → `skills-archive/`
3. Syncs all 210 agents → `agents/`
4. Syncs scheduled tasks → `scheduled-tasks/`
5. Syncs all automation scripts → `automations/bin/`
6. Syncs LaunchAgent plists (with credential scrubbing)
7. Rebuilds the n8n workflow manifest (8,000+ workflows categorized)
8. Strips all API keys, tokens, secrets before commit
9. Git commit + push to `claude-ai-system`
10. **README audit** — checks all 27+ repos for README size ≥ 15,000 chars
11. **New repo discovery** — finds local repos in `~/installed-repos/` not on GitHub → creates them + pushes professional README

---

## Installation

```bash
# Clone the automations
git clone https://github.com/hmzainjamil/claude-ai-automations.git

# Install bin scripts
cp automations/bin/* ~/.claude/bin/
chmod +x ~/.claude/bin/*

# Install LaunchAgents
cp automations/launchagents/ai.hmz.github-portfolio-sync.plist ~/Library/LaunchAgents/
launchctl load ~/Library/LaunchAgents/ai.hmz.github-portfolio-sync.plist
```

---

## Resources

- **[claude-ai-system](https://github.com/hmzainjamil/claude-ai-system)** — Full system repo (synced by these scripts)
- **[claude-ai-skills](https://github.com/hmzainjamil/claude-ai-skills)** — Skills managed by these scripts
- **[claude-ai-agents](https://github.com/hmzainjamil/claude-ai-agents)** — Agents managed by these scripts

## License

MIT

---

<p align="center">Built by <a href="https://github.com/hmzainjamil">Hafiz Muhammad Zulqarnain</a> &mdash; HMZ AI Agency</p>