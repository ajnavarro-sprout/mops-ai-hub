# Changelog

Notable changes to the MOps AI Hub, newest first. Each entry gives the date, the asset, its version, and what changed and why. See [CONTRIBUTING](CONTRIBUTING.md#versioning-and-the-changelog) for how versions work.

## 2026-11-11

### Hub foundation

- **README** rewritten around the hub's purpose: one shared library, nothing in silos, managed updates. Adds status levels, install paths per tool, and risk tiers.
- **CONTRIBUTING** added: how to add a skill, what each status requires, the QA bar, versioning rules, and what never goes in the repo.
- **CATALOG** added: index of every skill with status, risk, and owner, including planned and not-yet-uploaded skills.
- **Skill template** added at `templates/SKILL_TEMPLATE.md`, with governance fields (owner, version, status, risk, requires, last reviewed, manual baseline minutes) held under `metadata`.
- **Pull request template** added with a checklist for metadata, safety, quality, and status promotion.
- **CODEOWNERS** added with placeholder rules, commented out until GitHub usernames are confirmed.

### Existing skills

- `lifecycle-audit`, `mql-summary`, `eli-cmo`, and `instance-context` listed in the catalog as `draft`. They will get the standard header and a version in a follow-up change.

## How to add an entry

```
## YYYY-MM-DD

### <asset name> vX.Y.Z

- What changed, and why it matters to someone using it.
```
