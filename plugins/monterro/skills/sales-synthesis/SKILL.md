---
name: sales-synthesis
disable-model-invocation: true
description: Read a period of sales call transcripts and lost-deal debriefs and write the synthesis report, themes ranked by where in the call they were raised and what they cost, the market's voice kept apart from the book's, and the losses we should have taken named. Reads the sales folder, CONTEXT.md and the decision log only. Not for renewal risk in the book (account-synthesis) and not for tickets.
---

# sales-synthesis

A period of calls and debriefs goes in, one report comes out, with the table it was built from beside it. Sales calls are the only source that hears from the market rather than from our own book, so a theme that appears here and nowhere else is early information rather than noise. It creates nothing in the tracker.

Paths come from `.claude/monterro.json`: the signal root from `signals` (default `signals`), with `sales/transcripts/` and `sales/lost-deals/` under it; ICP, segments and non-goals from `context_file`; the decision log from `decisions` (default `roadmap/decisions.md`); reports into `synthesis` (default `discovery/synthesis`). If the config is missing, say so in one sentence and stop.

Transcripts are short and few. Read every one whole; the deciding question is usually in the last third and a skim of the opening misses it.

## 1. Scope

The period is what the person named, or else everything after the last sales report in the synthesis folder, or else everything. Say the period and the counts, calls by `type` and debriefs, in one line before reading.

## 2. Read

1. `context_file`: the segment table, the ICP, the competitors and the non-goals. These decide which losses were correct.
2. The decision log, for asks already decided or deferred.
3. The existing reports in the synthesis folder, for format and so the report says what is new against them.
4. Every transcript and every debrief in the period, fully.

Nothing else. Not the CSM notes, not the support queue, not the request channel. Those are combined with this report later, by `synthesis` and by people.

## 3. The table, one row per moment

Write `<produced date>-sales-calls-<period slug>.table.md` in the synthesis folder. One row per **moment**, a place in a call or a debrief where the prospect raises, asks or decides something: file; date; call `type`; prospect; country; sites and members from the frontmatter; **segment**, read off the CONTEXT.md table from sites and members; **in ICP** yes or no, with the ICP line that decides it; outcome; timestamp; **who raised it**, prospect unprompted, prospect answering a question, or the rep; **position**, first, middle or last third of the call by duration; **the moment** in one line in your own words, what they need to be able to do or what decided them, never the feature name; **kind**: `deciding` (the prospect says or the debrief says it decided the deal), `ask`, `objection`, `praise`, `other`; the quote; theme, blank for now.

A debrief gives two rows at least: the reason in the customer's words and the reason the rep suspects, marked as such, with the ARR estimate and `lost_to`.

The table is complete when every call and every debrief has at least one row. Say the counts.

## 4. Cluster

Fill the theme column. A **theme** is the same moment from at least two prospects or accounts. One prospect, however long they talked, goes to **One prospect**, never promoted and never dropped.

For each theme:

- **Where it was raised.** Count the rows in the last third and the rows raised unprompted. A theme that lives in the last third and comes unprompted decides deals; a theme the rep raises in the first third is our pitch, and counts for nothing here.
- **What it cost.** Lost deals citing it, with ARR summed from the debriefs, and whether the debrief's suspected reason agrees with the stated one. **A deal is counted in full against one theme only**, the one its debrief says decided it; every other theme that deal touches shows `same deal` instead of the figure, so the ARR column can be added up.
- **Market or book.** Prospect demos hear from the market; renewals and expansions hear from accounts we already have. Say which the theme comes from. A renewal theme is renewal risk and is handed to `account-synthesis` by name.
- **Shape.** Where the same words hide different needs, split them and say so: a capability ("we cannot sign someone up at the desk without it") is not a credibility ("it makes us look old"); a Swedish block invoice is not a Finnish employer share. One theme per shape.
- **Correct loss.** A theme carried only by prospects outside the ICP, or inside a non-goal, is a loss we chose. It stays in the report under its own heading and never in the ranking.

## 5. Write the report

File: `<produced date>-sales-calls-<period slug>.md` in the synthesis folder. Frontmatter in the existing reports' shape: `type: synthesis`, `source: sales calls and lost deals`, `period`, `calls_read`, `lost_deals`, `produced`, `by` (the person who asked, if they said who they are; otherwise `unattended run`; never an invented name), `method: /monterro:sales-synthesis over signals/sales, table in <table file>`.

Length: **hard ceiling 1200 words** for the report body, counting from the first heading and excluding the frontmatter and the table file. Count the words before saving and say the count in the closing block. Over the ceiling, demote whole items from the bottom of the ranking to one-line bullets; never trim sentences evenly across the report, which loses the evidence and keeps the padding.

Body, in the existing reports' register:

- **Heading and one paragraph.** Calls by type, debriefs, markets, share of prospects in the ICP, and that themes are ranked by where in the call they were raised rather than by how often. Say that recorded calls are the ones that went well.
- **Themes table**, ranked by rows in the last third, then lost deals: theme, calls, unprompted, last third, lost deals and ARR, markets, market or book, kind.
- **One section per theme worth acting on**, deciding themes first. The finding in two or three sentences. The quotes that carry it, two or three, translated with the original in brackets when not in English, followed by file, timestamp, role and segment. Then three lines headed **Cost:**, **Market or book:** and **For triage:**. The last names the kind, the files, and the one thing `signal-triage` has to know that the counts do not show. Never a solution, unless the prospect or the rep proposed one, in which case it is theirs and named as theirs.
- **What decides deals that we already do well.** Themes where the prospect's question was answered and the call turned; the quote; and where that answer lives today, if anywhere written.
- **Losses we chose.** One bullet per correct-loss theme with the non-goal or ICP line it fails and the ARR we declined.
- **Heard from the book.** Renewal and expansion moments, per account, in one line each, with the file. This section is for `account-synthesis`.
- **One prospect.** One bullet per single-prospect cluster: prospect, country, the moment, the quote and file. Say which the next period should listen for.
- **What this synthesis cannot tell you.** Recording bias, the debrief author's bias, which markets had no calls, and that nothing here measures the existing book.
- **Method.** Files read, the table file, the rules for a theme and for position.

## 6. Check

- the report body is under 1200 words; state the count
- every number in the report is a row count or an ARR sum from the table
- no lost deal carries its full ARR against two themes; the second and later say `same deal`, and the column sums to the period's real loss
- every quote is in the file it cites at the timestamp it cites, word for word
- every theme names at least two prospects or accounts
- every correct-loss theme quotes the CONTEXT.md line that makes it one
- every renewal or expansion moment appears in Heard from the book
- no For triage line names a solution
- the report says what is new against the existing sales report

## 7. Close

Three lines:

```
Report: <path>   Table: <path>   Calls read: <n>   Lost deals: <n>   Themes: <n>   Losses we chose: <n>   One prospect: <n>
For signal-triage: <deciding themes that describe something missing today, with the market>
For account-synthesis: <the accounts whose renewal moments are in Heard from the book>
```
