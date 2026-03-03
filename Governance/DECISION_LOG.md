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
