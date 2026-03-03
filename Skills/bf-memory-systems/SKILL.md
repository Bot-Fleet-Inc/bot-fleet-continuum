---
name: bf-memory-systems
description: Memory architecture for Bot Fleet bots — four methods from simple to advanced, with phased adoption roadmap.
version: 1.0
status: recommended
last_updated: 2026-03-03
owner: architecture-council
dependencies: []
related_skills:
  - ea-core-advisor
---

# Bot Fleet Memory Systems

**Version**: 1.0
**Last Updated**: 2026-03-03
**Purpose**: Define and configure persistent memory for Bot Fleet bots using four complementary methods, from structured folders to semantic search and SQLite.

> **Source**: Adapted from [OpenClaw Memory Systems spec](https://youtu.be/Nt03hgxv5TE) and fleet operational experience.

---

## When to Use This Skill

Invoke this skill when:

- Provisioning a new bot and configuring its memory system
- Enabling semantic memory search on an existing bot
- Deciding which memory method to adopt next
- Debugging memory-related issues (context loss, stale data, token bloat)
- Planning structured data storage for fleet state

---

## Prerequisites

- Access to the bot's workspace on its VM (`/opt/bot/workspace/<bot-name>`)
- OpenClaw config at `.openclaw/openclaw.json` in the bot's private repo
- For Method 2: Gemini API key (already provisioned per-bot in 1Password)
- For Method 3: npm access for plugin installation (future — pending security review)

---

## The Problem

A default OpenClaw setup dumps everything into a single massive `MEMORY.md` file which:
- Confuses the agent with irrelevant context
- Burns tokens on every request
- Requires users to repeat context each session
- Costs unnecessarily on API billing

**Solution**: Four methods for persistent memory, from simple to advanced. Use them individually or combine for full coverage.

---

## Method 1: Structured Folders (Current Standard)

**Best for**: Transparency, control, getting started fast
**Setup**: ~5 minutes
**Requirements**: None (works with all LLMs)
**Status**: Already in use across the fleet

### Directory Structure

```
workspace/
├── memory/
│   ├── YYYY-MM-DD.md          # Daily logs
│   └── MEMORY.md               # Curated long-term memory
├── projects/
│   └── {project-name}/
│       ├── goals.md
│       ├── decisions.md
│       └── status.md
├── preferences/
│   ├── coding-style.md
│   └── communication.md
└── knowledge/
    ├── research-notes.md
    └── references.md
```

### Implementation

1. **Create folder structure** in bot workspace
2. **Instruct the agent** via `AGENTS.md` or system prompt:
   ```markdown
   ## Memory Rules
   - Read `memory/YYYY-MM-DD.md` (today + yesterday) on every session start
   - Read `MEMORY.md` in MAIN SESSION only (not in group chats)
   - Update relevant files at end of session
   - Write significant events to daily files
   - Periodically distill daily files into MEMORY.md
   ```

### Strengths

- Transparent — you see exactly what the agent remembers
- Flexible — works with Anthropic, Google, OpenAI, local models
- Full control — you decide what gets remembered
- No dependencies — just markdown files

### Limitations

- No automatic semantic search
- Requires manual organisation

---

## Method 2: Memory Search (Built-in)

**Best for**: Natural language search across stored memories
**Setup**: ~2 minutes (if API keys are configured)
**Requirements**: Gemini API key (already provisioned per-bot)
**Status**: Ready to enable

### How It Works

OpenClaw has a built-in `memory_search` function that lets the agent:
- Store memories: `"Remember that we prefer TypeScript over JavaScript"`
- Retrieve memories: `"What are our coding preferences?"`

**Critical note**: Memory search requires an embedding provider (Google/OpenAI/Voyage) — it does **not** work with only an Anthropic API key.

### Implementation

Memory search is already configured in the fleet's `openclaw.json.template`:

```json
{
  "memory": {
    "enabled": true,
    "embeddingProvider": "google"
  }
}
```

The `GEMINI_API_KEY` environment variable (injected from `/opt/bot/secrets/<bot-name>.env`) provides the embedding capability.

To enable on a bot:
1. Verify `GEMINI_API_KEY` is set in the bot's env file
2. Confirm `memory.enabled: true` in `.openclaw/openclaw.json`
3. Restart the bot service: `systemctl restart openclaw-bot@<bot-name>.service`

### Strengths

- Built-in — no plugins needed
- Semantic search — finds related info even with different wording
- Cheap — embedding calls cost fractions of a cent

### Limitations

- Requires an embedding API key (separate from the main LLM key)
- Silent failures if misconfigured
- Error messages not always clear

---

## Method 3: MEM0 Plugin (Future — Pending Security Review)

**Best for**: Automatic long-term memory without manual intervention
**Setup**: ~10 minutes
**Requirements**: `@mem0/openclaw-mem0` plugin
**Status**: Under evaluation — **do not deploy** until security review is complete

### How It Works

MEM0 observes all conversations and:
1. **Monitors** — watches every message in the background
2. **Extracts** — identifies important info (preferences, decisions, facts)
3. **Stores** — saves as vector embeddings (semantic database)
4. **Retrieves** — injects relevant memories back into context automatically

### Security Concerns

- Data may be sent via MEM0's infrastructure — review privacy documentation before adopting
- Third-party dependency in the agent's memory pipeline
- May retrieve irrelevant memories without tuning

### Implementation (When Approved)

1. Install plugin:
   ```bash
   npm install @mem0/openclaw-mem0
   ```

2. Enable in config:
   ```json
   {
     "plugins": [
       {
         "package": "@mem0/openclaw-mem0",
         "enabled": true
       }
     ]
   }
   ```

### Strengths

- Fully automatic — no manual work
- Semantic understanding — matches meaning, not just keywords
- Works with all LLMs

### Limitations

- Third-party dependency
- Data privacy concerns
- Can retrieve irrelevant memories

---

## Method 4: SQLite Database (Structured Data)

**Best for**: Tightly structured data requiring precise queries
**Setup**: ~15 minutes (first time)
**Requirements**: None (SQLite is built into OpenClaw)
**Status**: Available — use for fleet state tracking

### When to Use

Markdown and vector search fall short when you have:
- Hundreds of API endpoints to track
- Token expiry dates across multiple bots
- Issue tracking state and statistics
- Fleet inventory and configuration data

### How It Works

OpenClaw can natively read/write SQLite databases. No plugin needed — just SQL.

**Example**:
```
User: "Create a SQLite database to track all fleet bot tokens.
       Store: bot_name, token_type, issued_date, expiry_date, status."

Agent: [Creates database, defines schema, populates data]

User: "Which tokens expire in the next 30 days?"
Agent: [Translates to SQL, runs query, provides answer]
```

### Implementation

1. Ask the agent to create a database:
   ```
   Create a SQLite database at data/fleet-state.db
   Schema: bot_name, token_type, issued_date, expiry_date, status
   ```

2. Populate data (manually or via agent)

3. Query naturally:
   ```
   "Which bots have tokens expiring this week?"
   "Show all bots with active Gemini API keys"
   ```

### Strengths

- No dependencies — SQLite is built in
- Precise queries — SQL is designed for this
- Portable — single `.db` file, can be version-controlled
- Persistent — survives session resets

### Limitations

- Requires SQL understanding (but the agent can generate queries)
- Not semantic search (use Method 2 for that)

---

## Recommended Phased Approach

| Phase | Method | Status | Action |
|-------|--------|--------|--------|
| **Now** | Method 1: Structured Folders | Active | Already in place across all deployed bots |
| **Next** | Method 2: Memory Search | Ready | Enable with Gemini API key, verify `openclaw.json` config |
| **Later** | Method 4: SQLite | Available | Use for structured fleet data (token tracking, issue stats) |
| **Evaluate** | Method 3: MEM0 | Blocked | Complete security review before any deployment |

### Combining Methods (Target State)

- **Short-term memory**: Context window (built-in)
- **Medium-term**: Memory search (Method 2) for semantic recall across sessions
- **Long-term**: Structured folders (Method 1) for curated, human-auditable knowledge
- **Structured data**: SQLite (Method 4) for fleet state and precise queries

---

## Validation Checklist

- [ ] Bot has `memory/` directory with `MEMORY.md` and daily log files
- [ ] `AGENTS.md` or system prompt includes memory rules
- [ ] `openclaw.json` has `memory.enabled: true` and `embeddingProvider: "google"`
- [ ] `GEMINI_API_KEY` is set in `/opt/bot/secrets/<bot-name>.env`
- [ ] Bot service restarted after config changes
- [ ] Memory search returns results for a test query

---

## Common Issues

| Symptom | Cause | Solution |
|---------|-------|----------|
| Memory search returns nothing | `memory.enabled` is false or missing | Set `memory.enabled: true` in `openclaw.json` |
| Memory search silent failure | No embedding API key | Verify `GEMINI_API_KEY` is set in bot env file |
| Token bloat from MEMORY.md | File too large, read on every request | Distill to essentials, archive old entries to daily logs |
| Context loss between sessions | Bot not reading daily logs on start | Update `AGENTS.md` memory rules to read today + yesterday |
| SQLite query errors | Schema mismatch or empty database | Ask agent to show generated SQL, verify with `.schema` |

---

## Examples

### Example 1: Enable Memory Search on a Bot

```bash
# 1. Verify API key exists
ssh bot@172.16.10.21 "grep GEMINI_API_KEY /opt/bot/secrets/dispatch-bot.env"

# 2. Verify config
ssh bot@172.16.10.21 "cat /opt/bot/workspace/dispatch-bot/.openclaw/openclaw.json | grep -A2 memory"

# 3. Restart service
ssh bot@172.16.10.21 "sudo systemctl restart openclaw-bot@dispatch-bot.service"

# 4. Test via chat
# "Remember that dispatch-bot was deployed on 2026-03-03"
# ... later ...
# "When was dispatch-bot deployed?"
```

### Example 2: Create Fleet State Database

```sql
-- Schema for token tracking
CREATE TABLE bot_tokens (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  bot_name TEXT NOT NULL,
  token_type TEXT NOT NULL,  -- 'github_pat', 'gemini_api', 'anthropic_api', 'openclaw_hook'
  issued_date TEXT NOT NULL,
  expiry_date TEXT,
  status TEXT DEFAULT 'active',  -- 'active', 'expiring', 'expired', 'rotated'
  onepassword_item TEXT          -- 1Password item reference
);

-- Example query: tokens expiring in next 30 days
SELECT bot_name, token_type, expiry_date
FROM bot_tokens
WHERE expiry_date <= date('now', '+30 days')
  AND status = 'active'
ORDER BY expiry_date;
```

---

## References

- **OpenClaw config template**: `ai-bot-fleet-org/shared/config/openclaw/openclaw.json.template`
- **Fleet knowledge**: `ai-bot-fleet-org/shared/config/fleet-knowledge.md` (Key Standards section)
- **Bot provisioning runbook**: `fleet-ops/docs/bot-provisioning-runbook.md`
- **1Password vault**: "Bot Fleet Vault" — Gemini API keys per bot
- **OpenClaw memory docs**: [docs.openclaw.ai/memory](https://docs.openclaw.ai/memory)

---

## Version History

- v1.0 (2026-03-03): Initial creation — four memory methods, phased adoption roadmap for Bot Fleet Inc
