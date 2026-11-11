# Contributing to the MOps AI Hub

Thanks for adding to the hub. This guide covers how to add something, what "done" looks like at each stage, and how changes get reviewed.

## Before you start

- **Check the [Catalog](CATALOG.md)** to see whether something similar exists. If it does, improve it instead of adding a near-duplicate.
- **Pick the right type.** A one-off message to paste is a *prompt*. A reusable playbook the AI loads automatically is a *skill*. A multi-step assistant with tools is an *agent*. Reference material is *context*.
- **Not sure it's ready?** Add it as `draft`. Drafts are welcome; they just aren't recommended for others yet.

## Add a new skill

1. **Pick a domain folder** under `skills/` (for example `lead-lifecycle` or `campaign-ops`). If nothing fits, propose a new folder in your pull request.
2. **Create the skill folder** and copy the template into it:
   - Copy [`templates/SKILL_TEMPLATE.md`](templates/SKILL_TEMPLATE.md) to `skills/<domain>/<skill-name>/SKILL.md`
   - The folder name must match the `name` in the header (kebab-case)
3. **Fill in the header.** Start at `status: draft` and `version: '0.1.0'`. Set an honest `manual_baseline_minutes`; it's how we measure hours saved.
4. **Write the skill** for the AI as the reader: be specific, document known traps, and say what *not* to do.
5. **Add test cases** in `evals/cases.md` (see [QA bar](#the-qa-bar)).
6. **Update the [Catalog](CATALOG.md) and [Changelog](CHANGELOG.md).**
7. **Open a pull request.** The template has a checklist. The domain owner reviews.

A skill folder looks like this:

```
skills/<domain>/<skill-name>/
├── SKILL.md           required
├── evals/
│   └── cases.md       required before `reviewed`
└── references/        optional: examples and longer reference docs
```

### Not using Git? Use the GitHub website

You don't need to clone anything. On the repo page, choose **Add file → Create new file** (or **Upload files**), type the full path such as `skills/lead-lifecycle/my-skill/SKILL.md`, and at the bottom choose **Create a new branch for this commit and start a pull request**. That opens the review for you.

## Agents, prompts, and context

- **Agents** go in `agents/<agent-name>/`. Include its instructions, the tools it needs, and what it may and may not change.
- **Prompts** go in `prompts/`, one file per prompt, with a short header (what it's for, owner, status).
- **Context** goes in `context/`. Write facts and standards, not opinions, and note who keeps it current and when it was last checked.

Agent, prompt, and context templates will be added later. Until then, use the same header fields as skills (`owner`, `version`, `status`, `risk`, `last_reviewed`).

## Statuses: what each one requires

| Status | What it means | To reach it |
|---|---|---|
| `planned` | Idea logged | An entry in the Catalog |
| `draft` | Works for its builder | Header complete, you've run it on a real task |
| `reviewed` | Safe for the MOps team | A peer other than the author has run it and signed off, and test cases exist |
| `production` | Safe to share with other teams | Test cases pass in Sandbox, the domain owner approves, and a newcomer can use it without hand-holding |

Change the `status` in the header and in the Catalog in the same pull request that earns it.

## The QA bar

"QA'd" has a concrete meaning here: the skill ships with **3 to 5 test cases** in `evals/cases.md`, and they pass.

Each case has:

- **Input:** what you'd give the AI (use fake or sanitized data)
- **Good result includes:** what must be present or true
- **Failure looks like:** what would count as wrong

At least one case should be an **edge case**, such as missing data, an unknown ID, or a request that should be refused. Run cases against **Sandbox**, never production.

## Risk tiers

Every skill and agent declares a `risk` tier in its header:

| Tier | What it may do | Review needed |
|---|---|---|
| `read-only` | Reads data, never changes anything | Standard review |
| `write-sandbox` | Changes data, tested and run in Sandbox only | Standard review, plus Sandbox test results |
| `write-prod` | Changes production | Domain owner approval, plus the maintainer for anything that sends, deletes, or activates |

If you aren't sure which tier applies, choose the more restrictive one.

## Versioning and the changelog

Use `MAJOR.MINOR.PATCH` in the `version` field:

- **Patch** (`1.0.1`): wording fix, typo, clearer example. Behavior is unchanged.
- **Minor** (`1.1.0`): new capability or step. Existing use still works.
- **Major** (`2.0.0`): changes what it does, what it needs, or how it's invoked. People may need to update how they use it.

Every change gets a line in [CHANGELOG.md](CHANGELOG.md): date, asset, version, and one sentence on what changed and why. Update `last_reviewed` whenever the owner confirms the asset still works.

## Review and ownership

- Every change goes through a pull request.
- [CODEOWNERS](.github/CODEOWNERS) requests the owner of the domain folder automatically.
- Authors can't approve their own pull requests.
- Owners are responsible for keeping their domain's assets current and deciding when something is ready for `production`.

## What never goes in the repo

- **Credentials:** passwords, API keys, tokens, client secrets, or Marketo/Salesforce connection details
- **Real lead or customer data:** names, emails, phone numbers, or notes from real records. Use fake data in examples
- **Anything you wouldn't want another team to read.** Once something is `production`, assume it may be shared outside MOps

If you commit a secret by mistake, tell the maintainer right away. Deleting the file isn't enough, because it stays in the history and the credential must be rotated.

## Updating or retiring something

- **Updating:** open a pull request, bump the version, and add a changelog entry.
- **Retiring:** mark it as retired in the Catalog, note it in the Changelog with the reason and a replacement if there is one, then remove the files. Git history keeps the old version.

## Questions

Ask in the MOps channel or open an issue on this repo.
