# 🌱 MOPs AI Hub

The shared library for everything Sprout Social's Marketing Operations team builds with AI: **agents, skills, prompts, and context**.

## Why this exists

AI work tends to end up in silos: a prompt in someone's notes, a skill on one laptop, an agent only its builder knows how to run. This hub fixes that.

- **Nothing lives in a silo.** If MOPs built it, it's here, findable, and usable by the whole team.
- **One place to download from.** When we build something that enables another team, this is where they get it.
- **Updates are managed, not emailed.** Every asset has an owner, a version, and a changelog, so people know what changed and whether to update.

## What's inside

| Folder | What it holds | Example |
|---|---|---|
| [`skills/`](skills/) | Packaged instructions that teach an AI how to do one job well, organized by domain | `lifecycle-audit`, `mql-summary` |
| [`agents/`](agents/) | Configurations for AI assistants that combine instructions, tools, and multiple steps | A campaign QA agent |
| [`prompts/`](prompts/) | Copy-and-paste prompts for people who don't use skills or agents | Email QA checklist prompt |
| [`context/`](context/) | Reference material AI needs to do good work, such as definitions, standards, and architecture notes | Naming conventions |

**Skill vs. agent vs. prompt, in one line each:**
- A **prompt** is a message you paste.
- A **skill** is a reusable playbook the AI loads when the task matches.
- An **agent** is an assistant that follows a playbook, uses tools (like Marketo MCP), and works through multiple steps.

## Find something

Browse the **[Catalog](CATALOG.md)** for every asset, its status, owner, and version.

## Use something

Every asset has a status, so you know what you're getting:

| Status | Meaning |
|---|---|
| `planned` | Idea logged, nothing to use yet |
| `draft` | Works for its builder, not yet reviewed |
| `reviewed` | Peer-reviewed and tested; safe for the MOps team |
| `production` | Reviewed, tested, owned, versioned; safe to share with other teams |

**Install a skill:**

- **Claude Desktop / Cowork:** download the skill folder, zip it so `SKILL.md` sits at the top level, then upload it under Settings → Skills.
- **Claude Code:** copy the skill folder into `~/.claude/skills/` (just you) or `.claude/skills/` in a project (shared with that project).
- **Cursor:** copy the skill's instructions into a Cursor rule. Skills that call Marketo or Salesforce tools may need their tool names adjusted.
- **No tooling at all:** use the [`prompts/`](prompts/) folder.

**Download without Git:** on GitHub, open the folder you want and use **Code → Download ZIP** for the whole repo, or grab the skill folder from the latest **Release** once releases are published.

## Safety first

Skills that touch Marketo or Salesforce carry a **risk tier** in their header:

- `read-only`: never changes anything in a system. Safest to run anywhere.
- `write-sandbox`: changes data, but only run against Sandbox.
- `write-prod`: changes production. Needs owner sign-off before use.

When in doubt, run it against Sandbox first.

## Keeping things current

- Each asset carries a **version** (`MAJOR.MINOR.PATCH`) and a **last reviewed** date.
- Changes go through a pull request reviewed by the **domain owner** (see [`CODEOWNERS`](.github/CODEOWNERS)).
- Notable changes are logged in the [Changelog](CHANGELOG.md), so you can see what's new before updating.

## Contribute

Built something useful? Add it. See **[CONTRIBUTING.md](CONTRIBUTING.md)** for the template, the review checklist, and how a `draft` becomes `production`.

## Repo layout

```
mops-ai-hub/
├── README.md
├── CATALOG.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── agents/
├── context/
├── prompts/
├── skills/
│   ├── analytics-attribution/
│   ├── campaign-ops/
│   ├── communications-strategy/
│   ├── compliance/
│   ├── data-management/
│   ├── enablement-intake/
│   ├── integrations/
│   ├── lead-lifecycle/
│   └── system-admin/
└── templates/
```

## Ownership

Skills are owned by domain. The owner for each folder is listed in [`CODEOWNERS`](.github/CODEOWNERS). Owners review changes, keep their assets current, and decide when something is ready for `production`.

# What are Skills?

Skills are markdown files that give AI agents specialized knowledge and workflows for specific tasks. When you add these to your project, your agent can recognize when you're working on a marketing task and apply the right frameworks and best practices.

# What are Agents?

**Agents** are autonomous or semi-autonomous AI assistants configured to perform specific marketing operations functions. Instead of just answering questions like a standard Gemini or Claude prompt, agents have specific personas, can use tools (like reading spreadsheets or pinging APIs), and can string multiple steps together to complete a complex workflow.

Inside the **/agents** folder, you will find the system instructions and tool configurations that bring these assistants to life and govern how they behave.

**How they work together:** You assign a task to an **Agent**. The Agent reads the foundational **Context**, picks up the right **Skill** for the job, and executes the work.

---

*Maintained by the Sprout Social Marketing Operations team.*
