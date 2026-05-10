# claude-ai-automations

> **All automation scripts, LaunchAgents, and hooks** powering the HMZ AI System. Runs 6 daily sweeps, auto-syncs to GitHub, auto-troubleshoots workflows every session.

Part of the [HMZ AI System](https://github.com/hmzainjamil/claude-ai-system).

## Scripts (`~/.claude/bin/`)

| Script | Purpose | Trigger |
|---|---|---|
| `github-sync` | Sync all repos to GitHub | 6:30 AM daily + on changes |
| `skill-auto-activate` | Keyword-route skills on every prompt | Every prompt (UserPromptSubmit hook) |
| `auto-troubleshoot` | Check all sweeps + LaunchAgents | Every session start |
| `agency-run` | Orchestrate 210 agents by division | `/agency-run` command |
| `llm-burst` | Fire 15 models in parallel | Any sub-task |
| `skill-on` / `skill-off` | Activate/deactivate skills | Manual or auto |

## LaunchAgents (7 always-on)

| Agent | Schedule | Output |
|---|---|---|
| `ai.hmz.github-portfolio-sync` | 6:30 AM | GitHub push |
| `ai.hmz.elite-leads` | 6:00 AM | Top prospects Excel |
| `ai.hmz.bdm-morning` | 7:00 AM | LinkedIn + Indeed leads |
| `ai.hmz.bdm-evening` | Evening | Lead refresh |
| `ai.openclaw.gateway` | Permanent | OpenClaw AI gateway |
| `ai.hmz.ollama` | Permanent | Local LLM server |
| `ai.hmz.paperclip` | On-demand | Agent orchestration :3100 |

**Main repo → [claude-ai-system](https://github.com/hmzainjamil/claude-ai-system)**
