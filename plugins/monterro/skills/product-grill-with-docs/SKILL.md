---
name: product-grill-with-docs
disable-model-invocation: true
description: The readiness gate. Check a ticket against the product ticket standard and move it to ready-for-handover, or leave it in discovery with the failing checks named.
---

# product-grill-with-docs

The readiness gate. It is the only thing allowed to move a ticket from `discovery` to `ready-for-handover`. Everything upstream (idea-to-ticket, advance-ticket, the prioritization gate) prepares the ticket; this skill decides whether the ticket is a fit artifact for engineering to receive, and says exactly why when it is not.

Two rules:

- **Every finding names a check.** A bounce is a list of failing checks from `references/checks.md`, each with the smallest fix. Never a bare status change, never "needs work".
- **The ticket is the record.** The verdict goes onto the ticket as a comment. A verdict that lives only in chat has not happened.

## 1. Load the yardsticks

Paths come from `.claude/monterro.json`: the ticket home from `ticket_home` (the tracker team in `tracker`, or `tickets_dir` when it is files), product areas, principles and non-goals from `context_file`, the engineering glossary from `glossary`. If the config is missing, say so in one sentence and stop.

The yardsticks, in this order:

- `product-ticket-standard.md`: the standard. Section 7 is the checklist; `references/checks.md` restates it as pass/fail rules and is the fallback if the standard is not found.
- `context_file`: capabilities, principles, non-goals, product areas.
- `decisions`: the decision log. Absent is fine and is reported as "no decision log".
- The engineering glossary. If unreachable, check 12 is skipped and skipped counts as failing. It is the only vocabulary source; nothing in the product repo stands in for it.

Then fetch the ticket through the tracker MCP by ID or link, or take pasted content. One ticket per run.

## 2. Run the checks

Walk `references/checks.md` top to bottom and record pass, fail or skipped for each, with a one-line reason for anything other than pass. Run all of them: the PM wants the whole bill at once, because fixing one and re-running for the next is the slow way.

For an S, only checks 1, 2, 5, 7, 10, 11 and 12 apply. The rest are marked not-applicable.

Checks 11 and 12 read outside the ticket. A contradiction with CONTEXT.md, a decision-log entry in the ticket's product area, or the glossary's meaning of a term fails. Where those sources are silent on something the ticket asserts, the ticket does not fail; note it in one line under Notes.

## 3. Verdict

All applicable checks pass: set status `ready-for-handover`, remove the `needs-more-info` label if present, post the verdict block, and say the ticket is now with `/monterro:handover-triage`.

Any check fails or is skipped: leave the status at `discovery`, add the `needs-more-info` label, and post the verdict block. Skipped counts as failing; a gate that passes on an unreadable glossary is not a gate.

Do not fix the ticket. Draft the fix in the verdict where it is short (a rewritten Intent sentence, an acceptance scenario), and leave the edit to the PM. The gate checks; the author writes.

## 4. Verdict block

```
Grill: <ticket id> <title>, tier <S/M/L>
Verdict: ready-for-handover | needs-more-info (stays in discovery)
Failing: <check number and name: what fails, smallest fix>  (one line each, or "none")
Skipped: <check number: why>  (or "none")
Notes: <where CONTEXT.md, the decision log or the glossary were silent, one line each>  (or "none")
```

Post it as a comment addressed to the ticket owner. If the tracker is read-only, deliver it in the conversation and say it was not written to the ticket.
