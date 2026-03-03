# Standards Library

Organized by maturity level to indicate enforcement.

## Maturity Levels

### Mandated/
**MUST follow** - Required for all BFI bots and services

These standards are non-negotiable and enforced through:
- Code review
- Automated checks (where applicable)
- Audit compliance (audit-bot)

### Recommended/
**SHOULD follow** - Strong preference unless documented exception

These standards are proven and preferred. Deviations require:
- Clear rationale
- Documentation in bot workspace or project README
- Alternative approach that meets same goals

### Emerging/
**MAY explore** - Experimental, not for production

These are patterns being evaluated:
- Proof-of-concept stage
- Limited production use
- Feedback welcome

## Standard Promotion Path

```
Emerging --> (proven in 2+ bots) --> Recommended --> (Architecture Council decision) --> Mandated
```

## Current Standards

*No standards documents yet. Standards will be created as bot fleet patterns mature.*

## Finding Standards

Standards reference documents in `Architecture/` viewpoints. The standards themselves are pointers to:
- Full documentation in viewpoint folders
- BPMN processes in `Processes/`

## Relationship to Architecture Viewpoints

Standards answer "must/should/may" while viewpoints answer "what/how/why":

- **Standard:** "MUST use OpenClaw runtime for bot deployment" (Mandated)
- **Viewpoint:** `openclaw-runtime_technology.md` explains configuration, rationale, and patterns

## Adding Standards

See [CONTRIBUTING.md](../CONTRIBUTING.md) for placement guidelines.

All standards require Architecture Council approval before addition to this library.
