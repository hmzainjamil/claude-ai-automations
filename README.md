# claude-ai-automations

> **Complete automation infrastructure** — scripts, LaunchAgents, hooks, and pipelines running 24/7 on HMZ AI Agency's system.

Part of [claude-ai-system](https://github.com/hmzainjamil/claude-ai-system).

---

## Overview

Every script, daemon, and hook that powers the autonomous operation of the HMZ AI system — from daily GitHub syncs to BDM sweeps, skill routing, and session-level intelligence.

---

## Directory Structure

```
automations/
├── bin/                    ← Executable scripts
│   ├── github-sync         ← Daily portfolio sync to GitHub (all repos)
│   ├── skill-auto-activate ← Keyword router (fires on every Claude prompt)
│   ├── skill-on            ← Activate a skill
│   ├── skill-off           ← Deactivate a skill
│   ├── skill-search        ← Find skills by keyword
│   ├── llm-burst           ← Multi-LLM parallel query (15 models)
│   ├── run-scheduled-task  ← Execute scheduled agent tasks
│   ├── github-portfolio-init ← One-time GitHub repo creation + auth
│   ├── website-builder-setup ← Premium web design stack installer
│   └── auto-troubleshoot   ← Session health checker
└── launchagents/           ← macOS LaunchAgent plists
    ├── ai.hmz.github-portfolio-sync.plist  ← Daily sync at 6:30 AM
    ├── ai.openclaw.gateway.plist           ← OpenClaw bridge (always-on)
    ├── ai.hmz.paperclip.plist              ← Paperclip AI server
    └── ai.hmz.ollama.plist                 ← Ollama local LLM (always-on)
```

---

## Core Scripts

### github-sync
**Trigger:** Daily at 6:30 AM (LaunchAgent) + on-demand

Syncs the complete HMZ AI system to GitHub:
1. Active skills → `claude-ai-system/skills-active/`
2. Skills archive → deduplicated SKILL.md files
3. Agents → markdown/yaml only
4. Scheduled tasks → full content
5. Automation scripts → `automations/bin/`
6. LaunchAgent plists → with credential scrubbing (ghp_/pat/sk- redacted)
7. Installed repos → README/SKILL.md only, capped at 50 repos
8. n8n workflow manifest → categorized index of 8,159 workflows
9. Credential strip → all API keys/tokens redacted before commit
10. Git commit + push → `github.com/hmzainjamil/claude-ai-system`

Also pushes deduplicated skills to: `hmz-skills-archive`

### skill-auto-activate
**Trigger:** `UserPromptSubmit` hook — fires on EVERY Claude prompt

Keyword-routes prompts to relevant skills:
```
ads/ppc/google ads/meta    → ads-strategy, ads-copy, ads-creative
seo/geo/ranking            → geo, geo-technical, geo-content
lead gen/prospect/crm      → leads-generator, outbound-dm-writer
agency/client/proposal     → agency, agency-client, agency-pipeline
apify/scrape/extract       → apify-actor-development
code/bug/build/deploy      → launch-optimized (code agent stack)
report/pdf/document        → report-creator
website/landing page       → website-builder-setup, premium-web-design
```

### llm-burst
**Purpose:** Fire 15 AI models in parallel → judge picks best → Claude synthesizes

```bash
~/.claude/bin/llm-burst "your prompt"
~/.claude/bin/llm-burst --models groq,gemini,deepseek "focused prompt"
~/.claude/bin/llm-burst --json "prompt" | jq '.all[] | {model,score}'
```

**Models:** Groq llama3-70b · Gemini 2.0 Flash · DeepSeek-V3 · GPT-4o-mini · GLM-4.5-air · Gemma4 · 7x GPT4All local models

### run-scheduled-task
**Purpose:** Execute remote agent tasks on cron schedule

---

## LaunchAgent Schedule

| LaunchAgent | Schedule | Purpose |
|---|---|---|
| `ai.hmz.github-portfolio-sync` | Daily 6:30 AM | Sync all repos to GitHub |
| `ai.openclaw.gateway` | Always-on (KeepAlive) | OpenClaw AI gateway bridge |
| `ai.hmz.paperclip` | Always-on (KeepAlive) | Paperclip AI orchestration server |
| `ai.hmz.ollama` | Always-on (KeepAlive) | Local Ollama LLM (llama3, mistral) |

---

## Hook System

Claude Code hooks fire automatically on session events:

| Hook | Trigger | Action |
|---|---|---|
| `UserPromptSubmit` | Every prompt | `skill-auto-activate` + OODA + L99 |
| `SessionStart` | Session open | Health check, model availability scan |
| `Stop` | Session end | `auto-learn` → write learnings to memory |
| `PreToolUse` | Before tool call | Security check, credential guard |

---

## Credential Security

All automation scripts enforce credential stripping before any Git commit:

```bash
# Patterns scrubbed:
ghp_[A-Za-z0-9]{20,}      → [REDACTED_GITHUB_TOKEN]
pat[A-Za-z0-9._]{20,}     → [REDACTED_PAT]
sk-[A-Za-z0-9]{20,}       → [REDACTED_SK]
API_KEY = "..."            → API_KEY = "[REDACTED]"
SECRET = "..."             → SECRET = "[REDACTED]"
TOKEN = "..."              → TOKEN = "[REDACTED]"
```

---

## Token Economy

Every automation enforces the **MANDATE: minimize Claude token consumption**:

- Sub-tasks → routed to Tier 0 (Groq/Gemini/DeepSeek/GPT4All local)
- Web extraction → Apify MCP (zero Claude tokens)
- Parallel execution → single round-trip, multiple results
- Session compression → `compact-guard` + `caveman` compression

**Result: 75-95% Claude token savings across all automated workflows.**

---

## Full System

[claude-ai-system](https://github.com/hmzainjamil/claude-ai-system) | [claude-ai-skills](https://github.com/hmzainjamil/claude-ai-skills) | [claude-ai-agents](https://github.com/hmzainjamil/claude-ai-agents)

---

*Auto-updated daily by github-sync at 6:30 AM — HMZ AI Agency*
