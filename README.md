# bot-fleet-continuum

Enterprise Architecture governance repository for **Bot Fleet Inc (BFI)**.

This repository is the **standards and architecture reference** for the `Bot-Fleet-Inc` GitHub organisation. It contains ArchiMate viewpoints, BPMN processes, technology standards, and governance records for the autonomous AI bot fleet.

## Purpose

- **ArchiMate viewpoints** documenting the bot fleet architecture
- **BPMN process definitions** for bot coordination workflows
- **Technology standards** governing bot implementation patterns
- **Governance records** for organisation setup, accounts, and access control
- **Skills** for Claude Code / OpenClaw bot capabilities

## Structure

```
bot-fleet-continuum/
+-- Architecture/
|   +-- Application-Cooperation/  # How bots and services integrate
|   +-- Application-Usage/        # User/human interaction patterns
|   +-- Implementation/           # Deployment and operations
|   +-- Information-Structure/    # Data architecture
|   +-- Layered/                  # ArchiMate Layered viewpoints
|   +-- Migration/                # Architecture transitions
|   +-- Models/                   # ArchiMate .archimate model files
|   +-- Motivation/               # Goals, principles, drivers
|   +-- Physical/                 # Equipment, VMs, network topology
|   +-- Project/                  # Project management and workflows
|   +-- Technology/               # Technology Layer viewpoints
|   `-- README.md                 # Viewpoint index and relationship map
+-- Governance/
|   +-- ORGANIZATION_REGISTRY.md  # Accounts, domains, services
|   `-- DECISION_LOG.md           # Architectural decisions (ADRs)
+-- Processes/
|   +-- Level-1-Orchestration/    # Top-level orchestration flows
|   `-- Level-2-Coordination/     # Coordination and subprocess flows
+-- Skills/
|   `-- bf-memory-systems/        # Memory systems skill for bot fleet
+-- Standards/
|   +-- Mandated/                 # MUST follow
|   +-- Recommended/              # SHOULD follow
|   `-- Emerging/                 # MAY explore
+-- CLAUDE.md                     # Claude Code guidance
`-- CONTRIBUTING.md               # Document placement and naming guide
```

## Related Repositories

| Repository | Purpose |
|-----------|---------|
| [fleet-ops](https://github.com/Bot-Fleet-Inc/fleet-ops) | Bot implementations, shared libraries, infrastructure configs |
| [fleet-vault](https://github.com/Bot-Fleet-Inc/fleet-vault) | Obsidian-compatible knowledge vault |

## Conventions

- All documents follow ArchiMate 3.2 viewpoint structure
- File naming: `[subject]_[viewpoint].md` (e.g. `bot-fleet-isolation-model_technology.md`)
- Diagrams use Mermaid syntax embedded in markdown
- Every architectural decision links to a GitHub Issue for traceability
- See [CONTRIBUTING.md](CONTRIBUTING.md) for document placement guidelines

## Ownership

- **Architecture Council**: governs via Issues in this repo
- **Maintained by**: archi-bot (model updates), audit-bot (compliance reviews)
- **Human oversight**: Jorgen Scheel (`jorgen@scheel.no`)
