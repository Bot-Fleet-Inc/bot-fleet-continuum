# Technology Viewpoint: OpenClaw Runtime Architecture

**ArchiMate Layer**: Technology
**Viewpoint Type**: Technology Usage
**Decision Record**: [ADR-003](../../Governance/DECISION_LOG.md#adr-003-openclaw-as-model-agnostic-agent-runtime)

---

## Overview

OpenClaw is the model-agnostic agent runtime that powers all bots in Bot Fleet Inc. It replaces the vendor-specific Claude Code CLI with a unified runtime that routes to 14+ LLM providers based on task complexity.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    OpenClaw Agent Runtime                     │
│                                                               │
│  ┌─────────────┐  ┌──────────────┐  ┌───────────────────┐   │
│  │   Gateway    │  │  Model Router │  │  Memory System    │   │
│  │  (health,    │  │  (4-tier      │  │  (Gemini          │   │
│  │   auth)      │  │   selection)  │  │   embeddings)     │   │
│  └─────────────┘  └──────────────┘  └───────────────────┘   │
│                                                               │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │                    Exec Tool Sandbox                     │ │
│  │   safeBins: [gh, git, curl]                              │ │
│  └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
         │              │              │              │
         ▼              ▼              ▼              ▼
   ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
   │  Ollama   │  │  Gemini  │  │  Claude   │  │  Claude  │
   │  (Local)  │  │  Flash   │  │  Sonnet   │  │  Opus    │
   │  Free     │  │  Free    │  │  API $$   │  │  API $$$ │
   └──────────┘  └──────────┘  └──────────┘  └──────────┘
    Tier 1: Low   Tier 2: Med   Tier 3: High  Tier 4: Crit
```

## Model Selection Criteria

| Tier | Complexity | Latency | Cost | Example Tasks |
|------|-----------|---------|------|---------------|
| 1 | Low | <1s | Free | Issue classification, label suggestion, text summarization |
| 2 | Medium | 1-5s | Free | Triage, dispatch, chat responses, standard analysis |
| 3 | High | 3-15s | ~$0.003/1K tokens | Multi-domain analysis, code review, architecture decisions |
| 4 | Critical | 5-30s | ~$0.015/1K tokens | Complex escalation reasoning, cross-bot conflict resolution |

### Routing Logic

1. **Default**: All tasks start at Tier 2 (Gemini Flash)
2. **Downgrade to Tier 1**: Classification, labeling, summarization — pattern-matched by task type
3. **Upgrade to Tier 3**: Multi-file analysis, code review, tasks requiring strong reasoning
4. **Upgrade to Tier 4**: Escalation decisions, policy-level reasoning, cross-domain conflicts
5. **Fallback**: If a tier is unavailable, escalate to the next tier up

## Memory Systems Integration

OpenClaw integrates with the fleet's layered memory architecture:

| Layer | Storage | Update Frequency | Purpose |
|-------|---------|-----------------|---------|
| Session | OpenClaw context window | Continuous | Current task state |
| Daily log | `memory/YYYY-MM-DD.md` | Appended during session | Raw events, decisions |
| Long-term | `MEMORY.md` | Daily curation (02:00 UTC) | Synthesized knowledge |
| Semantic search | Gemini embeddings | On write | Cross-session recall |

Memory search is enabled via `memory.enabled: true` in `openclaw.json`. The embedding provider uses Gemini for cost efficiency.

## Exec Tool Security Model

OpenClaw's `safeBins` configuration restricts which system commands bots can execute:

| Tool | Purpose | Constraints |
|------|---------|-------------|
| `gh` | GitHub CLI — issues, PRs, labels | Scoped to `Bot-Fleet-Inc` org via PAT |
| `git` | Version control — commit, push, pull | Within workspace directory only |
| `curl` | HTTP requests — LLM endpoints, Chat Worker | URLs constrained by network tier (DMZ/Infra-Access) |

All other system commands are blocked. Bots cannot execute arbitrary shell commands.

## Per-Bot Configuration

Each bot's `.openclaw/openclaw.json` contains:

```json
{
  "model": "google/gemini-2.5-flash-preview",
  "providers": {
    "google": { "apiKey": "$GEMINI_API_KEY" },
    "anthropic": { "apiKey": "$ANTHROPIC_API_KEY" }
  },
  "safeBins": ["gh", "git", "curl"],
  "memory": {
    "enabled": true,
    "provider": "google"
  }
}
```

## Infrastructure Dependencies

| Component | Location | Purpose |
|-----------|----------|---------|
| Ollama | VM 450, `172.16.11.10:11434` | Tier 1 — local LLM inference |
| Google AI API | External | Tier 2 — Gemini Flash |
| Anthropic API | External | Tier 3/4 — Claude Sonnet/Opus |
| Cloudflare Tunnel | VM 400, `172.16.10.10` | Outbound internet for API calls |

## Related Documents

- [ADR-003: OpenClaw Runtime](../../Governance/DECISION_LOG.md) — Decision record
- [Mandated Standard](../../Standards/Mandated/openclaw-runtime.md) — Compliance requirements
- [Memory Systems Skill](../../Skills/bf-memory-systems/SKILL.md) — Memory architecture
- [OpenClaw Config Template](https://github.com/Oss-Gruppen-AS/ai-bot-fleet-org/blob/main/shared/config/openclaw/openclaw.json.template) — Reference config
