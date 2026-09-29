---
name: feedback-synthesis
disable-model-invocation: true
description: Read a period of NPS comments, app reviews and member feedback and write the synthesis report, the operator's voice kept apart from the member's.
---

# feedback-synthesis

A period of the three thin channels goes in, one report comes out, with the table it was built from beside it. NPS is the operator's verdict, reviews and desk notes are the member's, and each alone is too small to carry a theme. Together they are the only place members speak. It creates nothing in the tracker.

Paths come from `.claude/monterro.json`: the signal root from `signals` (default `signals`), with `nps/`, `reviews/` and `member-feedback/` under it; release notes from `releases` (default `releases/release-notes`); reports into `synthesis` (default `discovery/synthesis`). If the config is missing, say so in one sentence and stop.

The files are small enough to read whole: `nps/responses.csv` and `nps/verbatims.md`, `reviews/app-store-reviews.csv`, one file per month in `member-feedback/`. Every count in the report comes from the table you build in step 3.

## 1. Scope

The period is what the person named, or else everything after the last feedback report in the synthesis folder, or else everything. Say the period and the item counts per channel in one line before reading.

## 2. Read

1. `signals/README.md`, "Reading this well": the rules that decide what counts.
2. The existing reports in the synthesis folder, for format and so the report says what is new against them.
3. Every release note dated inside or up to twelve months before the period. Each fixed or added item is a candidate for a theme that should have stopped.
4. The three channels for the period, fully. In `responses.csv` the rows without a comment count too: they carry the score by segment and role.

Nothing else. Not the support queue, not the CSM notes, not the platform exports. Those are combined with this report later, by `synthesis` and by people.

## 3. The table, one row per item with words

Write `<produced date>-feedback-<period slug>.table.md` in the synthesis folder, one row per NPS comment, review and desk note: id (`NPS-`, `REV-`, `MF-`); channel; date; tenant, for reviews the `tenant_guess` marked as a guess when `gym_mentioned` is empty; segment; country; **voice**: `operator` for NPS, `member` for reviews and desk notes; role or rating; language; **intent** in your own words, one line, what the person was trying to do or saw happen, never the review title; **kind**: `defect`, `request`, `praise`, `did-nothing` (a member who had a problem and asked for nothing: let payments fail, stopped coming, stayed for a reason unrelated to us), `other`; **theme**: blank for now.

Then, from the scored rows, a small second table: NPS by segment and by role, responses and comments per wave, and the share of one- and two-star reviews. Say the row count against the item count from step 1.

## 4. Cluster

Fill the theme column. A **theme** is the same intent from at least two tenants, or from the operator's voice and the member's voice about the same tenant. The same intent from one tenant in one voice, however many items, goes to **One tenant**, never promoted and never dropped. Identical wording from two respondents at the same tenant is one voice.

For each theme:

- **Both voices, or one.** A theme members complain about that no operator mentions is one the operator does not see from the desk; a theme operators score against that no member mentions is one members meet as the gym's problem. Say which.
- **Inverted.** Key accounts are 35% of ARR and their NPS is the lowest; their members write reviews naming the gym and not us. A theme with few key-account items is not a smaller problem for them.
- **Stopped or continuing.** Where a release note says the thing was fixed, split the theme's items by date around the release. Items after it mean the fix did not take or the story outlived it.
- **Answered.** For reviews, the share with a `developer_reply`, and whether the reply says anything about the problem. A canned reply on a defect is a finding.

Within a theme, separate what the person **saw happen** from what they **ask to have**. A defect and a request about the same moment are two themes.

`did-nothing` rows are never a theme. They go to their own section, because they are the members nobody hears from and no keyword finds.

## 5. Write the report

File: `<produced date>-feedback-<period slug>.md` in the synthesis folder. Frontmatter in the existing reports' shape: `type: synthesis`, `source: NPS, app reviews, member feedback`, `period`, `items_read` with the three counts, `produced`, `by` (the person who asked, if they said who they are; otherwise `unattended run`; never an invented name), `method: /monterro:feedback-synthesis over signals/nps, signals/reviews, signals/member-feedback, table in <table file>`.

Length: **hard ceiling 1200 words** for the report body, counting from the first heading and excluding the frontmatter and the table file. Count the words before saving and say the count in the closing block. Over the ceiling, demote whole items from the bottom of the ranking to one-line bullets; never trim sentences evenly across the report, which loses the evidence and keeps the padding.

Body, in the existing reports' register:

- **Heading and one paragraph.** Items read per channel, response rate, NPS overall and by segment, share of low-star reviews, and that themes are by intent across both voices. Say to read it as a picture of the period and not as a backlog.
- **Themes table**, ranked by tenants and then items: theme, items, tenants, channels, voices, segments, months active, kind, confidence.
- **One section per theme worth acting on**, defects first. The finding in two or three sentences. The quotes that carry it, two or three, translated with the original in brackets when not in English, followed by id, voice, role or rating, segment. Then three lines headed **Voices:**, **Stopped or continuing:** and **For triage:**. The last names the kind, the ids, and the one thing `signal-triage` has to know that the counts do not show. Never a solution, unless a member or operator proposed one, in which case it is theirs.
- **What members did instead of asking.** One bullet per `did-nothing` row: tenant, date, id, the words. Then one paragraph on what they have in common, and one line naming what would have to be combined with this to know how many they are. This section goes to `synthesis`, never to triage.
- **What to protect.** The praise, clustered the same way, one line per cluster with a quote. What people would miss.
- **One tenant.** One bullet per single-voice cluster: tenant, items, the intent, the strongest quote and id.
- **Already fixed.** Themes that stopped at a release, with the release date, and any items after it.
- **What this synthesis cannot tell you.** Who does not answer surveys, which members never review, the tenants a review could not be attributed to, and anything a score alone would have said differently.
- **Method.** Files read, the table file, the rule for a theme.

## 6. Check

- the report body is under 1200 words; state the count
- every number in the report is a row count from the table or the score table
- every quote is in the file it cites, word for word, with the id the file gives
- every theme names at least two tenants, or both voices at one tenant
- every table row has a theme, One tenant, did-nothing, praise, or "everything else"; the counts add up to rows
- every `did-nothing` row is a bullet in its section; count them
- no For triage line names a solution
- the report says what is new against the existing reports

## 7. Close

Three lines:

```
Report: <path>   Table: <path>   Items read: <nps>/<reviews>/<desk>   Themes: <n>   Did nothing: <n>   One tenant: <n>
For signal-triage: <themes that describe something broken or missing today, and which voice carries each>
For synthesis: <the did-nothing members, and what to combine them with>
```
