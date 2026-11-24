---
name: product-grill-with-docs
disable-model-invocation: true
description: The readiness gate. Check a ticket against the product ticket standard and move it to ready-for-handover, or leave it in discovery with the failing checks named.
---

# product-grill-with-docs

The readiness gate. It is the only thing allowed to move a ticket from `discovery` to `ready-for-handover`. Everything upstream (idea-to-ticket, advance-ticket, the prioritization gate) prepares the ticket; this skill decides whether the ticket is a fit artifact for engineering to receive, and says exactly why when it is not.

Three rules:

- **Every finding names a check.** A bounce is a list of failing checks from `references/checks.md`, each with the smallest fix. Never a bare status change, never "needs work".
- **Never comment on the ticket.** The verdict goes in the conversation. The only changes to the ticket are its state, the `needs-more-info` label, and the fixes the PM approved one by one in step 4.
- **Speak at the product altitude.** The PM reads every word of the output. Never cite source files, tables, endpoints or code paths in it, even when the code is how you found the problem: say what a user would see. What only engineering needs goes in one line for engineering, never in the reason for a failing check.

## 1. Load the yardsticks

Paths come from `.claude/monterro.json`: the tracker team or project from `tracker`, product areas, principles and non-goals from `context_file`, the engineering glossary from `glossary`. If the config is missing, say so in one sentence and stop.

The yardsticks, in this order:

- The ticket standard: `references/checks.md`, the bundled pass/fail rules. When `standard` in the config names a company's own ticket standard, its readiness checklist wins where the two disagree. No `standard` key is the normal case and is never reported.
- `context_file`: capabilities, principles, non-goals, product areas.
- `decisions`: the folder of Product Decision Records (PDRs). Read its `README.md` index, then the PDRs in the product areas at hand; only `accepted` PDRs decide anything, a `proposed` one is reported as pending. Absent is fine and is reported as "no PDRs". A PDR the ticket itself points to (`See PDR-nnnn`) that is still `proposed` goes under Before handover as "accept before handover", with its owner: the decision binds nothing until a human accepts it. It is a reminder, never a failing check.
- The engineering glossary. If unreachable, check 12 is skipped and skipped counts as failing. It is the only vocabulary source; nothing in the product repo stands in for it.

Then fetch the ticket through the MCP for `tracker.kind`, never another tracker's, by ID or link, or take pasted content. One ticket per run.

## 2. Run the checks

Walk `references/checks.md` top to bottom and record pass, fail or skipped for each, with a one-line reason for anything other than pass. Run all of them: the PM wants the whole bill at once, because fixing one and re-running for the next is the slow way.

For an S, only checks 1, 2, 5, 7, 10, 11 and 12 apply. The rest are marked not-applicable.

Checks 11 and 12 read outside the ticket. A contradiction with CONTEXT.md, a decision-record entry in the ticket's product area, or the glossary's meaning of a term fails. Where those sources are silent on something the ticket asserts, the ticket does not fail; note it in one line for engineering / CPO, and only when it is worth recording. The one exception is a domain term the glossary lacks: check 12 passes it only when the ticket defines it under Proposed terms, and a defined term still goes to engineering as a glossary gap.

## 3. First verdict

All applicable checks pass: go to step 5.

Any check fails or is skipped: open with one line, the count and the kind of work, such as "Fails 4 of 12, all wording fixes plus one missing link." Then go to step 4. Do not print the failing checks as a list first; step 4 shows them one at a time.

## 4. Fix together, one check at a time

Walk the failing checks in number order. For each, one message:

- **The check** by number and name.
- **Why it fails**, one sentence at the product altitude.
- **The proposed fix**: the exact replacement text for the ticket, as a quote the PM can read as it would appear, with the section it goes in. Where the fix needs something only the PM has (a link, a number, a name), say what is needed instead of inventing it.

Then ask with the structured question tool where the client has one, otherwise as one plain question in chat. At most three options, the recommended one first: apply the proposed fix; the alternative fix where there is a real one (drop the unsupported claim instead of linking it); skip for now. The PM's own wording, or the link they paste, comes through the free-text answer. One check per question; never batch them.

On an apply, or on the PM's own wording, edit only that section of the ticket through the tracker MCP, say in one line what changed, and move to the next check. On a skip, move on; it stays failing. A skipped check (an unreachable glossary) cannot be fixed here: name it and move on without asking.

You draft and the PM decides. Never apply a fix the PM has not chosen, and never edit a section no failing check named.

## 5. Final verdict

After the walk, fetch the ticket again and run every applicable check on the text as it now stands, not on what you remember editing. A fix can break another check.

All applicable checks pass: set status `ready-for-handover`, remove the `needs-more-info` label if present, post the verdict, and say the ticket is now with engineering: an engineer reads it at handover and routes it to grill-with-docs or wayfinder, or sends it back.

Any check fails or is skipped: leave the status at `discovery`, add the `needs-more-info` label, and post the verdict. Skipped counts as failing; a gate that passes on an unreadable glossary is not a gate.

Status names come from `tracker.states`. In Jira a status moves by transition: take the transition whose target status has that name, whatever the transition itself is called, and if none leads there from where the ticket is, leave it and say which move to make by hand.

## 6. Verdict format

Plain markdown in the conversation, addressed to the ticket owner, never in a code block and never as a ticket comment:

**<ticket id> <title>, tier <S/M/L>: ready-for-handover** (or **needs-more-info, stays in discovery**)

- Fixed in this session: check numbers and names, one line (or omit)
- Still failing: per check, its number and name, why in one sentence, and the fix as a quote (or omit)
- Skipped: per check, why (or omit)

**Before handover** (at most three, only what the PM must act on): PDRs the ticket points to that are still proposed, as "PDR-nnnn: accept before handover, owner <name>"; anything else that blocks the ticket from moving.

**For engineering / CPO**: one line each for glossary gaps the ticket defines, and silences in CONTEXT.md or the decision records worth recording. Drop a silence that changes nothing for anyone.

Name the checks that passed only by count ("8 others pass").
