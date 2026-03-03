# Architecture Viewpoints

This directory organizes architecture documentation using **ArchiMate viewpoints**. Each viewpoint addresses a specific architectural concern.

## What is a Viewpoint?

A viewpoint is a lens through which we examine the architecture. It defines:
- **What to show:** Relevant elements and relationships
- **What to hide:** Irrelevant details for this concern
- **Who cares:** Primary stakeholders for this view

## Current Viewpoints

| Viewpoint | Purpose | Primary Audience | Document Count |
|-----------|---------|------------------|----------------|
| [Layered](Layered/) | System overview and relationships | Architects, Management | 0 |
| [Application-Cooperation](Application-Cooperation/) | How bots and services integrate | Developers, Architects | 0 |
| [Application-Usage](Application-Usage/) | User interaction patterns | UX, Product, Developers | 0 |
| [Technology](Technology/) | Infrastructure, platforms, and bot runtime | DevOps, Architects | 0 |
| [Physical](Physical/) | Equipment, VMs, network topology | Network Engineers, DevOps | 0 |
| [Implementation](Implementation/) | Deployment and operations | DevOps, Operations | 0 |
| [Migration](Migration/) | Architecture transitions | Enterprise Architects | 0 |
| [Information-Structure](Information-Structure/) | Data architecture | Data Engineers, Architects | 0 |
| [Project](Project/) | Project management and workflows | Project Leads, Management | 0 |
| [Motivation](Motivation/) | Goals, principles, drivers | Architecture Council | 0 |

## ArchiMate Models

| Folder | Purpose | Contents |
|--------|---------|----------|
| [Models](Models/) | ArchiMate model files | `.archimate` files and exported images |

The `Models/` folder contains ArchiMate model files (`.archimate`) and exported images for use in documentation. Each continuum repository maintains **one model** covering all systems in that organization's scope.

- **enterprise-continuum**: `enterprise-reference.archimate` (reference patterns)
- **bot-fleet-continuum**: `bfi-architecture.archimate` (BFI implementation)

See [Models/README.md](Models/README.md) for naming conventions and storage strategy.

## How to Use

### Finding Documents
1. **By concern:** Browse viewpoint folders for your area of interest
2. **By filename:** Documents use `topic_viewpoint.md` naming
3. **By viewpoint README:** Each folder has an index of its documents

### Adding Documents
See [CONTRIBUTING.md](../CONTRIBUTING.md) for placement guidelines.

### Creating New Viewpoints
**This list should grow!** When existing viewpoints don't fit your needs:
1. Consult [ArchiMate reference](https://sparxsystems.com/resources/tutorials/archimate/)
2. Follow process in [CONTRIBUTING.md](../CONTRIBUTING.md)
3. Update this README

## Viewpoint Relationships

```
Layered (Overview)
  |--> Application-Cooperation (Integration)
  |--> Application-Usage (User Experience)
  |--> Technology (Infrastructure)
  `--> Physical (Equipment)

Physical (Equipment & Facilities)
  `--> Technology (Software running on equipment)

Technology (Platforms & Software)
  `--> Physical (Hardware it runs on)

Implementation (Deployment & Operations)
  |--> Technology (Platform choices)
  |--> Physical (Equipment deployment)
  `--> Project (Execution timing)

Migration (Transitions)
  |--> Implementation (How to deploy each plateau)
  |--> Physical (Infrastructure changes)
  `--> Project (Migration phases and schedules)

Information-Structure (Data)
  |--> Application-Cooperation (Data exchange)
  `--> Technology (Storage and databases)

Project (Management)
  |--> Implementation (Deployment execution)
  `--> Migration (Transition management)
```

## Missing Viewpoint = Gap

If you're struggling to place a document, it may indicate:
1. **Viewpoint needed:** Create new viewpoint for that concern
2. **Multiple viewpoints:** Document spans concerns (add to most relevant, reference from others)
3. **Wrong level:** Consider if it belongs in Processes/ or Standards/ instead
