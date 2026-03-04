# Bot Fleet Skills — Migrated

> **Skills have migrated to [`enterprise-continuum/Skills/`](https://github.com/Oss-Gruppen-AS/enterprise-continuum/tree/main/Skills).**
> Do not add new skills here.

As of March 2026 (Epic [enterprise-continuum#194](https://github.com/Oss-Gruppen-AS/enterprise-continuum/issues/194)), all Bot Fleet skills (`bf-*`) are maintained in `enterprise-continuum/Skills/` alongside enterprise skills. This consolidation provides a single source of truth for all non-personal skills.

## Migrated Skills

| Skill | New Location |
|-------|-------------|
| `bf-memory-systems` | `enterprise-continuum/Skills/bf-memory-systems/` |

## For Bot Fleet Users

Skills are distributed via the **skillset build pipeline** (second-brain). User/bot configs in `skillset-configs/` control which skills each bot receives. Bot Fleet skills are now sourced from the `enterprise` origin.
