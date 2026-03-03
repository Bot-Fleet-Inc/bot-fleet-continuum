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

| Repo | Purpose | Visibility |
|------|---------|------------|
| `fleet-ops` | Bot implementations, shared code, deployment configs | Public |
| `bot-fleet-continuum` | Enterprise architecture, governance, standards | Public |
| `fleet-vault` | Obsidian-compatible knowledge vault | Public |

## GitHub Teams

| Team | Purpose | Members |
|------|---------|---------|
| `architecture-council` | Governance, EA decisions | jorgen-fleet-boss, jorgenscheel |
| `core` | Coordinate and decide | botfleet-dispatch |
| `engineering` | Build and design | *(empty — populated on onboarding)* |
| `devops` | Infrastructure operations | *(empty)* |
| `specialist` | Domain-specific services | *(empty)* |

## Domain

| Property | Value |
|----------|-------|
| **Domain** | `bot-fleet.org` |
| **Registrar** | Cloudflare |
| **DNS** | Cloudflare |
| **Email routing** | Cloudflare Email Routing → Email Worker |

## Google Workspace

| Property | Value |
|----------|-------|
| **Domain** | `bot-fleet.org` |
| **Plan** | Cloud Identity Free (50 licenses) |
| **Admin** | `jorgen@bot-fleet.org` |

## Cloudflare

| Property | Value |
|----------|-------|
| **Account** | Bot Fleet Inc |
| **Account ID** | `b7079628ac25013a2ea7c92db2c99224` |
| **Workers subdomain** | `bot-fleet-inc` |

## 1Password

| Property | Value |
|----------|-------|
| **Vault** | Bot Fleet Vault |
| **Service account** | Bot Fleet |

## Infrastructure

| Property | Value |
|----------|-------|
| **Provider** | EcoByte AS |
| **Proxmox node** | AMD EPYC 7282, 62 GB RAM |
| **Bot VLAN** | 1010 (172.16.10.0/24) |
| **LLM VLAN** | 1011 (172.16.11.0/24) |
| **Management VLAN** | 200 |
