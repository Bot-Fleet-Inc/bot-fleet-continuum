# bot-fleet-continuum

Enterprise Architecture governance repository for **Bot Fleet Inc (BFI)**.

This repository is the **standards and architecture reference** for the `Bot-Fleet-Inc` GitHub organisation. It contains ArchiMate viewpoints, BPMN processes, technology standards, and governance records for the autonomous AI bot fleet.

## Purpose

- **ArchiMate viewpoints** documenting the bot fleet architecture
- **BPMN process definitions** for bot coordination workflows
- **Technology standards** governing bot implementation patterns
- **Governance records** for organisation setup, accounts, and access control

## Structure

```
bot-fleet-continuum/
├── Architecture/
│   ├── Layered/           # ArchiMate Layered viewpoints
│   ├── Technology/        # Technology Layer viewpoints
│   └── Motivation/        # Motivation Layer viewpoints
├── Governance/
│   ├── ORGANIZATION_REGISTRY.md   # Accounts, domains, services
│   └── DECISION_LOG.md            # Architectural decisions
├── Process/
│   └── (BPMN process definitions)
└── Standards/
    └── (Coding, deployment, security standards)
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

## Ownership

- **Architecture Council**: governs via Issues in this repo
- **Maintained by**: archi-bot (model updates), audit-bot (compliance reviews)
- **Human oversight**: Jorgen Scheel (`jorgen@scheel.no`)
