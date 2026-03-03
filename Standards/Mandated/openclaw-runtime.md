# Mandated Standard: OpenClaw Agent Runtime

**Status**: Mandated
**Effective Date**: 2026-03-03
**Decision Record**: [ADR-003](../../Governance/DECISION_LOG.md#adr-003-openclaw-as-model-agnostic-agent-runtime)

---

## Standard

All bots in Bot Fleet Inc **MUST** use OpenClaw as their agent runtime. No bot may use a vendor-specific CLI (e.g., Claude Code CLI) as its primary runtime.

## Rationale

OpenClaw is model-agnostic — it supports 14+ LLM providers and enables 4-tier routing that selects the right model for each task's complexity. This eliminates vendor lock-in, reduces per-bot cost (Gemini free tier for most tasks), and allows graceful escalation to frontier models when needed.

## Required Configuration

Each bot must have `.openclaw/openclaw.json` in its workspace with at minimum:

| Field | Required | Description |
|-------|----------|-------------|
| `model` | Yes | Default model (e.g., `google/gemini-2.5-flash-preview`) |
| `providers` | Yes | Provider configuration with API key references |
| `safeBins` | Yes | Exec tool allowlist (minimum: `gh`, `git`, `curl`) |
| `memory.enabled` | Yes | Must be `true` — memory persistence is mandatory |
| `memory.provider` | Yes | Embedding provider for memory search |

## Required Environment Variables

Each bot's env file (`/opt/bot/secrets/<bot-name>.env`) must contain:

| Variable | Scope | Description |
|----------|-------|-------------|
| `ANTHROPIC_API_KEY` | Shared (fleet-wide) | Claude Sonnet/Opus escalation (pay-as-you-go) |
| `GEMINI_API_KEY` | Per-bot | Gemini Flash free tier access |
| `OPENCLAW_HOOK_TOKEN` | Per-bot | Gateway authentication token |
| `GITHUB_TOKEN` | Per-bot | GitHub API access via `gh` CLI |

## 4-Tier LLM Routing

All bots must implement 4-tier routing:

| Tier | Model | Provider | Use Case |
|------|-------|----------|----------|
| 1 (Low) | Local LLM | Ollama (A10 GPU) | Classification, labeling, summarization |
| 2 (Medium) | Gemini 2.5 Flash | Google AI (free tier) | Triage, dispatch, chat, analysis |
| 3 (High) | Claude Sonnet | Anthropic API | Multi-domain analysis, code review |
| 4 (Critical) | Claude Opus | Anthropic API | Escalation reasoning, complex decisions |

## systemd Unit

Bots run as `openclaw-bot@<bot-name>.service` using the template unit at `shared/config/systemd/openclaw-bot@.service`.

## Compliance Verification

```bash
# Verify OpenClaw is installed
openclaw --version

# Verify gateway health
openclaw gateway health

# Verify config exists
cat .openclaw/openclaw.json | jq .model
```

## Exceptions

None. All bots must use OpenClaw. The legacy `.claude/CLAUDE.md` files are retained for reference only — OpenClaw does not read them.
