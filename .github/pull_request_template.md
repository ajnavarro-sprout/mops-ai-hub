## What is this?

<!-- One or two sentences: what are you adding or changing, and why? -->

**Type of change**
- [ ] New asset (skill / agent / prompt / context)
- [ ] Update to an existing asset
- [ ] Status change (e.g., `draft` → `reviewed`, `reviewed` → `production`)
- [ ] Repo/docs/template change

**Asset(s) affected:** <!-- e.g., skills/lead-lifecycle/mql-summary -->

---

## Asset checklist

*Skip sections that don't apply to this change.*

**Header / metadata**
- [ ] Frontmatter is complete: `name`, `description`, `owner`, `version`, `status`, `risk`, `requires`, `last_reviewed`, `manual_baseline_minutes`
- [ ] Version bumped (patch = fix, minor = new capability, major = changes how it behaves or what it needs)
- [ ] Entry added to [CHANGELOG.md](../CHANGELOG.md)
- [ ] [CATALOG.md](../CATALOG.md) updated

**Safety**
- [ ] `risk` tier is correct (`read-only`, `write-sandbox`, or `write-prod`)
- [ ] Anything that writes data was tested in **Sandbox**, not production
- [ ] No credentials, API keys, tokens, or personal data in the files
- [ ] No real customer or lead data in examples (use fake data)

**Quality**
- [ ] Description says clearly **when** to use it (trigger phrases), not just what it does
- [ ] Includes 3 to 5 test cases (input and expected behavior) in an `evals/` folder, or notes why not
- [ ] I ran it myself on a real task and it worked as described

---

## Promoting status? (skip if not changing status)

**`draft` → `reviewed`**
- [ ] A peer other than the author has run it and signed off

**`reviewed` → `production`**
- [ ] Test cases pass in Sandbox
- [ ] Domain owner has approved
- [ ] Safe for teams outside MOps to use without hand-holding (clear instructions, no hidden dependencies)

---

## Notes for the reviewer

<!-- Anything specific to look at, known limitations, or follow-ups. -->
