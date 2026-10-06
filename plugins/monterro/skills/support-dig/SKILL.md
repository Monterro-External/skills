---
name: support-dig
disable-model-invocation: true
description: Test one hypothesis against the whole support queue and return the evidence pack.
---

# support-dig

A hypothesis goes in, an evidence pack comes out. "End users get a payment message every day", "the week number is wrong again", "nobody can see why a charge failed". The pack says how widespread it is, who has it, who does not, and what the customers actually wrote, so that whoever writes or grills the ticket has the ids and not an impression. It creates nothing in the tracker and writes no file unless asked.

Paths come from `.claude/monterro.json`: the queue from `support` (default `signals/support`), release notes from `releases` (default `releases/release-notes`), segments and the account list from `context_file`. If the config is missing, say so in one sentence and stop.

**Reading the queue.** By default, search the export files in the `support` folder directly (JSONL, CSV, or one file per ticket) with grep over the text, reading a large export in chunks. When `support_cli` is set (for example `npm run support --`), use its commands instead (`list --grep`, `show`, `tenants`, `months`, `ids`, and whatever filters it documents). Search by words, never by tag.

## 1. Restate

Write the hypothesis as what a customer sees or is trying to do, in one line. Then list the words a customer would use for it in every language the queue holds; find the languages from the export or `context_file`. Tickets stay in the language they arrived in, so a search in one language finds one market.

## 2. Search, twice

First pass: search with the union of the words, over the whole queue and not a period, because a hypothesis about the product is not bounded by when someone last looked. Read every hit. Keep the ones that match on reading, set aside the ones that share a word and not the problem, and note the words the matching customers actually used.

Second pass: search again with the words learned from the first pass. Stop when a pass adds nothing. Read in full every kept ticket with three or more messages: the replies say what support told the customer, which is often the finding.

Say how many hits each pass returned and how many were kept.

## 3. Distribution

Group the kept tickets by account and by month: with a CLI, `tenants --ids <kept ids>` and `months --ids <kept ids>`; without one, count them from the kept tickets. The absent accounts are those with tickets elsewhere in the queue, or in the account list in `context_file`, and none here. Segments, their shares and how each raises problems come from `context_file`; where it is silent, say so and assume nothing.

- **Absent.** The absent accounts, with segment. Where the hypothesis is about something every account uses, an account with volume in the queue and none here is the control case: name it and say that what is different about it is the first question to answer. Pick the control from a segment that `context_file` says raises problems through support; an account from a segment that raises them elsewhere (a CSM, an account manager) proves little by its absence. Where `context_file` is silent on this, pick the account with the most other tickets and say why.
- **Inverted.** Segment shares against the queue's shares. Where a segment is under its share, say what its silence could hide, and whether `context_file` says it under-files.
- **Timing.** Months active, and whether the tickets start, stop or continue at a release. Read the release notes for the area; where one says the thing was fixed, split the kept tickets by date around it.
- **One account.** If every kept ticket is from one account, say so first. However many tickets, that is one voice.

## 4. The pack

Print it; write it to `<synthesis folder>/<date>-dig-<slug>.md` only if the person asked for a file.

- **Hypothesis**, as restated.
- **Verdict**, one line: supported across N accounts in M segments; one account; not found in the queue. Never more certain than the count allows.
- **Tickets**, grouped by account with segment, one line per ticket: id, date, role, the intent in your words.
- **Quotes**, three to five, the ones that carry it, translated with the original in brackets when not in English, with id, role and segment. Include the support reply where it shows what customers are being told.
- **Absent**, **Inverted**, **Timing**, one line each from step 3.
- **Where else to look**: which other signal sources should show it if it is real, named by the paths in `.claude/monterro.json` (reviews, end-user feedback, NPS verbatims, CSM notes, the request channel, usage data), and what each would add. You do not open them; two independent sources is the rule and this pack is one.
- **Search**: the patterns run and the hit and kept counts per pass, so the dig can be repeated.

## 5. Check

- every id in the pack is among the kept tickets of the final pass (the `ids` output with a CLI)
- every quote is in the ticket it cites, word for word
- the absent accounts are the ones step 3 listed, and the control case, if any, is named
- the verdict's count matches the account count in the pack
- nothing in the pack proposes a fix

## 6. Close

Three lines:

```
Hypothesis: <one line>   Verdict: <one line>   Tickets: <n> across <accounts> accounts
Control case: <account and what to ask, or none>
Confirm in: <the one other source most likely to settle it>
```
