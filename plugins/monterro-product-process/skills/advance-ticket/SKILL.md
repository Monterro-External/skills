---
name: advance-ticket
disable-model-invocation: true
description: Diagnose where a Product Ticket stands and prepare the cheapest next action toward handover. Not for new tickets (idea-to-ticket) or the readiness gate (product-grill-with-docs).
---

# advance-ticket

The navigator. A PM's problem with a ticket in discovery is rarely doing the next step; it is knowing which step is next and re-entering context after a week away. This skill reads the ticket, diagnoses its stage, and hands the PM the cheapest next action with as much of it already prepared as honesty allows.

Three standing rules shape everything below:

- **Prepare decisions, never make them.** The flow has human doors: the prioritization gate verdict and the solution commit. This skill assembles what those decisions need and then stops, naming the human who decides. It never sets `gate_decision`, never marks a ticket ready-for-handover, and never advances a status across a door.
- **Stay on the product side.** Never read source code, the engineering repo (beyond the glossary), logs or the database, and never write a technical root-cause hypothesis or a fix into the ticket or the diagnosis. Why the system behaves as it does is engineering's question, answered after handover by `/grill-with-docs` or `/wayfinder`. Existing mechanisms are checked through what the product documents and ships: `context_file`, the PDRs, help documentation, release notes, and the product as a user sees it. A root-cause theory the PM brings goes in the ticket as one line marked non-binding, for engineering, never as evidence.
- **The diagnosis goes in the conversation.** Never comment on the ticket. What the ticket should keep is offered as updated sections, written only once the PM confirms.

## 1. Read the context, then the ticket

Paths come from `.claude/monterro.json`: the tracker team or project from `tracker`, product areas, principles and non-goals from `context_file`, the engineering glossary from `glossary`. If the config is missing, say so in one sentence and stop.

The yardstick is the bundled `references/diagnosis-ladder.md`. When `standard` in the config names a company's own ticket standard, its section structure and readiness gate win where they differ. No `standard` key is the normal case and is never reported.

Fetch the ticket through the tracker `tracker.kind` names (Linear or Jira MCP), never the other one, by the ID or link the PM gave. If the tracker is not reachable, ask for the ticket content pasted. Work on one ticket per invocation; given several, ask which one, or take the one the PM named first.

If the ticket does not follow the Product Ticket structure at all (no sections, no fields, freeform text), do not force the ladder onto it. Say so, and offer the restructure as the next action: map what text exists into the standard's sections, mark everything unconfirmed, and show the PM the result for approval before writing it back. A diagnosis pretending a freeform ticket is an embryo produces confident nonsense.

## 2. Diagnose: walk the ladder, stop at the first gap

Walk `references/diagnosis-ladder.md` top to bottom and stop at the first stage whose exit condition fails. That stage is the diagnosis; everything below it is noise until it clears. Resist the urge to list every imperfection in the ticket: a PM handed twelve findings does none of them. One stage, one next action, everything else at most a one-line footnote.

Footnotes run in both directions. A finding about a **later** stage belongs there too, when the run established it as fact and it changes how that stage will be done: a source the ticket does not cite, a product feature that already covers part of the need and may end the discovery early. Never a finding from code. It stays a footnote. It never becomes a second next action, and it is never speculation about a stage the ticket has not reached, which is the bill of findings this rule exists to prevent. The test is whether the run verified it, not whether it is interesting.

The ladder in brief (the reference file carries the full exit conditions):

1. **Structure**: is it a conforming Product Ticket embryo?
2. **Evidence**: does every claim carry a live link? "Link needed" and named-but-unlinked sources fail this.
3. **Open questions**: are they down to three or fewer, and is each one still genuinely deferrable?
4. **Validation**: do the success-criteria hypotheses have a validation result, or at least a planned test? Include the build-versus-configure check here when the parked solution resembles something an existing tool or platform already does: thirty minutes of checking existing mechanisms against the acceptance scenarios beats any amount of ideation.
5. **Gate**: is there a gate decision (door, verdict, date)? If everything above is clear and this is missing, the next action is the gate, and it is a human's.
6. **Grilling**: has the readiness gate passed? If a grilling skill exists, point there; otherwise apply the checklist inline and report failing checks.
7. **Handover**: everything passed. The next action is handing to engineering triage, and the diagnosis says so plainly.

Scale the recommended action to the ticket's tier and audience. An M-tier internal ticket clears its validation stage with a three-question team poll; an L-tier pricing change needs the full machinery. Recommending ceremony a size too big is as much a failure as skipping a stage.

## 3. Prepare the action, honestly

Having named the next action, do the preparable share of it on the spot:

- Evidence gap → draft the exact request messages (who to ask, for what, where it likely lives), and if the connected tools can search the named sources (Slack, meeting transcripts, support systems), run the search and bring back candidate links for the PM to confirm. Candidates are suggestions; only the PM confirms what counts as evidence.
- Open validation → draft the minimum sufficient test: the poll text, the five interview questions, the build-versus-configure comparison table against the acceptance scenarios.
- Gate next → assemble the gate package: the evidence summary with links, the door classification argument both ways, the kill-criteria candidates. End with "the verdict is yours" and the named owner.
- Grilling next → run the checklist and list failing checks with the smallest fix for each.

Never manufacture what preparation cannot honestly produce: no invented links, no estimated baselines presented as measured, no poll results predicted. A prepared action with honest blanks beats a completed-looking one with quiet fabrications.

## 4. Deliver, and offer section updates

Print the diagnosis block (format below) in the conversation. Never post it, or anything else, as a ticket comment. Then:

- Offer section updates only for facts the run actually established (a confirmed link, an answered OQ with its answer and source), and write them once the PM confirms.
- Where a decision already made in the ticket meets the bar in `references/pdr.md` and has no PDR, offer to move it into one: write the file in the `decisions` folder with status `proposed`, and replace the decision in the ticket with `See PDR-nnnn: <title>`. Recording a decision is not making it; the PM confirms first, and only a human sets `accepted`.
- Leave every status and every field that belongs to a door (`gate_decision`, `door`, handover statuses) untouched.

If the tracker is read-only or the PM asked for advice only, skip the section updates and say so.

## 5. Diagnosis block format

```
Ticket: <id> <title>
Stage: <ladder stage> (<n> of 7)
Blocking: <the one gap, one line>
Next action: <what, who, and the effort in honest units: minutes, hours, or a meeting>
Prepared: <what this run drafted or found, as links or attachments>
Door ahead: <the next human decision on the horizon and its owner>
Footnotes: <at most two one-liners, or "none". Either direction: a lower-ladder issue too small to be the diagnosis, or a fact this run established about a later stage. A forward footnote names its stage and says what was found, never what should be decided there>
```

The whole block fits on one screen. If it does not, the diagnosis is trying to be a review; cut it back to the one blocking gap.
