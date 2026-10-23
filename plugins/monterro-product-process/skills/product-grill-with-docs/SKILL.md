---
name: product-grill-with-docs
disable-model-invocation: true
description: The readiness gate. Check a ticket against the product ticket standard and move it to ready-for-handover, or leave it in discovery with the failing checks named.
---

# product-grill-with-docs

The readiness gate. It is the only thing allowed to move a ticket from `discovery` to `ready-for-handover`. Everything upstream (idea-to-ticket, advance-ticket, the prioritization gate) prepares the ticket; this skill decides whether the ticket is a fit artifact for engineering to receive, and says exactly why when it is not.

Two rules:

- **Every finding names a check.** A bounce is a list of failing checks from `references/checks.md`, each with the smallest fix. Never a bare status change, never "needs work".
- **The verdict goes in the conversation.** Never comment on the ticket. The state and the `needs-more-info` label are the only things this skill changes on it.

## 1. Load the yardsticks

Paths come from `.claude/monterro.json`: the ticket home from `ticket_home` (the tracker team in `tracker`, or `tickets_dir` when it is files), product areas, principles and non-goals from `context_file`, the engineering glossary from `glossary`. If the config is missing, say so in one sentence and stop.

The yardsticks, in this order:

- The ticket standard: `references/checks.md`, the bundled pass/fail rules. When `standard` in the config names a company's own ticket standard, its readiness checklist wins where the two disagree. No `standard` key is the normal case and is never reported.
- `context_file`: capabilities, principles, non-goals, product areas.
- `decisions`: the folder of Product Decision Records (PDRs). Read its `README.md` index, then the PDRs in the product areas at hand; only `accepted` PDRs decide anything, a `proposed` one is reported as pending. Absent is fine and is reported as "no PDRs".
- The engineering glossary. If unreachable, check 12 is skipped and skipped counts as failing. It is the only vocabulary source; nothing in the product repo stands in for it.

Then fetch the ticket through the MCP for `tracker.kind`, never another tracker's, by ID or link, or take pasted content. One ticket per run.

## 2. Run the checks

Walk `references/checks.md` top to bottom and record pass, fail or skipped for each, with a one-line reason for anything other than pass. Run all of them: the PM wants the whole bill at once, because fixing one and re-running for the next is the slow way.

For an S, only checks 1, 2, 5, 7, 10, 11 and 12 apply. The rest are marked not-applicable.

Checks 11 and 12 read outside the ticket. A contradiction with CONTEXT.md, a decision-record entry in the ticket's product area, or the glossary's meaning of a term fails. Where those sources are silent on something the ticket asserts, the ticket does not fail; note it in one line under Notes. The one exception is a domain term the glossary lacks: check 12 passes it only when the ticket defines it under Proposed terms, and a defined term still goes under Notes as a glossary gap.

## 3. Verdict

All applicable checks pass: set status `ready-for-handover`, remove the `needs-more-info` label if present, post the verdict block, and say the ticket is now with `/monterro-product-process:handover-triage`.

Any check fails or is skipped: leave the status at `discovery`, add the `needs-more-info` label, and post the verdict block. Skipped counts as failing; a gate that passes on an unreadable glossary is not a gate.

Status names come from `tracker.states`. In Jira a status moves by transition: take the transition whose target status has that name, whatever the transition itself is called, and if none leads there from where the ticket is, leave it and say which move to make by hand.

Do not fix the ticket. Draft the fix in the verdict where it is short (a rewritten Intent sentence, an acceptance scenario), and leave the edit to the PM. The gate checks; the author writes.

## 4. Verdict block

```
Grill: <ticket id> <title>, tier <S/M/L>
Verdict: ready-for-handover | needs-more-info (stays in discovery)
Failing: <check number and name: what fails, smallest fix>  (one line each, or "none")
Skipped: <check number: why>  (or "none")
Notes: <where CONTEXT.md, the decision records or the glossary were silent, one line each; glossary gaps the ticket defines, for engineering>  (or "none")
```

Print it in the conversation, addressed to the ticket owner. Never post it as a ticket comment.
