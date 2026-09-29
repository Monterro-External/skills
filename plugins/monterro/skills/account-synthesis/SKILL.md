---
name: account-synthesis
disable-model-invocation: true
description: Read a period of CSM notes and escalations and write the account report, one section per account, weighted by ARR. Not for the market (sales-synthesis).
---

# account-synthesis

A period of notes and threads goes in, one report comes out, with the table it was built from beside it. Key accounts are 35% of ARR on ten tenants and they do not file tickets; their pain arrives as a workaround in a CSM note, a line on a renewal sheet, or an email that went past the CSM. Counting mentions under-serves them by design, so this report is written per account. It creates nothing in the tracker.

Paths come from `.claude/monterro.json`: the signal root from `signals` (default `signals`), with `csm/` and `escalations/` under it; release notes from `releases` (default `releases/release-notes`); the board from `tracker` or `tickets_dir`; reports into `synthesis` (default `discovery/synthesis`). If the config is missing, say so in one sentence and stop.

Notes are written for the writer. `signals/README.md` has the shorthand table; expand every abbreviation and **leave every initial as written unless the note itself names the person**. This holds in the table as much as in the report: a full name that appears nowhere in `csm/` or `escalations/` is a fabrication even when the person is real elsewhere in the corpus, because those folders are the only ones this skill may read. Write "the Tampere manager (JM)", never a name you inferred.

## 1. Scope

The period is what the person named, or else everything after the last account report in the synthesis folder, or else everything. An account named instead of a period means that account, all time. Say the period, the note count by `type`, the accounts covered by segment, and the escalation threads in one line before reading.

## 2. Read

1. `signals/README.md`, "Reading this well" and the CS shorthand.
2. The existing reports in the synthesis folder, for format and so the report says what is new against them.
3. Every release note dated inside or up to twelve months before the period, for claims that something was fixed.
4. Every note in the period, fully, and every escalation thread whose dates touch the period, fully.
5. The board, read-only: search issue titles for each problem an account is living with, so the report can say whether a ticket exists.

Nothing else. Not the support queue, not the sales folder. A note that says "raised through support twice" is a claim, recorded as one and pointed at `support-dig`.

## 3. The table, one row per observation

Write `<produced date>-accounts-<period slug>.table.md` in the synthesis folder. One row per bullet or paragraph that says something about the account: file; date; tenant; segment; CSM; note `type`; health band if the note gives one; **who at the account**, as written; **the observation** in one line in your own words, expanded from the shorthand; **kind**: `problem` (something they see happen), `workaround` (a routine they run around the product), `ask`, `renewal` (anything about the renewal, the comparison sheet, or a competitor), `commitment` (an action point or promise from our side), `claim` (a statement about another channel: tickets filed, support answers), `other`; **first raised**, the earliest note where this account said the same thing, blank if this is the first; the quote.

An escalation thread gives one row per message, kind by the same rule, and its stated numbers copied exactly.

The table is complete when every note and every message has at least one row. Say the counts.

## 4. Read per account

For every key account, and every account of any segment with an escalation, a red or amber health band, or a `renewal` row, build the account's picture from its rows:

- **Trajectory.** Health bands by date, and the renewal date or window if any note gives one.
- **Living with.** Each `problem` and `workaround`, with first and last note, and whether the board has a ticket. A workaround with no ticket is the finding this report exists for: name it as such. Where a release note says the thing was fixed, say whether the account mentioned it afterwards.
- **Asking for.** Each `ask`, and whether a note puts it on the renewal comparison sheet.
- **Promised.** Each `commitment` that is a promise to the account, and whether a later note closes it. Open ones are listed by date. A CSM's own routine todo is not a commitment: "AP: f/u next call", "send numbers by Fri" and the like are housekeeping, counted in one line as routine action points and never listed. If the count of open commitments runs past a handful, the filter is wrong.
- **Escalated.** For a thread: who raised it to whom, the numbers the customer states, the three things they ask, and what our last message committed to.

Then across accounts: a **shared problem** is the same `problem` or `workaround` at two or more accounts. Rank shared problems by the ARR share of the segments carrying them, never by row count. Mid and long-tail notes without any of the triggers above are read in aggregate only.

## 5. Write the report

File: `<produced date>-accounts-<period slug>.md` in the synthesis folder. Frontmatter in the existing reports' shape: `type: synthesis`, `source: CSM notes and escalations`, `period`, `notes_read`, `threads_read`, `accounts`, `produced`, `by` (the person who asked, if they said who they are; otherwise `unattended run`; never an invented name), `method: /monterro:account-synthesis over signals/csm and signals/escalations, table in <table file>`.

Length: **hard ceiling 1400 words** for the report body, counting from the first heading and excluding the frontmatter and the table file. One account section fits on a screen. Count the words before saving and say the count in the closing block. Over the ceiling, demote whole items from the bottom of the ranking to one-line bullets; never trim sentences evenly across the report, which loses the evidence and keeps the padding.

Body, in the existing reports' register:

- **Heading and one paragraph.** Notes by type, accounts by segment, threads, and that the report is per account and weighted by ARR because the accounts that matter most write the fewest tickets.
- **Accounts table**, key accounts first, then any triggered account, sorted by renewal proximity: account, segment, health trajectory, renewal, open escalation, problems, workarounds without a ticket, open commitments.
- **One section per account in the table.** Trajectory in one sentence. Living with, one bullet per problem or workaround with first and last note, the quote expanded from the shorthand with the original in brackets, the file, and `ticket: <id>` or `no ticket`. Asking for, one bullet each, marked if on the renewal sheet. Promised, open commitments by date. Escalated, one paragraph with the customer's numbers as written. Then one line headed **For triage:** naming the kind, the files, and what the account's own words show that nothing else does. Never a solution, unless the account proposed one, in which case it is theirs.
- **Shared across accounts.** One bullet per shared problem: the accounts, the segments and ARR share, the earliest first-raised date, and whether any of them has a ticket.
- **Renewal calendar.** Accounts with a renewal in or near the period, what is on their sheet, and the health band at the last note.
- **Claims to check.** Every `claim` row: the account, what they say happened in another channel, and the `support-dig` hypothesis it becomes.
- **Mid and long tail in aggregate.** What the untriggered notes say, in three or four lines, with counts.
- **What this synthesis cannot tell you.** Accounts with no note in the period, the accounts that left, initials that could not be resolved, and that notes record what the CSM chose to write down.
- **Method.** Files read, the table file, the triggers, the ranking rule.

## 6. Check

- the report body is under 1400 words; state the count
- every number in the report is a row count from the table or a number the customer stated, quoted
- every quote is in the file it cites, word for word, before the expansion in brackets
- every workaround in an account section says ticket or no ticket, and the board was searched for it
- every open commitment is a promise to the account, not a routine action point; routine ones are a count, not a list
- every open commitment has a date and no later row closing it
- no full name in the report or the table that is absent from the notes and threads read
- every shared problem names two or more accounts and is ranked by ARR share, never by rows
- no For triage line names a solution
- the report says what is new against the existing reports

## 7. Close

Three lines:

```
Report: <path>   Table: <path>   Notes read: <n>   Threads: <n>   Accounts: <n>   Workarounds without a ticket: <n>   Open commitments: <n>
For signal-triage: <shared problems and escalated problems that describe something broken today, with the accounts>
For the renewal review: <accounts by renewal date, with the one thing on each sheet>
```
