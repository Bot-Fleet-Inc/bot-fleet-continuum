# Organisation Registry

Canonical inventory of accounts, domains, and services for Bot Fleet Inc.

---

## GitHub Organisation

| Property | Value |
|----------|-------|
| **Org name** | `Bot-Fleet-Inc` |
| **Plan** | Free |
| **Owner** | `jorgen-fleet-boss` |
| **URL** | https://github.com/Bot-Fleet-Inc |

## Repositories

### Shared Repositories

| Repo | Purpose | Visibility |
|------|---------|------------|
| `fleet-ops` | Bot implementations, shared code, deployment configs | Public |
| `bot-fleet-continuum` | Enterprise architecture, governance, standards | Public |
| `fleet-vault` | Obsidian-compatible knowledge vault | Public |

### Bot Private Repositories

Each bot has a private repo for disaster recovery (workspace files, OpenClaw config, memory).

| Repo | Owner Bot | Contents |
|------|-----------|----------|
| `dispatch-bot` | botfleet-dispatch | `.openclaw/`, workspace files, `memory/` |

### Skillset Repositories

Skillsets are built by the pipeline and pushed to per-bot repos.

| Repo | Target Bot | Source |
|------|------------|--------|
| `skillset-dispatch-bot` | dispatch-bot | `second-brain` pipeline |

## GitHub Teams

| Team | Purpose | Members |
|------|---------|---------|
| `architecture-council` | Governance, EA decisions | jorgen-fleet-boss, jorgenscheel |
| `core` | Coordinate and decide | botfleet-dispatch |
| `engineering` | Build and design | *(empty — populated on onboarding)* |
| `devops` | Infrastructure operations | *(empty)* |
| `specialist` | Domain-specific services | *(empty)* |

## Bot Accounts

| Bot | GitHub User | Email | Tier | VMID | IP | Status |
|-----|-------------|-------|------|------|----|--------|
| dispatch-bot | `botfleet-dispatch` | `dispatch@bot-fleet.org` | Manager | 411 | 172.16.10.21 | **Deployed** |
| archi-bot | `botfleet-archi` | `archi@bot-fleet.org` | Specialist | — | — | GitHub activated |
| coding-bot | `botfleet-coding` | `coding@bot-fleet.org` | Specialist | — | — | GWS user created |
| devops-cloudflare-bot | `botfleet-devcloudflare` | `devcloudflare@bot-fleet.org` | Specialist | — | — | GWS user created |
| audit-bot | `botfleet-audit` | `audit@bot-fleet.org` | Specialist | — | — | GWS user created |
| design-bot | `botfleet-design` | `design@bot-fleet.org` | Specialist | — | — | GWS user created |
| knowledge-bot | `botfleet-knowledge` | `knowledge@bot-fleet.org` | Service | 417 | 172.16.10.27 | Planned |

## Domain

| Property | Value |
|----------|-------|
| **Domain** | `bot-fleet.org` |
| **Registrar** | Cloudflare |
| **DNS** | Cloudflare |
| **Email routing** | Cloudflare Email Routing -> Email Worker |

## Google Workspace

| Property | Value |
|----------|-------|
| **Domain** | `bot-fleet.org` |
| **Plan** | Cloud Identity Free (50 licenses) |
| **Admin** | `jorgen@bot-fleet.org` |
| **Secondary email (all bots)** | `jorgen@scheel.no` |

## Cloudflare

| Property | Value |
|----------|-------|
| **Account** | Bot Fleet Inc |
| **Account ID** | `b7079628ac25013a2ea7c92db2c99224` |
| **Workers subdomain** | `bot-fleet-inc` |

### Cloudflare Workers

| Worker | URL | Purpose |
|--------|-----|---------|
| Email Worker | `https://botfleet-email.bot-fleet-inc.workers.dev` | Inbound email routing for bots |
| Chat Worker | `https://botfleet-chat.bot-fleet-inc.workers.dev` | Chat integration for bots |

## 1Password

| Property | Value |
|----------|-------|
| **Vault** | Bot Fleet Vault |
| **Service account** | Bot Fleet |
| **Naming convention** | `<Provider> <Type> --- <Owner> --- <Purpose>` (em dash) |

## Infrastructure

| Property | Value |
|----------|-------|
| **Provider** | EcoByte AS |
| **Proxmox node** | AMD EPYC 7282, 62 GB RAM |
| **Bot VLAN** | 1010 (172.16.10.0/24) |
| **LLM VLAN** | 1011 (172.16.11.0/24) |
| **Management VLAN** | 200 |
