---
name: support-synthesis
disable-model-invocation: true
description: Read a period of the support queue and write the synthesis report, themes by what customers were trying to do, counted by reading rather than by tag.
---

# support-synthesis

A period of the queue goes in, one report comes out, with the table it was built from beside it. The report says what customers were trying to do when they wrote to us, ranked by how many independent customer accounts had the same problem, and names who is missing from each theme. It creates nothing in the tracker.

Paths come from `.claude/monterro.json`: the queue from `support` (default `signals/support`), release notes from `releases` (default `releases/release-notes`), segments and the account list from `context_file`, reports into `synthesis` (default `discovery/synthesis`). If the config is missing, say so in one sentence and stop.

**Reading the queue.** By default, read the export files in the `support` folder directly: JSONL, CSV, or one file per ticket. A queue too large to hold at once is read in batches (step 3). When `support_cli` is set (for example `npm run support --`), use its commands instead (`list`, `show`, `tenants`, `months`, `ids`, and whatever filters it documents). Every count in the report comes from the table you build in step 3, never from a tag.

## 1. Scope

List the months the queue covers, with `months` when there is a CLI or from the ticket dates otherwise. The period is what the person named, or else everything after the last support report in the synthesis folder, or else the whole queue. Say the period and the ticket count in one line before reading.

## 2. Read

1. A README in the signals root or the `support` folder, if one exists: its rules decide what counts.
2. `context_file`: the segments, their ARR shares, the key accounts if it names them, and how each segment tends to raise problems. Where it is silent on any of these, say so in the report and assume nothing.
3. The existing reports in the synthesis folder, for format and so the report says what is new against them.
4. Every release note dated inside or up to twelve months before the period. Each item that says something was fixed or added is a candidate for a theme that should have stopped.

Nothing else. Not the OST, not the tickets folder, not the other signal folders. Those are combined with this report later, by `synthesis` and by people.

## 3. The table, one row per ticket

Read every ticket in the period (`list` with a CLI); the table is built from reading, never from a sample or a pattern match. Write `<produced date>-support-tickets-<period slug>.table.md` in the synthesis folder, a markdown table with one row per ticket: id; account; segment; role; language; month; **intent** in your own words, one line, what the customer was trying to do or what they saw happen, taken from the subject and first sentence and never from the tag; **kind**: `how-to`, `defect`, `request`, `rules` (asking us to confirm how the product behaves), `other`; **theme**: blank for now.

Where the first sentence leaves the intent unclear, read the whole ticket (`show` with a CLI). Do that for every ticket whose intent you cannot state in one line, and for every ticket with three or more messages, because a long thread is the queue's expensive tail and the shape of the problem is in the replies.

**Batches.** When the period will not fit in one reading, split it into batches of one month each, and halve any month that still does not fit. Where the environment offers subagents, give each batch to its own subagent on the Haiku model with the month's ticket ids (or the CLI command that lists them), the README's rules, and this step's column definitions and whole-ticket rule. Each one reads every ticket in its batch and returns its rows and its ticket count: no themes, no clustering, no quotes chosen. Without subagents, run the batches one after another and append each batch's rows to the table file before starting the next, so no batch needs the previous one in view. A batch whose row count falls short of its ticket count is run again before step 4. Intents written in different batches will be worded differently; step 4 clusters on meaning, not wording.

The table is complete when its row count equals the number of tickets in the period. Say both numbers, and the batches when there were any.

## 4. Cluster

Fill the theme column. A **theme** is the same intent from at least two customer accounts. The same intent from one account, however many tickets, is a single voice and goes to **One account**, never promoted and never dropped.

For each theme with five or more tickets, find the accounts that are absent from it: with a CLI, `tenants --ids <the theme's ids>`; without one, the accounts in the table, or in the account list in `context_file`, that have tickets in the period and none in the theme. Keep the list:

- **Absent.** The absent accounts, with segment. Where the theme touches something every account uses, an account with volume in the queue and none in the theme is the control case: name it and say what is different about it, or say that finding out is the first question. Pick the control from a segment that `context_file` says raises problems through support; an account from a segment that raises them elsewhere (a CSM, an account manager) proves little by its absence. Where `context_file` is silent on this, say so and pick the account with the most other tickets.
- **Inverted.** The theme's segment shares against the queue's shares. Where a segment is under its share, say what its silence could hide, and whether `context_file` says that segment under-files.
- **Stopped or continuing.** Where a release note says the thing was fixed, split the theme's tickets by date around the release. Tickets after the release mean the fix did not take, or staff are reporting from memory; the thread says which. Tickets only before it mean the theme is closed and belongs in Already fixed, outside the ranking.
- **Mis-tagged.** Where the export carries tags, the count of the theme's tickets whose tags say something else. Report the rate for the queue as a whole in the opening paragraph. No tags in the export: say so once and skip this line.

Within a theme, separate what the customer **reports happening** from what they **ask to have**. A defect and a request about the same screen are two themes.

## 5. Write the report

File: `<produced date>-support-tickets-<period slug>.md` in the synthesis folder. Frontmatter in the existing reports' shape: `type: synthesis`, `source: support tickets`, `period`, `tickets_read`, `produced`, `by` (the person who asked, if they said who they are; otherwise `unattended run`; never an invented name), `method: /monterro-product-process:support-synthesis over <support folder>, table in <table file>`.

Length: **hard ceiling 1800 words** for the report body, counting from the first heading and excluding the frontmatter and the table file. Count the words before saving and say the count in the closing block. Over the ceiling, demote whole items from the bottom of the ranking to one-line bullets; never trim sentences evenly across the report, which loses the evidence and keeps the padding.

Body, in the existing reports' register:

- **Heading and one paragraph.** Tickets read, period, markets, the mis-tag rate, and that themes are by intent. Say to read it as a picture of the period and not as a backlog.
- **Themes table**, ranked by number of accounts and then tickets: theme, tickets, accounts, segments, roles, months active, kind, confidence.
- **One section per theme worth acting on**, the defects and rules themes first. The finding in two or three sentences. The quotes that carry it, two or three, each translated with the original in brackets when the ticket was not in English, followed by the ticket id, role and segment. Sub-clusters with counts where the theme splits. Then three lines headed **Absent:**, **Stopped or continuing:** and **For triage:**. The last names the kind, the ids, and the one thing signal-triage has to know that the counts do not show. Never a solution, unless a customer proposed one, in which case it is theirs.
- **Themes not acting on**, one bullet each with the reason: how-to volume, one segment far from the ICP, already decided in the decision records if the existing reports cite it.
- **One account.** One bullet per single-account cluster: account, tickets, the intent, the strongest quote and id. Say which the next period should watch.
- **Already fixed.** Themes that stopped at a release, with the release date, and any tickets after it.
- **What this synthesis cannot tell you.** Segments that raise problems outside support, end users who never write to support, and anything a tag count would have said differently. Name what `context_file` was silent on.
- **Method.** How the queue was read (files or CLI commands, and the batches if any), the table file, the rule for a theme.

## 6. Check

- the report body is under 1800 words; state the count
- every number in the report is a row count from the table; nothing cites a tag count except the mis-tag rate
- every quote is in the ticket it cites, word for word; re-read the ticket to confirm
- every theme names at least two accounts
- every table row has a theme, One account, or "everything else"; the row counts add up to tickets read
- every theme worth acting on has its Absent line, and the absent accounts are the ones step 4 listed
- no For triage line names a solution
- the report says what is new against the existing reports

## 7. Close

Three lines:

```
Report: <path>   Table: <path>   Tickets read: <n>   Themes: <n>   One account: <n>   Already fixed: <n>
For signal-triage: <themes that describe something broken or missing today, with the control account where there is one>
For the OST: <themes that are opportunities, and what the under-filing segments' silence hides>
```
