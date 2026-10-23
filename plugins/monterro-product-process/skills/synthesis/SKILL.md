---
name: synthesis
disable-model-invocation: true
description: Read every synthesis report, the request ledger and the cohorts, and write the cross-source report: what two or more independent channels show. Reads reports only, never raw signals.
---

# synthesis

The reports go in, one more report comes out, into the same folder. Every per-channel report is honest about its own channel and blind to the others; this pass reads across them. It exists for one kind of finding above all: the pattern no channel shows, where end users in one report do something and a cohort in another counts it, and nobody has filed anything. It creates nothing in the tracker.

Paths come from `.claude/monterro.json`: reports and the ledger from `synthesis` (default `discovery/synthesis`), cohort analyses from `cohorts` (default `discovery/usage-cohorts`), principles and non-goals from `context_file`, the decision records from `decisions` (default `strategy/decisions`), the board from `tracker`. If the config is missing, say so in one sentence and stop.

Raw signals are not input. If a channel has no report for the period, say so and go on; never open the raw signal folders to fill the gap. The gap is a finding of its own.

## 1. Scope

The period is what the person named, or else the union of the periods of every report newer than the last cross-source report, or else everything in the folder. List the reports in scope by file, with each one's source and period, and the cohort analyses, before reading. Say which channels have no report in the period.

## 2. Read

1. `context_file`: the principle that a pattern needs two independent sources, the segments and their ARR shares, the non-goals. Where it is silent on segments or ARR, say so and rank by families alone.
2. The decision records, for findings already decided or deferred. Read its `README.md` index, then the PDRs in the product areas at hand; only `accepted` PDRs decide anything, a `proposed` one is reported as pending.
3. Every report in scope, fully, including its table file where one exists, and the ledger.
4. Every cohort analysis in scope.
5. The board, read-only: search issue titles and descriptions for each cross-source finding, so the report says whether a ticket already exists and in what state.

Nothing else.

## 3. The table, one row per claim

In your working notes, one row per theme, ledger row, observation, one-account bullet, did-nothing bullet, heard-once bullet, claim, and cohort finding across every report in scope: the report file and heading; **channel family**: `support` (support-synthesis, support-dig), `feedback` (NPS, reviews, in-product feedback), `accounts` (CSM notes, escalations), `sales`, `requests` (the ledger), `interviews`, `platform` (cohorts and, later, platform-dig); the claim in one line in your own words; kind as the report gave it; accounts named; segments; period; the ids or files it cites; whether the report promoted it to a theme or kept it as one voice.

Two reports from the same family are one source. Support-dig confirming a support-synthesis theme is the same channel twice.

## 4. Cross

Give rows the same **finding** when they describe the same thing happening or wanted, whatever each report called it. Then, for every finding:

- **Independent.** Count channel families. Two or more is a cross-source finding. One, however many rows, goes to **Seen in one channel**, never promoted and never dropped; say which channel and whether the others had a report in the period at all, because an absence with no report proves nothing.
- **Promoted below the line.** A finding that every report kept as one voice, one account or one prospect, but that two families carry, is exactly what this pass exists to promote. Mark it, and say that no single report could have.
- **Contradictions.** Where reports disagree, or a report disagrees with `context_file` or a decision-record entry (a documented default that the support queue says is not what happens; a fix a release note claims and a later channel still reports), one row each with both sides quoted. A contradiction is a finding, not an error to resolve here.
- **Nobody asked.** Behaviour in one family and a number in another, with no `ask` or `request` row anywhere: the did-nothing users beside a churn cohort, a workaround in an account note beside an absence in the queue. These get their own section and a population size where a cohort gives one.
- **Decided.** Findings the decision records or `context_file` already decides or defers, with the entry quoted; they are reported and marked, never re-argued.
- **On the board.** The issue id and state where one exists.

## 5. Write the report

File: `<produced date>-cross-source-<period slug>.md` in the synthesis folder. Frontmatter in the existing reports' shape: `type: synthesis`, `source: cross-source`, `period`, `reports_read` as a list of files, `produced`, `by` (the person who asked, if they said who they are; otherwise `unattended run`; never an invented name), `method: /monterro-product-process:synthesis over discovery/synthesis and discovery/usage-cohorts`.

Length: **hard ceiling 1500 words** for the report body, counting from the first heading and excluding the frontmatter and the table file. This report cites; it does not repeat. Count the words before saving and say the count in the closing block. Over the ceiling, demote whole items from the bottom of the ranking to one-line bullets; never trim sentences evenly across the report, which loses the evidence and keeps the padding.

Body, in the existing reports' register:

- **Heading and one paragraph.** Reports read with their periods, channels without a report, and the rule: two independent channel families or it stays a hypothesis.
- **Findings table**, ranked by channel families and then by segment ARR share: finding, families, reports, accounts, segments, kind, on the board, decided.
- **One section per cross-source finding.** What is happening, in two or three sentences. One line per report that carries it, with the heading and the ids or files it cites, so the ticket's `signal_refs` can be copied from here. Where a report kept it below the line, say so. Then two lines headed **Absent from:** (the families that had a report and do not carry it, and what that says) and **For triage:** (kind, the strongest ids across channels, the one thing no single report shows). Never a solution, unless a customer proposed one, in which case it is theirs.
- **Nobody asked for this.** One section per such finding in the same shape, plus the population size and its source, and one line on what product moment is missing between the behaviour and the outcome.
- **Contradictions.** One bullet each, both sides quoted with file and heading, and who has to settle it.
- **Seen in one channel.** One bullet each: the claim, the channel, the report, and whether the silent channels had a report in the period.
- **Already decided.** One bullet each with the decision-record or `context_file` line.
- **For platform-dig.** One bullet per hypothesis this report cannot confirm from documents, naming the export and column that would.
- **What this synthesis cannot tell you.** The channels with no report, the periods that do not overlap, and that a finding kept apart by every report and joined here is still built from second-hand sources.
- **Method.** Reports read, the channel-family rule, the ranking rule.

## 6. Check

- the report body is under 1500 words; state the count
- every finding names two or more channel families, and no two rows from the same family were counted as two
- every citation points at a heading that exists in the report it names, and every id came from that report
- every one-family finding is in Seen in one channel, with the silent channels' report status stated
- every did-nothing or workaround row from the reports is either in a finding or in Nobody asked for this; count them
- every finding that the decision records covers is marked decided, with the entry quoted
- no For triage line names a solution
- the report says what is new against the previous cross-source report, if one exists

## 7. Close

Three lines:

```
Report: <path>   Reports read: <n>   Channels without a report: <list>   Findings: <n>   Nobody asked: <n>   Contradictions: <n>   Seen once: <n>
For signal-triage: <findings that describe something broken or missing today, with their families>
For the OST: <the nobody-asked findings, with population size where known>
```
