---
name: feedback-synthesis
disable-model-invocation: true
description: Read a period of NPS comments, reviews and in-product feedback and write the synthesis report, the customer account's voice kept apart from the end user's.
---

# feedback-synthesis

A period of the thin feedback channels goes in, one report comes out, with the table it was built from beside it. Each channel alone is usually too small to carry a theme, and together they are often the only place end users speak. It creates nothing in the tracker.

Paths come from `.claude/monterro.json`: the channel folders from `channels.nps`, `channels.reviews` and `channels.in_product`, any of which may be null when the company has no such channel; segments, ARR shares and how each segment tends to raise problems from `context_file`; release notes from `releases` (default `releases/release-notes`); reports into `synthesis` (default `discovery/synthesis`). If the config is missing, say so in one sentence and stop.

Read whatever files each configured folder holds, in any format (CSV, markdown, JSONL); they are usually small enough to read whole. A channel that is not configured, or whose folder is empty, is named as such in the report. Every count in the report comes from the table you build in step 3.

## 1. Scope

The period is what the person named, or else everything after the last feedback report in the synthesis folder, or else everything. Say the period and the item counts per channel in one line before reading.

## 2. Read

1. Any README or shorthand glossary in the channel folders or the signals root: the rules that decide what counts.
2. The existing reports in the synthesis folder, for format and so the report says what is new against them.
3. Every release note dated inside or up to twelve months before the period. Each fixed or added item is a candidate for a theme that should have stopped.
4. Every configured channel for the period, fully. Scored rows without a comment count too: they carry the score by segment and role.

Nothing else. Not the support queue, not the CSM notes, not the platform exports. Those are combined with this report later, by `synthesis` and by people.

## 3. The table, one row per item with words

Write `<produced date>-feedback-<period slug>.table.md` in the synthesis folder, one row per NPS comment, review and in-product feedback item: id as the file gives it; channel; date; customer account, marked as a guess when the item does not name it; segment; country; **voice**: `account` (the customer's staff or admins) or `end user` (the people the customer serves), from the item's role where it has one, else by channel, and say which rule was used; role or rating; language; **intent** in your own words, one line, what the person was trying to do or saw happen, never the review title; **kind**: `defect`, `request`, `praise`, `did-nothing` (someone who had a problem and asked for nothing: stopped using it, worked around it, stayed for a reason unrelated to us), `other`; **theme**: blank for now.

Then, from the scored rows, a small second table: NPS by segment and by role, responses and comments per wave, and the share of one- and two-star reviews. Say the row count against the item count from step 1.

## 4. Cluster

Fill the theme column. A **theme** is the same intent from at least two customer accounts, or from the account voice and the end-user voice about the same account. The same intent from one account in one voice, however many items, goes to **One account**, never promoted and never dropped. Identical wording from two respondents at the same account is one voice.

For each theme:

- **Both voices, or one.** A theme end users complain about that no account mentions is one the customer's staff do not see; a theme accounts score against that no end user mentions is one end users meet as the customer's problem. Say which.
- **Inverted.** Compare each segment's share of items with its ARR share from `context_file`. A segment that gives little feedback is not a smaller problem for it; where `context_file` says how a segment tends to raise problems, use that. Where it is silent on segments or ARR, say so and do not assume.
- **Stopped or continuing.** Where a release note says the thing was fixed, split the theme's items by date around the release. Items after it mean the fix did not take or the story outlived it.
- **Answered.** Where the review data records our replies, the share with one, and whether the reply says anything about the problem. A canned reply on a defect is a finding.

Within a theme, separate what the person **saw happen** from what they **ask to have**. A defect and a request about the same moment are two themes.

`did-nothing` rows are never a theme. They go to their own section, because they are the users nobody hears from and no keyword finds.

## 5. Write the report

File: `<produced date>-feedback-<period slug>.md` in the synthesis folder. Frontmatter in the existing reports' shape: `type: synthesis`, `source:` the channels read, `period`, `items_read` with a count per channel, `produced`, `by` (the person who asked, if they said who they are; otherwise `unattended run`; never an invented name), `method: /monterro:feedback-synthesis over <the channel folders read>, table in <table file>`.

Length: **hard ceiling 1200 words** for the report body, counting from the first heading and excluding the frontmatter and the table file. Count the words before saving and say the count in the closing block. Over the ceiling, demote whole items from the bottom of the ranking to one-line bullets; never trim sentences evenly across the report, which loses the evidence and keeps the padding.

Body, in the existing reports' register:

- **Heading and one paragraph.** Items read per channel, channels not configured or empty, response rate, NPS overall and by segment, share of low-star reviews, and that themes are by intent across both voices. Say to read it as a picture of the period and not as a backlog.
- **Themes table**, ranked by accounts and then items: theme, items, accounts, channels, voices, segments, months active, kind, confidence.
- **One section per theme worth acting on**, defects first. The finding in two or three sentences. The quotes that carry it, two or three, translated with the original in brackets when not in English, followed by id, voice, role or rating, segment. Then three lines headed **Voices:**, **Stopped or continuing:** and **For triage:**. The last names the kind, the ids, and the one thing `signal-triage` has to know that the counts do not show. Never a solution, unless an end user or account proposed one, in which case it is theirs.
- **What users did instead of asking.** One bullet per `did-nothing` row: account, date, id, the words. Then one paragraph on what they have in common, and one line naming what would have to be combined with this to know how many they are. This section goes to `synthesis`, never to triage.
- **What to protect.** The praise, clustered the same way, one line per cluster with a quote. What people would miss.
- **One account.** One bullet per single-voice cluster: account, items, the intent, the strongest quote and id.
- **Already fixed.** Themes that stopped at a release, with the release date, and any items after it.
- **What this synthesis cannot tell you.** Who does not answer surveys, which users never review, the items that could not be attributed to an account, the channels the company does not have, and anything a score alone would have said differently.
- **Method.** Files read, the table file, the rule for a theme.

## 6. Check

- the report body is under 1200 words; state the count
- every number in the report is a row count from the table or the score table
- every quote is in the file it cites, word for word, with the id the file gives
- every theme names at least two accounts, or both voices at one account
- every table row has a theme, One account, did-nothing, praise, or "everything else"; the counts add up to rows
- every `did-nothing` row is a bullet in its section; count them
- no For triage line names a solution
- the report says what is new against the existing reports

## 7. Close

Three lines:

```
Report: <path>   Table: <path>   Items read: <per channel>   Themes: <n>   Did nothing: <n>   One account: <n>
For signal-triage: <themes that describe something broken or missing today, and which voice carries each>
For synthesis: <the did-nothing users, and what to combine them with>
```
