# CLAUDE.md

This file provides guidance to Claude Code when working in the bot-fleet-continuum repository.

## Repository Overview

**bot-fleet-continuum** is the Enterprise Architecture governance repository for Bot Fleet Inc (BFI). It contains ArchiMate viewpoints, BPMN processes, technology standards, and governance records for the autonomous AI bot fleet.

## Key Concepts

- **ArchiMate 3.2** is the modelling standard — all architecture documentation follows ArchiMate viewpoint structure
- **BPMN 2.0** defines process workflows for bot coordination
- **Architecture Council** governs this repo via Issues — archi-bot proposes, audit-bot reviews, Jorgen approves

## Working in This Repo

### Document Structure

- **Architecture/** — ArchiMate viewpoints organised by type (Layered, Technology, Motivation)
- **Governance/** — Organisation registry, decision logs, account inventories
- **Process/** — BPMN process definitions
- **Standards/** — Coding, deployment, and security standards specific to BFI

### File Naming

Follow the pattern: `[subject]_[viewpoint].md`

Examples:
- `bot-fleet-isolation-model_technology.md`
- `bot-fleet-organization_layered.md`
- `github-cloudflare-account-mapping_technology.md`

### Governance Documents

- `ORGANIZATION_REGISTRY.md` — Canonical list of accounts, domains, and services
- `DECISION_LOG.md` — Architectural Decision Records (ADRs)

### Conventions

- Use Mermaid diagrams for visual models
- Link every decision to a GitHub Issue number
- Tables for structured data (account mappings, service catalogues)
- Keep documents focused — one viewpoint per file

## Related Repositories

| Repository | Org | Purpose |
|-----------|-----|---------|
| fleet-ops | Bot-Fleet-Inc | Bot implementations and infrastructure |
| fleet-vault | Bot-Fleet-Inc | Obsidian-compatible knowledge vault |

## No Build Process

This is a documentation-only repository. No build, test, or deployment commands.
