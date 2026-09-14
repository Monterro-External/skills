---
name: advance-ticket
disable-model-invocation: true
description: Read a Product Ticket, diagnose where it stands against the product-ticket-standard, and name and prepare the cheapest next action to move it toward engineering handover. Use whenever a PM asks what's next for a ticket, wants to move a ticket forward, advance a ticket, unblock a ticket, get a status diagnosis of a product ticket in discovery, or pastes a ticket link or ID asking what to do with it. Also use when someone asks "is this ticket ready for the gate" or "is this ready for handover". Not for creating new tickets (that is idea-to-ticket or signal intake) and not the final quality gate itself (that is product grilling).
---

# advance-ticket

The navigator. A PM's problem with a ticket in discovery is rarely doing the next step; it is knowing which step is next and re-entering context after a week away. This skill reads the ticket, diagnoses its stage, and hands the PM the cheapest next action with as much of it already prepared as honesty allows.

Two standing rules shape everything below:

- **Prepare decisions, never make them.** The flow has human doors: the prioritization gate verdict and the solution commit. This skill assembles what those decisions need and then stops, naming the human who decides. It never sets `gate_decision`, never marks a ticket ready-for-handover, and never advances a status across a door.
- **The ticket is the single source of truth.** Everything this skill learns or prepares is offered back into the ticket (as a comment or updated sections), not left in the chat. A diagnosis that lives only in a conversation is lost by next week.

## 1. Read the context, then the ticket

Paths come from `.claude/monterro.json`: the ticket home from `ticket_home` (the tracker team in `tracker`, or `tickets_dir` when it is files), product areas, principles and non-goals from `context_file`, the engineering glossary from `glossary`. If the config is missing, say so in one sentence and stop.

The standard's section structure and readiness gate are the yardstick; the bundled `references/diagnosis-ladder.md` is the fallback when no standard file is found.

Fetch the ticket through the connected tracker (Linear or Jira MCP) by the ID or link the PM gave. If no tracker is reachable, ask for the ticket content pasted, or accept a file. Work on one ticket per invocation; given several, ask which one, or take the one the PM named first.

If the ticket does not follow the Product Ticket structure at all (no sections, no fields, freeform text), do not force the ladder onto it. Say so, and offer the restructure as the next action: map what text exists into the standard's sections, mark everything unconfirmed, and show the PM the result for approval before writing it back. A diagnosis pretending a freeform ticket is an embryo produces confident nonsense.

## 2. Diagnose: walk the ladder, stop at the first gap

Walk `references/diagnosis-ladder.md` top to bottom and stop at the first stage whose exit condition fails. That stage is the diagnosis; everything below it is noise until it clears. Resist the urge to list every imperfection in the ticket: a PM handed twelve findings does none of them. One stage, one next action, everything else at most a one-line footnote.

The ladder in brief (the reference file carries the full exit conditions):

1. **Structure**: is it a conforming Product Ticket embryo?
2. **Evidence**: does every claim carry a live link? "Link needed" and named-but-unlinked sources fail this.
3. **Open questions**: are they down to three or fewer, and is each one still genuinely deferrable?
4. **Validation**: do the success-criteria hypotheses have a validation result, or at least a planned test? Include the build-versus-configure check here when the parked solution resembles something an existing tool or platform already does: thirty minutes of checking existing mechanisms against the acceptance scenarios beats any amount of ideation.
5. **Gate**: is there a gate decision (door, verdict, date)? If everything above is clear and this is missing, the next action is the gate, and it is a human's.
6. **Grilling**: has the readiness gate (standard section 7) passed? If a grilling skill exists, point there; otherwise apply the checklist inline and report failing checks.
7. **Handover**: everything passed. The next action is handing to engineering triage, and the diagnosis says so plainly.

Scale the recommended action to the ticket's tier and audience. An M-tier internal ticket clears its validation stage with a three-question team poll; an L-tier pricing change needs the full machinery. Recommending ceremony a size too big is as much a failure as skipping a stage.

## 3. Prepare the action, honestly

Having named the next action, do the preparable share of it on the spot:

- Evidence gap → draft the exact request messages (who to ask, for what, where it likely lives), and if the connected tools can search the named sources (Slack, meeting transcripts, support systems), run the search and bring back candidate links for the PM to confirm. Candidates are suggestions; only the PM confirms what counts as evidence.
- Open validation → draft the minimum sufficient test: the poll text, the five interview questions, the build-versus-configure comparison table against the acceptance scenarios.
- Gate next → assemble the gate package: the evidence summary with links, the door classification argument both ways, the kill-criteria candidates. End with "the verdict is yours" and the named owner.
- Grilling next → run the checklist and list failing checks with the smallest fix for each.

Never manufacture what preparation cannot honestly produce: no invented links, no estimated baselines presented as measured, no poll results predicted. A prepared action with honest blanks beats a completed-looking one with quiet fabrications.

## 4. Write back

Offer the diagnosis to the ticket, not just the chat:

- Post the diagnosis block (format below) as a comment on the ticket, so the next person who opens it sees where it stands.
- Update ticket sections only with facts the run actually established (a confirmed link, an answered OQ with its answer and source). Mark anything the PM has not confirmed as proposed.
- Leave every status and every field that belongs to a door (`gate_decision`, `door`, handover statuses) untouched.

If the tracker is read-only or the PM asked for advice only, deliver the same block in the conversation and say it was not written to the ticket.

## 5. Diagnosis block format

```
Ticket: <id> <title>
Stage: <ladder stage> (<n> of 7)
Blocking: <the one gap, one line>
Next action: <what, who, and the effort in honest units: minutes, hours, or a meeting>
Prepared: <what this run drafted or found, as links or attachments>
Door ahead: <the next human decision on the horizon and its owner>
Footnotes: <at most two one-liners on lower-ladder issues, or "none">
```

The whole block fits on one screen. If it does not, the diagnosis is trying to be a review; cut it back to the one blocking gap.
