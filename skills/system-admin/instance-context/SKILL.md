---
name: instance-context
description: "Generate or update a Marketo Instance Context document by pulling live data from the Marketo API. Use this skill whenever the user asks to 'generate instance context', 'update context', 'pull instance data', 'map my Marketo instance', 'build a context doc', 'refresh the context file', 'document my Marketo setup', 'create an instance context', or 'update-context'. Also trigger when the user says things like 'what does my Marketo instance look like', 'map out my folders and programs', 'I need a context file for my instance', 'generate the instance doc', or 'pull everything from Marketo into a doc'. This skill reads existing context (if any) to preserve manual notes, then pulls fresh data from the Marketo MCP to build a comprehensive reference document including QA instructions."
metadata:
  owner: AJ Navarro
  domain: system-admin
  version: '0.1.0'
  status: draft
  risk: read-only
  requires: 'Marketo MCP'
  last_reviewed: '2026-11-11'
  manual_baseline_minutes: 'TBD'
---
 
# Instance Context Generator
 
Build or refresh a Marketo Instance Context document — a single reference file that captures your full Marketo setup: folder structure, program templates, channels, tokens, lead fields, tags, and QA instructions. Other skills (like `prepare-email-rp`) read this document before doing any work, so keeping it accurate matters.
 
## How It Works
 
1. **Read** any existing Instance Context file to preserve manual edits
2. **Pull** fresh data from the Marketo API via MCP tools
3. **Merge** — API data overwrites stale values; hand-written notes and `[NOT CONFIGURED]` sections are kept
4. **Write** the final markdown file to the user's workspace folder
---
 
## Before You Start
 
Check the user's workspace folder for an existing Instance Context file. Look for files matching `*Context*` or `*context*` with `.md` extension. If one exists, read it — you'll merge into it rather than starting from scratch.
 
If no file exists, you're generating from scratch. That's fine — use the template structure below.
 
---
 
## Data Collection
 
Pull data from Marketo in this order. Each step maps to a section of the output document. Make calls in parallel where possible to save time.
 
### Batch 1 — Structure & Channels (run in parallel)
 
| API Call | What It Returns | Maps To |
|---|---|---|
| `browse_folders` (root, maxDepth: 5) | Full folder tree | Section 2 — Folder Structure |
| `browse_channels` (maxReturn: 200) | All channels with statuses | Section 4 — Channels |
| `browse_tag_types` (maxReturn: 200) | Tag types and allowed values | Section 11 — Tags |
| `describe_lead` | All lead fields with metadata | Section 7 — Lead Fields |
 
### Batch 2 — Programs & Templates (run in parallel)
 
| API Call | What It Returns | Maps To |
|---|---|---|
| `browse_programs` (maxReturn: 200) | All programs — infer naming conventions from these | Sections 2, 3 |
| `browse_lists` (maxReturn: 200) | Static lists — find suppression/compliance lists | Section 7 |
 
### Batch 3 — Template Deep-Dive
 
For each program template discovered in the folder tree (look inside the Templates folder):
 
| API Call | What It Returns | Maps To |
|---|---|---|
| `get_program_by_id` (id) | Full program details | Section 2 — Template table |
| `get_tokens_by_folder` (id, folderType: "Program") | Tokens on each template | Section 5 — Token Standards |
| `get_folder_content` (program folder id) | Assets inside the template (emails, smart campaigns, forms) | Section 6 — Email Config |
 
For each email asset found inside a template program:
 
| API Call | What It Returns | Maps To |
|---|---|---|
| `get_email_by_id` (id) | Email metadata including from/reply-to | Section 6 — Sender Info |
| `get_email_content` (id) | Email sections and token references | Section 6 |
 
For each smart campaign found inside a template program:
 
| API Call | What It Returns | Maps To |
|---|---|---|
| `get_smart_campaign_by_id` (id, includeFlowSteps: true) | Campaign config + flow | QA Instructions |
 
---
 
## Merging With an Existing File
 
When an existing Instance Context file is present:
 
- **API-sourced sections** (folders, channels, tokens, lead fields, tags): Replace with fresh data. These go stale and the API is the source of truth.
- **Manual/config sections** (Instance Details timezone, sender info, scheduling rules, compliance, audit severity, integration notes): Preserve exactly as-is. These contain human decisions.
- **`[NOT CONFIGURED]` values**: Keep them — they're placeholders for the user to fill in.
- **`NEEDS TEAM INPUT` notes**: Keep these callout blocks. They signal open decisions.
- **Sections the user added manually**: If the existing file has sections not in the template (e.g. a "Custom Workflows" section), preserve them at the end.
---
 
## Output Document Structure
 
Generate the document using the sections below. Use the exact heading structure so other skills can find sections by number.
 
### Section 1 — Marketo Instance Details
 
A settings table. On first generation, fill what you can detect and mark the rest `[NOT CONFIGURED]`:
 
```markdown
## 1. Marketo Instance Details
 
| Setting | Value |
|---|---|
| Instance URL | <from API base URL if detectable, otherwise [NOT CONFIGURED]> |
| Workspace(s) | <from browse_folders workspace param> |
| Lead Partition(s) | Default |
| Primary Timezone | [NOT CONFIGURED] |
```
 
### Section 2 — Folder Structure
 
Build a tree from `browse_folders` output. Focus on the AI-managed folder (the one containing Templates and Active Marketing Programs). Use this format:
 
```
Marketing Activities / Default / <AI Folder>/ (ID: XXXX)
├── 0. Templates/ (ID: XXXX)
└── 1. Active Marketing Programs/ (ID: XXXX)
    ├── 1. Webinars/ (ID: XXXX)
    ├── 2. Events/ (ID: XXXX)
    └── 3. Emails/ (ID: XXXX)
```
 
Include two sub-tables:
 
**Program Templates** — one row per template program found in the Templates folder:
 
| Template Name | Type | Program ID | Channel |
|---|---|---|---|
 
**Destination Folders by Program Type** — map each template type to its year folder:
 
| Program Type | Destination Folder Pattern | Current Year Folder ID |
|---|---|---|
 
### Section 3 — Naming Conventions
 
Infer naming patterns from existing programs returned by `browse_programs`. Group by program type and extract the pattern. Flag with `NEEDS TEAM INPUT` if the patterns are ambiguous.
 
### Section 4 — Channels & Program Statuses
 
One row per channel from `browse_channels`. Include: channel name, applicable program type, statuses in order, and which statuses are marked as success. Show the key channels in the main table and note the total count.
 
### Section 5 — Token Standards
 
For each template program, list its tokens from `get_tokens_by_folder`:
 
| Token Name | Type | Default Value |
|---|---|---|
 
### Section 6 — Email Program Template Configuration
 
Document the clone-and-configure workflow based on the template programs found. Include:
- Step-by-step "How to Create an Email" instructions
- Default sender info from `get_email_by_id` (From Name, From Email, Reply-To)
### Section 7 — Lead & List Configuration
 
From `describe_lead`, group notable custom fields by purpose:
- Lifecycle & Scoring fields
- Stage Date fields
- Firmographic & Segmentation fields
Only include custom fields (those ending in `__c` or clearly custom). There will be hundreds — select the most operationally relevant ones and note the total count.
 
Include a Suppression & Compliance Lists table from `browse_lists` (look for lists with names suggesting suppression, blocklist, compliance, etc.).
 
### Section 8 — Salesforce Integration
 
Preserve from existing file. If generating fresh, use `[NOT CONFIGURED]` values.
 
### Section 9 — Scheduling & Operations Rules
 
Preserve from existing file. If generating fresh, use `[NOT CONFIGURED]` values.
 
### Section 10 — Audit & QA Standards
 
Preserve from existing file. If generating fresh, use `[NOT CONFIGURED]` values.
 
### Section 11 — Tags & Reporting
 
From `browse_tag_types`, list all tag types with required status and applicable program types.
 
### Section 12 — Integration Notes
 
Preserve from existing file. If generating fresh, use `None`.
 
### Section 13 — QA Instructions
 
This is the detailed QA section. See below.
 
---
 
## Section 13: QA Instructions
 
This section is critical. It tells the AI (and humans) exactly what can be verified programmatically through the Marketo API and what requires a human to check manually. Organize it by program type, but only for types that have templates configured.
 
### Structure
 
```markdown
## 13. QA Instructions
 
*Split into two categories: checks that can be performed via the Marketo API, and checks that require manual human review.*
 
### API-Verifiable Checks (Automated)
 
These checks can be run programmatically using the Marketo MCP tools after a program is created or modified.
 
#### All Program Types
 
| # | Check | API Tool | What to Verify |
|---|---|---|---|
| 1 | Program exists | `get_program_by_id` | Returns valid program data, not an error |
| 2 | Correct folder location | `get_program_by_id` | `folder.value` matches the expected destination folder ID |
| 3 | Program naming convention | `get_program_by_id` | `name` matches the pattern from Section 3 |
| 4 | Channel assignment | `get_program_by_id` | `channel` matches the expected channel for this program type |
| 5 | Program tags set | `get_program_by_id` | Check that required tags from Section 11 are present (if any are marked required) |
| 6 | Tokens configured | `get_tokens_by_folder` (id, "Program") | All tokens have values ≠ their template defaults (no "REPLACE" or placeholder values left) |
 
#### Email Programs
 
| # | Check | API Tool | What to Verify |
|---|---|---|---|
| 7 | Email asset exists | `get_folder_content` (program folder) | At least one email asset is present inside the program |
| 8 | Email approved | `get_email_by_id` | `status` = "approved" |
| 9 | Email subject line set | `get_email_by_id` | `subject.value` is not empty and not a placeholder |
| 10 | From name configured | `get_email_by_id` | `fromName.value` is populated |
| 11 | From email configured | `get_email_by_id` | `fromEmail.value` is populated |
| 12 | Email content sections populated | `get_email_content` | All editable sections have non-placeholder content |
| 13 | Smart list / audience defined | `get_program_smart_list` (id, includeRules: true) | At least one rule exists in the program's smart list |
 
#### Webinar Programs
 
| # | Check | API Tool | What to Verify |
|---|---|---|---|
| 7 | Webinar date token set | `get_tokens_by_folder` | `{{my.Webinar Date}}` is a future date, not the template default |
| 8 | Webinar name token set | `get_tokens_by_folder` | `{{my.Webinar Name}}` ≠ template default |
| 9 | Smart campaigns exist | `browse_smart_campaigns` (folder) | Expected campaigns (invited, registered, attended, etc.) are present |
| 10 | Flow steps configured | `get_smart_campaign_by_id` (includeFlowSteps: true) | Each campaign has at least one flow step |
 
#### Event Programs
 
| # | Check | API Tool | What to Verify |
|---|---|---|---|
| 7 | Event date token set | `get_tokens_by_folder` | `{{my.Event Date}}` is a future date, not the template default |
| 8 | Event name token set | `get_tokens_by_folder` | `{{my.Event Name}}` ≠ template default |
| 9 | Smart campaigns exist | `browse_smart_campaigns` (folder) | Expected campaigns are present |
| 10 | Flow steps configured | `get_smart_campaign_by_id` (includeFlowSteps: true) | Each campaign has at least one flow step |
 
### Manual Checks (Human Review Required)
 
These cannot be verified via the API and require a human to review in the Marketo UI or a rendered preview.
 
#### Visual & Content Review
 
| # | Check | Why It's Manual |
|---|---|---|
| 1 | Email renders correctly across clients | The API returns raw HTML — it cannot simulate how Outlook, Gmail, Apple Mail, etc. render it. Use Litmus, Email on Acid, or Marketo's preview. |
| 2 | Images load and display properly | The API can confirm image URLs exist in the HTML, but not whether they render at the correct size, aren't broken, or look right in context. |
| 3 | Personalization tokens render correctly | The API shows `{{lead.FirstName}}` in the markup. Only a preview or sample send with a real lead shows whether fallback values work and formatting is correct. |
| 4 | Mobile responsiveness | No API for rendering. Requires a device preview or sample send to a mobile client. |
| 5 | Copy tone, grammar, and brand voice | The API returns text content, but quality judgments need human review. |
| 6 | Landing page content and design | Landing pages are referenced by URL in emails. The API cannot render or validate external page content. |
 
#### Audience & Targeting Review
 
| # | Check | Why It's Manual |
|---|---|---|
| 7 | Smart list logic is correct (not just present) | The API can confirm rules exist, but whether "Country is USA AND Job Title contains VP" is the *right* audience requires business judgment. |
| 8 | Suppression lists are appropriate | The API can check if a list is referenced, but whether it's the *right* suppression list for this send is a business decision. |
| 9 | Send volume is acceptable | Requires running the smart list in Marketo UI to see estimated lead count. The API has no "count matching leads" endpoint. |
| 10 | Segmentation / dynamic content targeting | Dynamic content assignments can be partially read via API, but verifying they show the right content to the right segments requires UI preview. |
 
#### Scheduling & Compliance Review
 
| # | Check | Why It's Manual |
|---|---|---|
| 11 | Send date/time is appropriate | The API can read a scheduled run time, but whether Tuesday at 10am PST is right for *this* audience is a human call. |
| 12 | Timezone alignment with audience | Business decision — no API validation possible. |
| 13 | Frequency / fatigue rules respected | Requires checking communication limits and recent sends to the same audience in Marketo UI. |
| 14 | Legal compliance (CAN-SPAM, GDPR, CASL) | The API can check if unsubscribe links exist in HTML, but full compliance (physical address, consent basis, opt-out mechanism) needs human verification. |
| 15 | A/B test configuration (if applicable) | A/B setup is configured in the Marketo UI and not fully exposed via API. |
```
 
Adapt this template based on what you actually find in the instance:
- Only include program-type sections for types that have templates in the Templates folder
- If a template has additional tokens beyond the basics, add matching QA checks
- If the instance has forms on landing pages, add form-specific checks (fields present via `get_form_fields`, thank you page via `get_thank_you_page`)
---
 
## Writing the Output
 
1. **Filename**: `Instance_Context.md` (or match the existing file's name)
2. **Location**: Save to the user's workspace folder
3. **Header**: Start with `# MARKETO INSTANCE CONTEXT` and a note about the generation date
4. **Format**: Use markdown tables for structured data, code blocks for folder trees, and blockquotes for callouts
Add this note at the top of the file:
 
```markdown
*This document is read by AI skills before every workflow to ensure they follow your exact setup. Edit any section directly. Sections marked [NOT CONFIGURED] will prompt the AI to ask at runtime.*
 
> **Last refreshed from API:** YYYY-MM-DD
```
 
---
 
## After Generation
 
Present a short summary to the user:
- How many sections were updated vs. preserved
- Any `[NOT CONFIGURED]` sections that need their input
- Any `NEEDS TEAM INPUT` flags
- Link to the output file
Don't dump the entire document into chat — it's long. Just give the highlights and let them open the file.
