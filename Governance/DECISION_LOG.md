# Decision Log

Architectural Decision Records (ADRs) for Bot Fleet Inc.

---

## ADR-001: Establish Bot Fleet Inc as Independent Organisation

**Date**: 2026-03-03
**Status**: Accepted
**Decider**: Jorgen Scheel

### Context

The AI bot fleet was developed in the `Oss-Gruppen-AS/ai-bot-fleet-org` incubator repo. Bots need their own org where they can operate autonomously with proper isolation.

### Decision

Create `Bot-Fleet-Inc` as the bots' working GitHub organisation. Bots are members of BFI only — never invited to other orgs. Work is brought into BFI by the human manager via Issues.

### Consequences

- Clean separation between incubator (design) and production (operations)
- Bots have least-privilege access — only BFI repos
- External deliverables are ported by the human manager
- Three repos: `fleet-ops` (operational), `bot-fleet-continuum` (EA), `fleet-vault` (knowledge)

---

## ADR-002: Incremental Bot Onboarding

**Date**: 2026-03-03
**Status**: Accepted
**Decider**: Jorgen Scheel

### Context

The fleet has 10+ planned bots. Mass provisioning creates risk — untrained bots generating noise.

### Decision

Onboard bots one at a time, when the organisation needs their capability. Each onboarding follows the Bot Resources (BR) playbook and validates the process.

### Consequences

- dispatch-bot is first, proving the deployment pipeline
- Each subsequent bot benefits from lessons learned
- BR playbooks evolve with each cycle
- Fleet grows organically based on actual need

---

## ADR-003: OpenClaw as Bot Runtime

**Date**: 2026-03-03
**Status**: Accepted
**Decider**: Jorgen Scheel

### Context

The bot fleet initially used Claude Code CLI as the runtime for autonomous agents. This created a hard dependency on a single LLM provider and limited model routing flexibility. The fleet needed a model-agnostic runtime that could route between local LLMs (Ollama), free-tier APIs (Gemini Flash), and paid APIs (Claude Sonnet/Opus) based on task complexity.

### Decision

Migrate all bot runtimes from Claude Code CLI to **OpenClaw** — a model-agnostic agent runtime (npm package) supporting 14+ LLM providers. Each bot runs via a systemd template unit (`openclaw-bot@.service`) with configuration in `.openclaw/openclaw.json`.

### Consequences

- 4-tier LLM routing: Local LLM (free) -> Gemini Flash (free tier) -> Claude Sonnet (API) -> Claude Opus (API)
- Default model: `google/gemini-2.5-flash-preview` (free tier for routine operations)
- Anthropic API used only for escalation — significant cost reduction
- Memory search enabled via Gemini embeddings
- Exec tool allowlisted: `gh`, `git`, `curl` via safeBins config
- All 29 files updated in PR #42 (`ai-bot-fleet-org`) — zero Claude Code CLI references remain

---

## ADR-004: Skillset Distribution via Pipeline

**Date**: 2026-03-03
**Status**: Accepted
**Decider**: Jorgen Scheel

### Context

Bots need access to enterprise skills (ArchiMate, BPMN, deployment patterns) but skills are authored in multiple source repos (`enterprise-continuum`, `bot-fleet-continuum`). Manual skill copying is error-prone and doesn't scale.

### Decision

Distribute skills via an automated pipeline: `build-skillset.sh` assembles skills from multiple origins into a single skillset, then `push-to-user-repo.sh` publishes to `Bot-Fleet-Inc/skillset-<bot-name>`. VMs pull skillsets via git clone to `/opt/bot/workspace/skillset-<bot-name>/skills`.

### Consequences

- Skillset configs live in `second-brain/skillset-configs/<bot-name>.yaml` with `repo_org` field
- Push targets org-owned repos (`Bot-Fleet-Inc/skillset-<bot-name>`)
- Skills sourced from `enterprise` and `botfleet` origins
- VM path: `/opt/bot/workspace/skillset-<bot-name>/skills` (configured in openclaw.json)
- Pipeline is reusable for all future bots — just add a YAML config
- Existing user skillset configs unaffected (empty `repo_org` falls back to personal repos)
