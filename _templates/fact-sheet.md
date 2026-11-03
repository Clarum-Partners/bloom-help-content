---
page:              # UI page name
route:
roles: []          # roles with access (school-owner | hr | direct-report | supervisor)
publish: false     # internal only, never published
extracted_from_commit:   # Bloom repo commit hash at extraction time
---

# {Page name}: fact sheet

> Internal source material. Never published. Quote UI strings exactly. Cite a source file for every fact.

## Route

- `{route}` (source: `{path}`)

## Roles

| Role            | Access (none / view / full) | Source   |
|-----------------|-----------------------------|----------|
| `school-owner`  |                             | `{path}` |
| `hr`            |                             | `{path}` |
| `direct-report` |                             | `{path}` |
| `supervisor`    |                             | `{path}` |

## Source files

- `{path}`: {what it covers}

## Actions

| UI label (exact) | What it does | Roles | Source |
|------------------|--------------|-------|--------|
| **{Label}**      |              |       | `{path}` |

## Fields

| Field label (exact) | Required? | Validation rules | Source |
|---------------------|-----------|------------------|--------|
|                     |           |                  | `{path}` |

## Statuses

| Status (exact) | Meaning | When it changes | Source |
|----------------|---------|-----------------|--------|
|                |         |                 | `{path}` |

## Empty states

- "{Exact text}" (when: {condition}; source: `{path}`)

## Messages

| Type    | Exact text | Trigger | Source |
|---------|------------|---------|--------|
| Success |            |         | `{path}` |
| Error   |            |         | `{path}` |

## Role differences

- {What differs between roles; say explicitly when visibility is unconfirmed.}

## Flags and open questions

- **Feature flags / unreachable / dead code:** {item} (source: `{path}`)
- **TODO (Pat):** {question}
