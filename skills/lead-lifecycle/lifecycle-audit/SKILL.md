---
name: lifecycle-audit
description: >
  Audit a Marketo instance's lead lifecycle end to end: the stage model as built, entry-criteria and score-threshold integrity, flow integrity, and whether a person can actually traverse the funnel. Use this skill whenever the user asks to audit the lifecycle, check the lead lifecycle, review the funnel, document lifecycle stages, verify MQL logic, check score thresholds, find dormant stages, or "figure out why nobody is becoming an MQL." Also trigger for "lifecycle audit," "audit my funnel," "is my lifecycle working," "why did this lead not progress," "what writes our lifecycle stages," "check our MQL definition," "are our lifecycle campaigns live," "map my lifecycle," "review our demand funnel," or when the user supplies a sales process document and wants it reconciled against Marketo. This skill is READ-ONLY: it inventories, diagnoses and ranks findings but never activates, edits or deletes a campaign, status or token. It outputs a markdown report plus an Excel workbook.
---

# Lifecycle Audit

The lifecycle is the one piece of Marketo architecture every other program depends on and almost nobody has documented. Stages get defined in a channel, campaigns get built to write them, someone switches three off during a migration, and two years later the funnel report still shows those stages — permanently reading zero. Meanwhile the number everyone quotes in board meetings comes out of one score threshold buried inside a single trigger condition that nobody has looked at since it was set.

To an agent this is worse than undocumented, it is actively misleading. Asked "how many MQLs did we make last month," an agent reads a stage that looks real, returns a number, and is confidently wrong. This skill establishes what the lifecycle actually does — as built, not as designed — and ranks what is broken.

It produces four analyses, kept separate: the **stage model as built**, **threshold and criteria integrity**, **flow integrity**, and **funnel continuity**. Where the user supplies a sales process document, it adds a fifth: a **marketing-to-sales crosswalk**.

## When to use

- Documenting or inheriting an instance whose funnel nobody can explain
- Diagnosing why leads stop progressing, or why a stage reads zero
- Verifying the MQL definition before it is used for targets, comp or forecasting
- Checking lifecycle integrity before letting agents read or act on stage data (AI-readiness, Pillar 1 · Architecture)
- Reconciling Marketo's stage vocabulary against how the sales team actually works

## Operating rules (read first)

- **Read-only. Diagnose, never execute.** Do not call any create, update, activate, schedule or delete tool, even if one is available — and specifically never activate a dormant campaign, however obviously broken its absence looks. A dormant campaign may be off on purpose. End the run by stating plainly that nothing in Marketo was changed.
- **Cite campaign names and IDs throughout.** "`06a.Opportunity Sales Qualified` (1048) triggers on opportunity stage `Nurture`" — never "one campaign has a bad trigger."
- **Separate what you read from what you concluded.** The stage map and criteria tables are as-built fact from the API. Severity, intent and recommendations are your reading. Label them differently and say which is which.
- **Absence of a value is a finding, not a blank.** The API returns real values when they are set, so a `null` attribute or an empty `values: []` is a genuine gap. Quote it verbatim rather than smoothing it over.
- **Check for an Instance Context document first.** Look in the workspace folder for `*Context*.md`. If it has a lifecycle or scoring section, read it and treat it as the *intended* state — the gap between intended and as-built is often the most useful finding in the whole audit.
- **Never recommend a threshold change on architecture grounds alone.** Whether MQL should fire at 100 is a conversion-analysis question needing closed-won data. You can say the threshold is undocumented or inconsistently applied; you cannot say it is wrong.

## Data collection

### Step 1 — Locate the lifecycle

Search `browse_programs` and `browse_folders` case-insensitively, ignoring leading digits and punctuation, for: `lifecycle`, `lead lifecycle`, `demand funnel`, `funnel`, `stages`, `MQL`. Then the same for scoring: `scoring`, `score`, `behavior`, `demographic`, `grading`.

If no lifecycle program exists, stop and report that. An instance running its funnel through scattered campaigns with no owning program is a finding in itself, and the rest of this audit needs re-scoping with the user before it is worth running.

### Step 2 — Pull the vocabulary and the machinery

| Tool | Purpose |
|---|---|
| `get_program_by_id` (lifecycle program) | Program details, and its channel |
| `get_channel_by_name` (that channel) | The authoritative stage list: every progression status, step number, success flag |
| `browse_smart_campaigns` (lifecycle folder, `maxReturn: 200`) | Every campaign with `status` and `isActive` |
| `get_smart_campaign_by_id` (`includeFlowSteps: true`) | What each campaign writes: statuses, field stamps, alerts, routing flags, campaign requests |
| `get_smart_list_by_campaign_id` (`includeRules: true`) | **Entry criteria.** The captive smart list holding triggers and filters |
| `get_tokens_by_folder` (scoring program, `folderType: "Program"`) | The score ladder |
| `get_smart_campaign_used_by` (campaign id) | Dependencies, for the continuity analysis |

Pull criteria *and* flow steps for **every** lifecycle campaign, including the inactive ones. Inactive campaigns are where most findings live, and you cannot tell whether reactivating one is safe without reading it.

`get_smart_list_by_campaign_id` is the call that makes this audit possible and the one easiest to miss. A campaign's smart list is captive — its own ID, invisible to `browse_smart_lists`.

### Step 3 — Reading it correctly

Five traps. Getting these wrong produces a confident, wrong report.

1. **`Requested` is live.** `Active` and `Requested` both mean `isActive: true`. `Requested` additionally means the campaign only runs when another campaign calls it. Never report a Requested campaign as switched off.
2. **A stage existing does not mean anything writes it.** Join the channel status list against the campaigns that write each one. A status whose only writer is inactive is **dormant** and reads zero forever.
3. **Custom filter logic changes the audience.** When `filterMatchType` is `custom`, read `filterCustomRuleLogic` — e.g. `(1 or 2) and (3 or 4 or 5 or 6)`, where the numbers index filters in order. Treating a custom-logic list as plain ALL misreads it completely.
4. **Add / wait / remove is a stopwatch.** A flow that adds someone to a list, waits, then removes them is writing a timestamped activity for a *later* campaign to filter on, not managing a population. Membership will be near zero by design. Never report it as a broken list, and flag any `Member of List` filter pointed at such a list as a real defect.
5. **Compare siblings to confirm a gap.** If one campaign's date-stamp step returns `{{system.date}}` and its sibling returns `null`, the `null` is a genuine gap. This comparison is the cheapest way to distinguish real misconfiguration from an API quirk.

## The four analyses

### 1. Stage model as built

One row per channel status in funnel order, joined to its writer: status, step number, record tier, success flag, writing campaign, whether that campaign is live, and a one-sentence entry condition.

Where the channel uses step numbers to split record types — commonly one step for CRM Leads and a later step for Contacts after conversion — state the convention explicitly. It is not self-evident and it governs how every filter in the instance reads.

Report counts plainly: N statuses, M written by live campaigns, D dormant. The dormant count is the headline number of this audit.

### 2. Threshold and criteria integrity

Extract every score band from the `Data Value Changes` conditions across stage campaigns and lay them end to end. Then check:

- **Gaps** — a score that qualifies for no stage (bands of 65-99 and 101+ leave 100 homeless).
- **Overlaps** — two stages claiming the same score, where which one fires depends on trigger ordering nobody controls.
- **Threshold drift** — the same logical threshold written as different numbers in different campaigns (`greater than 99` in one, `at least 100` in another). Equivalent today, divergent after one edit.
- **Empty required values** — `values: []` or `isError: true`. These campaigns error or never fire.
- **Contradictory filters** — a filter that can never be true alongside its siblings, e.g. requiring a status the trigger just excluded.
- **One-off exclusions in core routing** — entry-gate campaigns carrying single-campaign list exclusions. These are debugging leftovers that suppress records forever.
- **Unreadable-by-design criteria** — a stage gated on a field no campaign in the instance ever writes.

State the thresholds as a single clear line (*MEL = 65-99, MQL = 100+*). It is the most-quoted fact you will produce.

### 3. Flow integrity

Per campaign, per step, in order. What each step writes, and what it fails to write:

- **Null primary attributes** — a Change Data Value with no attribute name does nothing.
- **Missing date stamps** — stage-date fields not being written, or written by some stage campaigns and not their siblings. This breaks velocity reporting silently.
- **Alerts with no email** — a Send Alert with a null Mailing ID. Sales is not being told.
- **Request Campaign pointing at an inactive campaign** — the most common broken handoff in any lifecycle. Chase every request target and check its `isActive`.
- **Uncalled Requested campaigns** — status `Requested` but named in no Request Campaign step anywhere. Dead code.
- **Choice ordering** — Marketo takes the first matching choice. A broad choice above a narrow one makes the narrow one unreachable. Read the order, not just the set.
- **Score resets** — where detour campaigns zero scores, confirm all score fields are reset consistently. Zeroing behaviour but not demographic score leaves a person primed to re-qualify instantly.
- **CRM values that may not exist** — triggers filtering on opportunity or lead status values. Flag any value that looks stale for human confirmation against the CRM picklist; you cannot read the picklist from here.

### 4. Funnel continuity

Trace whether a person can actually get from entry to the end, and back out again. Walk the graph rather than the list:

- **Dead ends** — a stage with no live campaign writing anything downstream of it.
- **One-way doors** — no path back to marketable. Check the recycle and disqualification-reversal paths specifically.
- **Tier asymmetry** — a Lead-tier path that is live and a Contact-tier path that is not, meaning people stop progressing the moment they convert in the CRM.
- **Re-entry** — can someone who was qualified, recycled, and re-engaged qualify again, or does a suppression list hold them out permanently?
- **Compliance exposure** — if disqualification campaigns are inactive, nothing sets marketing suspension. Disqualified people remain marketable. Rank this High: it is a compliance issue wearing an architecture costume.

### 5. Marketing-to-sales crosswalk (when a sales process is supplied)

If the user provides a sales process document, deck or stage list — or one is already in the workspace folder — line the two models up: one row per funnel position, with the Marketo status, its channel step, the sales stage, its CRM label and probability, the owning team, and whether Marketo tracks it at all.

What this reliably surfaces:

- Sales stages with **no Marketo status at all**, and how much of the pipeline value sits in that blind range.
- Marketo campaigns **triggering on CRM stage values absent from the documented process** — usually a legacy stage name, always a campaign that never fires.
- **Vocabulary mismatch** — Marketo speaking BAL/BQL while Sales speaks TQL/SAL, so no single handoff conversion rate can be quoted by either side.
- **Documented gates the lifecycle never reads** — an ICP-fit or budget-confirmed requirement in the sales process that no Marketo criterion checks, meaning marketing hands over leads Sales must reject on grounds marketing never evaluated.

Keep this tab honest about its own status: the Marketo columns are as-built, the sales columns are transcribed from a human document, and the alignment between them is your interpretation.

## Severity

Rank every finding. Use these definitions so the ranking means the same thing run to run:

| Severity | Definition |
|---|---|
| **High** | The funnel is factually broken, a number people rely on is wrong, or there is compliance exposure. Someone should look this week. |
| **Medium** | Works today but fragile, undocumented, or dependent on something nobody owns. Will break on the next edit. |
| **Low** | Hygiene. Debugging leftovers, inconsistent naming, redundant filters. |

Two rules: a dormant stage that appears in reporting is always at least Medium, because the zero it returns is indistinguishable from a true zero. Anything that leaves suppressed or disqualified people marketable is always High.

## Output

Produce two artifacts and save both to the workspace folder.

### Markdown report — `Lifecycle-Audit-YYYY-MM-DD.md`

```markdown
# Lifecycle Audit — [Instance] — YYYY-MM-DD
*Read-only analysis. No campaigns, statuses, tokens or fields were changed.*

## Summary
- Lifecycle program: <name> (ID)  |  Channel: <name> — N statuses
- Campaigns: N total — A live, B inactive
- Score thresholds: <e.g. MEL 65-99, MQL 100+>
- Dormant stages: D of N
- Findings: F total — H high, M medium, L low

## 1. Stage model as built
## 2. Threshold and criteria integrity
## 3. Flow integrity
## 4. Funnel continuity
## 5. Marketing-to-sales crosswalk   (only if a sales process was supplied)
## Findings, ranked
## What could not be checked via API
```

Lead the summary with the thresholds and the dormant count. Those two lines are what gets forwarded.

### Excel workbook — `Lifecycle-Audit-YYYY-MM-DD.xlsx`

Use the **xlsx skill**. Tabs, in this order — the reading order matters, each tab answers the question the previous one raises:

- **Read me** — program identity, channel, thresholds, counts (as formulas over the other tabs, so they stay true if rows are edited), a tab guide, and a legend for any warning marker used
- **Stage map** — status, step, tier, success flag, writing campaign, live?, entry condition in one sentence
- **Campaigns** — id, name, group, status, active, qualification rule, smart list id, flow id, status written, one-line purpose
- **Entry criteria** — one row per trigger and filter: campaign, kind, position, rule, operator, value, note. Include the custom filter logic expression where present
- **Flow steps** — one row per step per choice branch, in order: campaign, step order, action, values written, choice condition, default?
- **Scoring** — the ladder sorted descending within type, with the threshold marked, plus any fast-path override called out separately
- **Sales process** — one row per stage activity or criterion, when a sales document was supplied
- **Funnel crosswalk** — the two models side by side, when a sales document was supplied
- **Findings** — severity, area, finding, evidence with names and IDs, why it matters, suggested action. Sorted severity-first

Mark unset values with a consistent warning glyph and define it on the Read me tab. Freeze header rows, add autofilters, and colour live/dormant consistently across every tab so a reader learns the encoding once.

## After the run

Give the user four things, in this order: the score thresholds, how many stages are dormant, the highest-severity finding with its campaign name and ID, and whether a fast-path override is doing the work the graduated scoring model appears to do.

Then ask whether they want the findings turned into a sequenced remediation plan, or whether to fold the as-built stage model into their Instance Context document (Sections 14 and 15 of the `instance-context` skill are built to receive exactly this). If no sales process document was available, offer the crosswalk as a follow-up — it is usually where the most surprising findings come from.

Confirm explicitly that nothing in Marketo was changed, and that reactivating any dormant campaign remains a human decision.
