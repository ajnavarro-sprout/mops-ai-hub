# Changelog

Notable changes to the MOps AI Hub, newest first. Each entry gives the date, the asset, its version, and what changed and why. See [CONTRIBUTING](CONTRIBUTING.md#versioning-and-the-changelog) for how versions work.

## 2026-11-11

### Existing skills

- **lifecycle-audit v0.1.0, mql-summary v0.1.0, eli-cmo v0.1.0, instance-context v0.1.0:** added the standard metadata header (owner, domain, version, status, risk, requires, last reviewed, manual baseline minutes). All four are `draft`.
- **eli-cmo** moved from `skills/system-admin/` to `skills/communications-strategy/`, along with its `references/examples.md`. It's executive communication, not system administration.
- **instance-context** moved from `context/` to `skills/system-admin/instance-context/SKILL.md`. It's a skill that generates context, not reference context itself.
- **CATALOG** paths and versions updated to match.

### Hub foundation

- **README** rewritten around the hub's purpose: one shared library, nothing in silos, managed updates. Adds status levels, install paths per tool, and risk tiers.
- **CONTRIBUTING** added: how to add a skill, what each status requires, the QA bar, versioning rules, and what never goes in the repo.
- **CATALOG** added: index of every skill with status, risk, and owner, including planned and not-yet-uploaded skills.
- **Skill template** added at `templates/SKILL_TEMPLATE.md`, with governance fields (owner, version, status, risk, requires, last reviewed, manual baseline minutes) held under `metadata`.
- **Pull request template** added with a checklist for metadata, safety, quality, and status promotion.
- **CODEOWNERS** added with placeholder rules, commented out until GitHub usernames are confirmed.

## How to add an entry

```
## YYYY-MM-DD

### <asset name> vX.Y.Z

- What changed, and why it matters to someone using it.
```
