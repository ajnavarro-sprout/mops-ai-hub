# Catalog

Every asset in the hub, with its status, risk, and owner. Find something here first; add yours when you contribute.

*Last updated: 2026-11-11*

**Status:** `planned` → `draft` → `reviewed` → `production` (see [CONTRIBUTING](CONTRIBUTING.md#statuses-what-each-one-requires))
**Risk:** `read-only` · `write-sandbox` · `write-prod` (see [risk tiers](CONTRIBUTING.md#risk-tiers))

## Skills

### In the repo

| Skill | Domain | What it does | Status | Risk | Owner | Version |
|---|---|---|---|---|---|---|
| [lifecycle-audit](skills/lead-lifecycle/lifecycle-audit/SKILL.md) | lead-lifecycle | Audits a Marketo lead lifecycle end to end: stage model, score thresholds, flow integrity, and funnel continuity. Produces a report and workbook. | `draft` | `read-only` | AJ Navarro | n/a |
| [mql-summary](skills/lead-lifecycle/mql-summary/SKILL.md) | lead-lifecycle | Turns a lead's Marketo activity into a short brief a salesperson can act on in 30 seconds. | `draft` | `read-only` | AJ Navarro | n/a |
| [eli-cmo](skills/system-admin/eli-cmo/SKILL.md) | system-admin | Translates technical MOps work into business-outcome language for a CMO or executive audience. | `draft` | `read-only` | AJ Navarro | n/a |
| [instance-context](context/instance-context.md) | system-admin | Builds a Marketo instance reference document (folders, channels, tokens, fields, QA steps) from the API. | `draft` | `read-only` | AJ Navarro | n/a |

*Version shows `n/a` until each skill gets the [standard header](templates/SKILL_TEMPLATE.md). `eli-cmo` fits better under `communications-strategy`, and `instance-context` is currently filed under `context/` but belongs in `skills/`. Both moves are pending.*

### Built, waiting to be added

| Skill | Domain | What it does | Status | Risk | Owner |
|---|---|---|---|---|---|
| field-audit | data-management | Audits Marketo lead fields for AI-readiness: usage inventory, retirement candidates, and a consolidation plan. | `draft` | `read-only` | AJ Navarro |
| marketo-dependency-graph | system-admin | Maps what a Marketo asset depends on and what depends on it, as an interactive graph with risk findings. | `draft` | `read-only` | AJ Navarro |
| marketo-concierge | system-admin | A guided control panel for browsing and running Marketo tools, with a Production/Sandbox toggle. | `draft` | `write-prod` | AJ Navarro |
| sprout-email-performance | analytics-attribution | Analyzes a Marketo email performance export and produces diagnostics and recommendations. | `draft` | `read-only` | AJ Navarro |

### Planned

| Skill | Domain | Status | Owner |
|---|---|---|---|
| marketo-program-build | campaign-ops | `planned` | TBD |
| campaign-reporting | analytics-attribution | `planned` | TBD |
| opt-out-management | compliance | `planned` | TBD |
| list-import | data-management | `planned` | TBD |
| lead-scoring | lead-lifecycle | `planned` | TBD |
| integration-error-resolution | system-admin | `planned` | TBD |

## Agents

No entries yet.

## Prompts

No entries yet.

## Context

No entries yet.

---

**Adding or changing an asset?** Update its row in the same pull request and add a line to the [Changelog](CHANGELOG.md).
