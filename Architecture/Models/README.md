# ArchiMate Models

**ArchiMate Viewpoint**: Technology
**Purpose**: Storage location for ArchiMate model files and exported images.

---

## Model Scope Strategy

Each organization maintains **one ArchiMate model** that encompasses all repositories and systems within that organization's scope.

```
1 Organization = 1 Continuum Repo = 1 ArchiMate Model
```

### BFI Model

| Repository | Model File | Scope | Content Type |
|------------|------------|-------|--------------|
| `bot-fleet-continuum` | `bfi-architecture.archimate` | Bot Fleet Inc | All BFI repos, bots, and infrastructure |

### Reference vs Implementation

| Model Type | Contains | Example Elements |
|------------|----------|------------------|
| **Reference** (enterprise-continuum) | Patterns, standards, templates | "Bot Runtime" (pattern), "Cloudflare Worker" (archetype) |
| **Implementation** (bot-fleet-continuum) | Actual deployed instances | "dispatch-bot-vm-411", "botfleet-email-worker" |

---

## Folder Structure

```
Architecture/Models/
+-- README.md                     <-- This file
+-- bfi-architecture.archimate    <-- BFI model (to be created)
`-- exports/                      <-- Exported images for documentation
    `-- {model}_{view}_{date}.{format}
```

---

## Naming Conventions

### Model File Naming

```
{scope}-{type}.archimate
```

| Scope | Type | Example |
|-------|------|---------|
| `bfi` | `architecture` | `bfi-architecture.archimate` |

### Element Naming (Inside Models)

| Element Type | Pattern | Examples |
|--------------|---------|----------|
| **Application Component** | `bfi-{system}-{component}` | `bfi-dispatch-bot`, `bfi-email-worker` |
| **Node** | `{location}-{type}-{id}` | `proxmox-vm-411`, `cf-worker-email` |
| **Technology Service** | `{provider}-{service}` | `cloudflare-workers`, `openclaw-runtime` |
| **Business Process** | `{verb}-{object}` | `dispatch-issue`, `groom-vault` |
| **Business Actor** | `bfi-{role}` | `bfi-dispatch`, `bfi-archi` |

### View Naming (Inside Models)

```
{viewpoint}_{subject}_{detail-level}
```

**Examples**:
- `layered_bot-fleet_overview`
- `technology_openclaw-runtime_detail`
- `implementation_vm-provisioning_overview`
- `physical_proxmox-network_overview`

---

## Workflow

### Creating the BFI Model

1. **Create model** in Archi as `bfi-architecture.archimate`
2. **Create initial views** following view naming convention
3. **Save** to this `Models/` directory
4. **Export images** to `exports/` for documentation

### Adding Views

1. **Open model** in Archi
2. **Create new view** with naming: `{viewpoint}_{subject}_{detail}`
3. **Reuse existing elements** (don't duplicate)
4. **Export images** if needed for documentation

---

## Related Documentation

| Document | Relationship |
|----------|--------------|
| [Architecture/README.md](../README.md) | Viewpoint framework |
| [ORGANIZATION_REGISTRY.md](../../Governance/ORGANIZATION_REGISTRY.md) | Organization details |
| [enterprise-continuum Models/](https://github.com/Oss-Gruppen-AS/enterprise-continuum/tree/main/Architecture/Models) | Reference model and naming standards |
