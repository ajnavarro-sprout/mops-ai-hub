---
name: eli-cmo
description: >
  Translate marketing operations (MOPs) technical work -- system builds, tool
  integrations, routing/scoring logic, campaign mechanics, data and reporting
  changes -- into the business-outcome language a CMO or executive audience
  cares about. Use this whenever someone is prepping a presentation, QBR,
  board update, demo, or status report for leadership and has technical MOPs
  content that needs translating, even if they don't use the words "CMO" or
  "executive": e.g. "help me explain this to my boss," "make this sound less
  technical," "what's the so-what here," "how do I pitch this win," "translate
  this into business impact," "I need to present this project to leadership,"
  or someone pasting a list of tool changes, integrations, or campaign results
  and asking how to frame it. Always run technical MOPs writeups through this
  translation pass before they go in front of executives -- content that leads
  with tool names, raw metrics, or implementation mechanics needs this first.
---

# ELI-CMO

**Philosophy: it's not about what you know, it's about how you translate what you do.**

Marketing ops people are often excellent at the work and mediocre at explaining
why it matters to someone who doesn't live in the tools. This skill closes that
gap -- not by dumbing anything down, but by re-centering the story on business
outcomes instead of implementation.

## The three pitfalls this skill exists to avoid

1. **Tool-centric and in the weeds.** Leading with product names and platform
   mechanics (LeanData, Outreach, Sendoso, a specific field or workflow) instead
   of what changed for the business. An exec doesn't know or care what LeanData
   is; they care what it did to speed-to-lead.
2. **Simply reporting data.** Reciting a number without interpreting it. "We ran
   70 webinars" is a fact, not an insight. Insight is what the number means and
   what to do about it.
3. **Staying stuck on the how.** Describing the mechanism (how the routing logic
   was built, how the fields were configured) rather than the capability it
   creates and the result that capability drives.

## What to do instead

- **Tie back to OKRs and goals** where they exist -- pipeline, conversion,
  velocity, spend efficiency, retention, whatever the org is actually measured
  on. A translation that doesn't connect to a goal is just a different kind of
  jargon.
- **Deliver insight plus a recommended action**, not just a restated fact. If
  there's a comparison to be made (channel A vs. channel B, this year vs. last
  year, this segment vs. that one), make it and say what it implies.
- **Stay aware of tool-language vs. outcome-language** as you write -- if a
  vendor or platform name is doing load-bearing work in the sentence, that's a
  sign the sentence hasn't been translated yet.
- **Surface investment intelligence.** Where should the next dollar or hour go?
  Translation should help answer "where do we double down."
- **Drive a more / less / different decision.** The best translations leave the
  reader knowing what to do next, not just what happened.
- **Finish the thought.** Every translated statement should answer "by doing
  this, what does it accomplish?" If the sentence stops at the action, it's not
  done yet.

## The core move

For each piece of technical content, work through three questions in order:

1. **What was actually done?** (the technical fact -- keep this internal, it's
   your raw material, not your output)
2. **What capability or change does that create?** (e.g. routing logic ->
   leads reach the right rep with the right urgency; a UTM generator -> clean
   attribution data becomes possible)
3. **So what does that drive, and for whom?** (the business outcome: faster
   conversion, better spend allocation, higher rep productivity, more trustworthy
   reporting -- ideally tied to a real metric or goal)

The output is built from #2 and #3. #1 rarely appears in the final language at
all, and when it does, it's a parenthetical, not the subject of the sentence.

Worked example:

> **Technical fact:** Rebuilt the decision logic in LeanData so leads route to
> the right inbound/outbound BDR using a standard queue.
> **Capability:** MQLs reach the right BDR, with the right SLA, with context.
> **So what:** Improved conversion of MQL to meetings.
> **Translated:** "Improved conversion of MQL to meetings by routing MQLs to
> the right BDR with corresponding SLAs, at the right time, with relevant
> context."

Notice LeanData never appears. The queue mechanics never appear. Only the
capability and the outcome survive.

## Not everything needs translating

If a technical action already reads as business-legible -- e.g. "standardize
channels and statuses and create a campaign naming convention" -- leave it
alone rather than forcing extra language onto it. The test isn't "did I change
the words," it's "would a CMO nod along without needing MOPs context." Some
sentences already clear that bar. Padding a good sentence to prove you
translated it makes the whole output feel less trustworthy, not more.

## Handling metrics and goals

Real numbers make translations land ("every $1 spent on programs returned $10
in MSB, up from $8 last year"). Don't invent them.

- If the input includes numbers, use them -- especially in ratio, rate, or
  comparison form (per-dollar return, conversion rate, YoY change) rather than
  raw totals. A raw total ("70 webinars, 457 MQLs") is a data dump; a rate or
  comparison ("Demo Lab webinars converted at a higher rate than TOFU webinars
  despite fewer registrations") is an insight.
- If the input names a goal or OKR, anchor the translation to it explicitly.
- If neither a metric nor a goal is given and one plausibly exists (the person
  would know it, or could look it up quickly), ask for it before finalizing --
  a specific number is worth a follow-up question. Ask something concrete and
  quick, e.g. "Do you have the before/after conversion rate for this, or a goal
  it ties to?" rather than a generic "what's the context?"
- If they don't have a number on hand, don't block on it -- proceed using
  directional/capability language ("drives faster follow-up on hot leads")
  instead of a fabricated figure, and say so plainly rather than presenting a
  guess as fact.

## Picking the output shape

Look at what was handed in and match the shape of the output to it:

- **A list of discrete technical items** (bullet points, a list of projects,
  a table like "here's what we built this quarter") -> produce a two-column
  table, Geek Speak on the left and C-Suite Speak on the right. See
  `references/examples.md` for the reference format and worked examples.
- **A fuller narrative** (a campaign recap, a project summary, "here's what
  happened with our webinar program this year") -> produce narrative talking
  points: a short sequence of statements, each carrying its own insight and
  "so what," reading like something the person could say out loud in a
  meeting. Group by OKR/theme if multiple things are being translated at once
  rather than listing them in the order they were mentioned.

When unsure which fits, default to whichever shape makes the content easier to
present -- a table for scanning a list of wins, narrative for telling the story
of one initiative or result.

## Common MOPs -> business themes

Useful lenses when figuring out what a technical change actually drives. Not
exhaustive, and a given change may map to more than one:

- Pipeline / meetings generated
- Conversion rate and funnel velocity (MQL->meeting, MQL->SQL, etc.)
- Speed to lead / SLA and rep responsiveness
- Spend efficiency and ROI (return per dollar, cost per outcome)
- Attribution and investment visibility (where dollars are working)
- Personalization and relevance at scale
- Sales and marketing alignment / shared visibility
- Data trust and decision quality

## Self-check before delivering

Before handing back the translation, check it against the failure modes above:

- Does any sentence still lead with a tool or platform name? If so, either cut
  it or demote it to a parenthetical.
- Is there a bare number or list of numbers with no interpretation attached?
  If so, turn it into a rate, comparison, or trend, or add the "so what."
- Does every statement finish the thought -- does it say what the action
  accomplishes, not just what the action was?
- Is there a recommendation or a more/less/different call where the content
  supports one? Not every line needs one, but a full translation pass usually
  surfaces at least one.
- Would this survive being read out loud to a CMO with zero MOPs background?

## Reference examples

`references/examples.md` holds the full set of before/after translations this
skill was calibrated on (routing logic, webinar programs, spend efficiency,
lead scoring, and a six-row geek-speak/C-suite table). Read it before doing a
first translation pass, or whenever the input resembles one of those patterns
closely and you want to match tone precisely.
