---
name: feature-request-ledger
disable-model-invocation: true
description: Sweep the request channel and update the one living feature-request ledger, one row per ask. Never creates a ticket.
---

# feature-request-ledger

The channel export goes in, the ledger comes out updated. The channel is the fastest route from a customer conversation to product and the worst structured: no fields, no dedup, and the same ask from the same market three times in a quarter reads as three asks. The ledger is where those three become one row with a count. It creates nothing in the tracker and posts nothing in the channel.

Paths come from `.claude/monterro.json`: the export folder from `requests` (default `signals/slack`), the ledger into the folder named by `synthesis` (default `discovery/synthesis`) as `feature-request-ledger.md`, non-goals and product areas from `context_file`, the decision records from `decisions` (default `strategy/decisions`), the board from `tracker` or `tickets_dir`. If the config is missing, say so in one sentence and stop.

## 1. Scope

Read the ledger's sweep log if the ledger exists. This sweep covers every message dated after the last sweep's `through` date, or the whole export on the first run, or the period the person named. Say the period and the message count in one line before reading. Rerunning the same period must change no row; it only appends a sweep-log line.

## 2. Read

1. The export, fully, for the period. Newest at the bottom, thread replies indented under the message they answer, a reactions line under some messages.
2. `context_file`: the non-goals section and the product-area taxonomy.
3. The decision records, for entries that decide or defer an ask. Read its `README.md` index, then the PDRs in the product areas at hand; only `accepted` PDRs decide anything, a `proposed` one is reported as pending.
4. The existing ledger, every row, so the sweep updates rows instead of duplicating them.
5. The reports in the synthesis folder, by title and theme headings only, for corroboration.
6. The board: search issue titles for each open ask so a row that already has a ticket says so. Read-only.

Nothing else. Not the support queue, not the other signal folders. Those are combined with the ledger later, by `signal-triage`, `synthesis` and people.

## 3. The table, one row per message

Build it in your working notes before clustering. One row per top-level message and per reply: date; poster; **kind**: `ask` (a request relayed on behalf of a customer or prospect), `observation` (a volume or behaviour note, "export complaints spiked in March"), `decision` (a product person answering in-thread with a yes, a no or a later), `chatter` (everything else); **account**, the customer account or prospect named, or `unnamed` with the count the poster gives ("two mid-market owners" is `unnamed: 2`); **the ask** in one line in your own words, what the customer wants to be able to do, never the feature name they used; **claimed repeats**, the number in the text ("third time", "second one", "again" is 2 if nothing else is said), blank otherwise; the verbatim text.

Reactions are not votes and are not counted. A poster saying a customer "has asked support three times as well" is a claim, recorded in the row, never a corroboration.

The table is complete when its row count equals the message count from step 1. Say both numbers.

## 4. Cluster into asks

Give every `ask` row a cluster: the same thing wanted about the same part of the product, whatever words were used. Two named local payment methods and "a local wallet" are one ask about payment methods only if the poster says the customer would have paid with any of them; otherwise they are one ask per market, and the ledger row says so. A price and the structure it prices are different asks (a team price is one row, several users under one payer another).

For each cluster count: **named accounts**, distinct; **unnamed**, summed from the rows; **messages**; **posters**, distinct; **claimed repeats**, the highest number anyone stated; **first seen** and **last seen**. Assign one product area from the taxonomy. Two areas means two rows.

Then set the row's status, in this order, first match wins:

- **declined**: the ask is inside a non-goal in `context_file`, or the decision records rejects it. Quote the line. An in-thread no from a product person is recorded as a decision (step 5) and is not enough on its own; say "in-thread no, not in the decision records" when that is the case.
- **deferred**: the decision records or an in-thread decision says later and names what it waits for.
- **ticketed**: the board has an issue for it. Record the id.
- **corroborated**: a synthesis report in the folder has a theme for the same ask, named by file and heading, or the cluster has three or more named accounts from two or more posters. This is the row `signal-triage` picks up.
- **watching**: everything else. One account, however many times, stays here.

`observation` rows are not asks. They go to their own section and are pointed at `signal-triage` as a possible defect, with the poster's words; where the poster says the tags are useless, say that too, because it tells `support-dig` how to search.

## 5. Update the ledger

One file, `feature-request-ledger.md` in the synthesis folder, updated in place. Frontmatter: `type: ledger`, `source: <channel name from the export's heading>`, `covers: <first message date> to <last message date>`, `messages_read: <cumulative>`, `last_sweep: <today>`, `method: /monterro-product-process:feature-request-ledger over <export path>`. Create the file with this shape on the first run; on every later run edit rows and keep the shape.

Sections, in this order:

- **Open asks.** One table row per cluster with status `watching` or `corroborated`, sorted by named accounts, then last seen: ask, area, named accounts (the names), unnamed, messages, posters, claimed repeats, first seen, last seen, corroboration (file and heading, or the account count that qualified), status. Below the table, one block per open ask: the ask in one line, then every message that carries it as a blockquote with `<export file> · <date> · <poster>` after it. Those citations become the ticket's `signal_refs` later, so they are exact.
- **Observations.** One bullet per `observation` row: date, poster, the words, and the one line that says what `signal-triage` or `support-dig` should check.
- **Decisions made in the channel.** One bullet per `decision` row: date, who, what they said, and whether the decision records has it. A decision that is only in the channel is flagged, because it will be relitigated.
- **Closed.** One line per row with status `ticketed`, `declined` or `deferred`: ask, status, the id, the quoted non-goal or decision-record entry, or what it waits for, **and the row's strongest citation, kept**: the one quote with its date and poster that most shows the ask being made, and the claimed-repeat quote where there is one. A closed row still has to prove the ask happened. A closed row keeps its counts; a new message on a closed ask updates last seen and claimed repeats and the row stays closed, unless the message is new evidence against a deferral, which is said in the sweep summary.
- **Archive.** Rows unseen for twelve months, one line each. Never deleted.
- **Sweep log.** One line per run, appended: date, period swept, messages read, rows added, rows updated, status changes as `ask: old to new`.

Length: the ledger grows with distinct asks, never with runs. **Hard ceiling 60 words per open-ask block** beyond its quotes, and the quotes are the evidence, so they stay. If a sweep adds more rows than it updates after the first run, look again at the clustering before writing.

## 6. Check

- every open-ask block is under 60 words beyond its quotes
- every closed row keeps its strongest citation, and its claimed-repeat quote where it has one
- the counts in the closing block are the ledger's totals, not the sweep's
- every quote in the ledger is in the export, word for word, with the date and poster the export gives
- every `ask` row in the step 3 table is in exactly one cluster; the cluster counts add up to the ask rows
- no row's named-account count includes an unnamed mention, and no reaction was counted as anything
- every `declined` row quotes the non-goal or decision-record line; every `deferred` row names what it waits for
- every `corroborated` row names its second source or its qualifying accounts and posters
- no open ask names a solution in the ask column; a customer's proposed solution appears in the quote as theirs
- the row count of Open asks plus Closed plus Archive equals the previous ledger's plus rows added

## 7. Close

Four lines, then the replies:

```
Ledger: <path>   Period: <from> to <through>   Messages read this sweep: <n>   Asks: <open>/<closed>   Observations: <n>
(every count after the period is the ledger's total, not this sweep's, so a sweep that reads nothing changes none of them)
For signal-triage: <the corroborated asks, and the observations that describe something broken today>
Decided only in the channel: <asks with an in-thread decision the decision records does not have>
Replies to post: <n>
```

Then, for every row whose status changed this sweep, one line the PM can paste into the thread that raised it: the status in plain words and the id, the non-goal, or what it waits for. The skill never posts them. A thread that gets its answer stops being re-asked, which is the only thing that makes the counts in this ledger go down.
