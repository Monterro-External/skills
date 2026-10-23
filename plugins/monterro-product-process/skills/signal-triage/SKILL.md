---
name: signal-triage
disable-model-invocation: true
description: Sweep the Triage state of the product team and the For triage lines of every synthesis report, and turn each problem into one typed, tiered ticket embryo in backlog with the signal preserved, or merge it into the ticket that already exists, or close it against a non-goal or a release note. The only skill that moves a ticket out of triage.
---

# signal-triage

Two queues go in: the issues someone filed straight into the tracker's Triage state, and the findings the synthesis reports marked for triage. One board comes out, where every problem has exactly one embryo, typed and tiered, in `backlog`, with the customer's words and the evidence links on it. Completeness is enforced at grilling, never here; an embryo with an Intent, a verbatim signal, the fields and the links is finished.

Paths and the tracker come from `.claude/monterro.json`: `tracker` (team or project, states, labels, the tier label group or prefix); templates from `templates`; non-goals and product areas from `context_file`; vocabulary from `glossary`; the decision records from `decisions` (default `strategy/decisions`); release notes from `releases` (default `releases/release-notes`); reports from `synthesis` (default `discovery/synthesis`). If the config is missing, say so in one sentence and stop. Where the product side is several teams, the person names the team; never hard-code it.

## 1. Read

1. `context_file`: the product-area taxonomy, the non-goals, the segments. Then `glossary` if it resolves; if it does not, say so in the closing note.
2. The decision records and every release note from the last twelve months. Read its `README.md` index, then the PDRs in the product areas at hand; only `accepted` PDRs decide anything, a `proposed` one is reported as pending.
3. The tier templates.
4. Every issue in the team's or project's `triage` state, fully, with attachments and comments. If `tracker.states.triage` is null, the project has no such state: skip this queue and say so in the closing note.
5. Every report in the synthesis folder: its **For triage** lines and the sections they point at, and the ledger's corroborated rows and observations. Skip a report's did-nothing, one-account, one-voice, heard-once and watching items; those are not triage material and the report said so.
6. The board: every issue in every state except `done` and `canceled`, by title, description and links. Read-only until step 5.

Nothing in the raw signal folders. The reports read the signals; this skill reads the reports.

## 2. The queue, one row per item

In your working notes, one row per Triage issue and per For triage finding: source (issue id, or report file and heading); the problem in one line, what the customer sees happen or cannot do, never the feature name; the ids and files the source cites; accounts and segments; the customer's strongest words, verbatim, with id; **type**: `bug` if the product fails to do what it claims, `feature` if something is missing, `initiative` if it is several capabilities or a one-way door; **tier**: S when one sentence describes it and nobody would argue a product decision, M when there are product decisions to make, L for a one-way door; **product area**, one, from the taxonomy; and the columns below, blank for now: existing ticket, action.

Say the counts: Triage issues, report findings, rows.

## 3. Cluster, then look for the ticket that already exists

Rows describing the same problem are one embryo, whatever channel each came from and however the sources named it. A Triage issue that names a problem a report also names joins that row. Two reports carrying the same finding is one row with both cited.

For every row, search the board on the problem, not the feature name, across all states except `done` and `canceled`. Fill **existing ticket** with the id and state, or none. Then decide the **action**, first match wins:

- **leave**: the existing ticket is in `discovery`, `ready-for-handover` or `in-development`, or carries `needs-more-info`. The row becomes at most a comment on that ticket adding the new evidence link. Never re-tier, re-type or move a ticket past triage.
- **merge**: the existing ticket is in `backlog` or `todo`, or is another Triage issue in this pass. One embryo per problem: the Triage issue is marked duplicate of it with a comment naming the shared problem, and the new evidence is added to the surviving ticket. **Correct the survivor where the clustered evidence contradicts it.** A backlog embryo was written from one signal and is often scoped to it: one customer's notification complaint for a product-wide payment defect, a reporting error for a timezone defect. Where the survivor's type, tier, product area or title says something the evidence does not support, change them and say what changed and why in the same comment. A merge that leaves a wrong scope in place has buried the finding under the ticket that hid it.
- **decline, non-goal**: the problem is inside a non-goal in `context_file` or rejected in the decision records. The line is quoted.
- **decline, fixed**: a release note says it was fixed, and every cited item predates the release. If any cited item is after the release, this is a bug, `keep`, with the release note as evidence that the fix did not take.
- **keep**: a new problem. One embryo, on its own evidence and its own tier. Matching `product_area` with a planted or existing cluster is never a reason to merge; the problem has to be the same.

A row with one source, one account and no report behind it is still kept, as an S or M on one source, with "one source" said in the Problem section. Triage never waits for evidence; grilling does.

## 4. Show the plan

Print the queue as a table, one line per row: source, problem, type, tier, area, existing ticket, action, reason. Below it, the embryos to be created, each as the complete plain-text draft it will become. Then one plain question in chat: "Reply with row numbers to change and what to, or 'ok' to run." Nothing in the tracker changes before the answer. If the session is unattended and nobody can answer, print the plan and stop.

## 5. Act

For every **keep**: create the embryo in the team, in the template for its tier with only the sections that have content, and nothing above the first heading. The Intent is the problem in one sentence. Problem and evidence holds **Original signal (verbatim)**, the customer's words with source, author or id and date, and one evidence line per cited report section, ticket id, review id or file. An S adds the reproduction and the current and expected behaviour where the source gives them, and acceptance scenarios only where the source makes them obvious; otherwise those lines are left for grilling and the closing note says so. An M or L holds Intent and Problem and evidence and says the rest is for discovery. No implementation detail anywhere. Fields per `references/tracker.md`: type label, tier, area label, state `backlog`, `signal_refs` as links. State is `todo` only when the person running the pass says these are for the current sprint. A Triage issue that became the embryo is moved to `backlog` and edited into this shape, its original text kept verbatim under Original signal; it is never recreated under a new id.

For every **merge**: comment on the surviving ticket with the new source and evidence link, mark the Triage issue as duplicate of it. For every **leave**: comment with the evidence link, nothing else. For every **decline**: move the Triage issue to `canceled` with a comment quoting the non-goal, decision or release note; a report finding declined the same way gets a line in the closing note and no ticket. **Then name the twins.** Search the board for tickets in `backlog` or `todo` carrying the same declined problem and list them in the closing note under Declined twins on the board, with their ids and the same quoted line. Do not move them: they may predate the decision or belong to someone. Declining the queue item and leaving four copies in backlog is how a non-goal comes back.

Vocabulary from the glossary: existing terms, never renamed, never defined in the ticket. A domain term from the signal that the glossary lacks stays in the verbatim signal and goes in the closing note as a glossary gap; defining it under Proposed terms is discovery's work, and the grill fails the ticket until it is done.

## 6. Check

Against `references/checklist.md` for every created or edited embryo, and:

- no ticket in `discovery`, `ready-for-handover` or `in-development` changed state, tier, type or labels
- every problem on the board has one embryo; every merged issue names the survivor
- every embryo has the verbatim signal, at least one evidence link, type, tier, area and state
- every decline quotes its line, and its twins in backlog are listed, not moved
- every merge that changed the survivor's type, tier, area or title says so in the comment, with the evidence
- no embryo states a solution as a requirement, and no report's did-nothing or one-voice item became a ticket

## 7. Close

```
Triage pass: <team>   Queue: <triage issues> + <report findings> = <rows>   Created: <n>   Merged: <n>   Left: <n>   Declined: <n>
Created: <id · type · tier · area · problem>, one line each
Declined: <source · reason · line quoted>, one line each
Declined twins on the board: <id · the same problem, left where it is>, one line each
Re-scoped on merge: <id · what changed · why>, one line each
Missing before grilling: <per created S, what the source did not give: reproduction, acceptance scenarios>
Glossary gaps: <id · term the glossary lacks>, one line each, or none
```
