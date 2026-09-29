---
name: support-dig
disable-model-invocation: true
description: Test one hypothesis against the whole support queue and return the evidence pack.
---

# support-dig

A hypothesis goes in, an evidence pack comes out. "Members get a payment message every day", "the week number is wrong again", "nobody can see why a charge failed". The pack says how widespread it is, who has it, who does not, and what the customers actually wrote, so that whoever writes or grills the ticket has the ids and not an impression. It creates nothing in the tracker and writes no file unless asked.

Paths come from `.claude/monterro.json`: the queue from `support` (default `signals/support`), release notes from `releases` (default `releases/release-notes`). If the config is missing, say so in one sentence and stop.

The queue is read through `npm run support -- <command>` in the product repo (`scripts/support.ts`; `list`, `show`, `tenants`, `months`, `ids`, filters in its header). Search by words, never by tag.

## 1. Restate

Write the hypothesis as what a customer sees or is trying to do, in one line. Then list the words a customer would use for it in every language the queue holds: English, Swedish, Norwegian, Finnish and Danish. Tickets stay in the language they arrived in, so a search in one language finds one market.

## 2. Search, twice

First pass: `list --grep` with the union of the words, over the whole queue and not a period, because a hypothesis about the product is not bounded by when someone last looked. Read every hit. Keep the ones that match on reading, set aside the ones that share a word and not the problem, and note the words the matching customers actually used.

Second pass: `list --grep` again with the words learned from the first pass. Stop when a pass adds nothing. `show` every kept ticket with three or more messages: the replies say what support told the customer, which is often the finding.

Say how many hits each pass returned and how many were kept.

## 3. Distribution

Run `tenants --ids <kept ids>` and `months --ids <kept ids>` and keep both outputs.

- **Absent.** The tenants the command lists as absent, with segment. Where the hypothesis is about something every tenant uses, a tenant with volume in the queue and none here is the control case: name it and say that what is different about it is the first question to answer. Pick the control from the segments that file freely, long tail and mid: a key account's absence is its habit, they carry complaints through the CSM, and proves little on its own. A long-tail or mid tenant that files everything else and nothing here is the one that behaves differently.
- **Inverted.** Segment shares against the queue's shares. A key account under its share is not a smaller problem; say what their silence hides.
- **Timing.** Months active, and whether the tickets start, stop or continue at a release. Read the release notes for the area; where one says the thing was fixed, split the kept tickets by date around it.
- **One operator.** If every kept ticket is from one tenant, say so first. However many tickets, that is one voice.

## 4. The pack

Print it; write it to `<synthesis folder>/<date>-dig-<slug>.md` only if the person asked for a file.

- **Hypothesis**, as restated.
- **Verdict**, one line: supported across N tenants in M segments; one operator; not found in the queue. Never more certain than the count allows.
- **Tickets**, grouped by tenant with segment, one line per ticket: id, date, role, the intent in your words.
- **Quotes**, three to five, the ones that carry it, translated with the original in brackets when not in English, with id, role and segment. Include the support reply where it shows what customers are being told.
- **Absent**, **Inverted**, **Timing**, one line each from step 3.
- **Where else to look**: which other signal sources should show it if it is real, named by path (reviews, member feedback, NPS verbatims, CSM notes, Slack, the platform exports by file), and what each would add. You do not open them; two independent sources is the rule and this pack is one.
- **Search**: the regexes run and the hit and kept counts per pass, so the dig can be repeated.

## 5. Check

- every id in the pack is in the `ids` output of the final pass
- every quote is in the ticket it cites, word for word
- the absent tenants are the ones `tenants` listed, and the control case, if any, is named
- the verdict's count matches the tenant count in the pack
- nothing in the pack proposes a fix

## 6. Close

Three lines:

```
Hypothesis: <one line>   Verdict: <one line>   Tickets: <n> across <tenants> tenants
Control case: <tenant and what to ask, or none>
Confirm in: <the one other source most likely to settle it>
```
