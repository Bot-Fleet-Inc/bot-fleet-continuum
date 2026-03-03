# Contributing to bot-fleet-continuum

## Overview

This repository uses ArchiMate viewpoints to organize architecture documentation for Bot Fleet Inc. Each viewpoint represents a specific architectural concern and should evolve as the fleet grows.

## Naming Conventions

### Documents in Architecture/
**Format:** `topic-name_viewpoint-name.md`

The underscore clearly separates content from viewpoint classification.

**Examples:**
- `openclaw-runtime_technology.md`
- `bot-fleet-isolation-model_technology.md`
- `dispatch-bot-deployment_implementation.md`
- `proxmox-network_physical.md`

### Documents in Processes/
**Format:** `process-name_level-N.bpmn` or `.md`

**Examples:**
- `issue-dispatch_level-1.bpmn`
- `bot-onboarding_level-2.bpmn`

### Documents in Standards/
**Format:** `standard-name.md`

Place in the appropriate maturity tier (`Mandated/`, `Recommended/`, `Emerging/`).

**Examples:**
- `Mandated/openclaw-runtime.md`
- `Recommended/memory-systems.md`

### Documents in Skills/
**Format:** `bf-skill-name/SKILL.md`

Each skill gets its own directory under `Skills/` with a `SKILL.md` file.

**Examples:**
- `Skills/bf-memory-systems/SKILL.md`

## Document Placement Decision Tree

```
Is it about deployment/operations procedures?
  --> Architecture/Implementation/

Is it about architecture transitions (As-Is -> To-Be)?
  --> Architecture/Migration/

Is it about how bots and services integrate?
  --> Architecture/Application-Cooperation/

Is it about user/human interaction with bots?
  --> Architecture/Application-Usage/

Is it about software/platforms/logical infrastructure?
  --> Architecture/Technology/

Is it about physical equipment/VMs/network topology?
  --> Architecture/Physical/

Is it about data models/structure?
  --> Architecture/Information-Structure/

Is it about project management/workflows?
  --> Architecture/Project/

Is it about overall system relationships?
  --> Architecture/Layered/

Is it about goals, principles, or drivers?
  --> Architecture/Motivation/

Is it a process flow?
  --> Processes/Level-[1|2]/

Is it a standard to enforce?
  --> Standards/[Mandated|Recommended|Emerging]/

Is it a Claude Code / OpenClaw skill?
  --> Skills/bf-<skill-name>/

Is it governance/authority?
  --> Governance/

Doesn't fit anywhere?
  --> Consider creating new viewpoint!
```

## Adding New Viewpoints

### Governance: Creating New Viewpoints

Create a new viewpoint only when ALL criteria are met:
1. **3+ documents** share a common architectural concern
2. **Don't naturally fit** any existing viewpoint
3. **Documented justification** referencing ArchiMate standard or clear organizational need

**Requires Architecture Council approval.**

### How to Add a Viewpoint

1. **Research:** Check [ArchiMate viewpoint reference](https://sparxsystems.com/resources/tutorials/archimate/)
2. **Create folder:** `Architecture/[Viewpoint-Name]/`
3. **Add README:** Use template below
4. **Document decision:** Update Architecture/README.md viewpoint table
5. **Submit PR:** With clear rationale

### Viewpoint README Template

```markdown
# [Viewpoint Name] Viewpoint

## ArchiMate Definition
[Quote from ArchiMate standard if applicable, or describe purpose]

## Our Usage
[How BFI specifically uses this viewpoint]

## Related Viewpoints
- **[Other Viewpoint]:** [How they relate]

## Documents in This Viewpoint
- [Document 1](filename_viewpoint.md) - Brief description

## Common Patterns
[Optional: Recurring themes or patterns in this viewpoint]
```

## Promoting Standards

Standards naturally evolve through maturity levels:

1. **Emerging -> Recommended:** After proven in 2+ bots
2. **Recommended -> Mandated:** Architecture Council decision when critical for consistency

Document promotions in PR with usage evidence.

## Change Control

All structural changes follow this process:

1. Create feature branch
2. Make changes
3. Submit PR with rationale
4. Architecture Council review and approval
5. Merge

## Quality Checklist

Before submitting PR for new/moved documents:

- [ ] Naming follows convention (`topic_viewpoint.md`)
- [ ] Viewpoint README updated with new document
- [ ] Main README updated if new viewpoint added
- [ ] All internal links updated
- [ ] Document metadata complete
