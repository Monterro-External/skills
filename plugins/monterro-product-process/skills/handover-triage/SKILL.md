---
name: handover-triage
disable-model-invocation: true
description: Engineering's gate. Read a ticket at ready-for-handover against the code, then hand it to engineering or return it to discovery with one reason code. Waits for an ok before writing.
---

# handover-triage

The grill asked whether the ticket meets the standard. This asks whether engineering can start on it, and which way. It is the only skill that moves a ticket out of `ready-for-handover`, forward to `in-development` or back to `discovery`.

Four rules:

- **Route on shape, never on a field.** Product does not propose a route. A ticket that already carries `handover_route` has a workflow error; say so and route it as if the field were absent.
- **Read what the grill could not.** The grill reads documents. This skill reads the code, so a ticket's claim about what the product does today is checked where the product does it. A passed grill is not evidence the claim is true.
- **A bounce carries one reason code** and the asks that would change the verdict. Never a bare label, never "needs work".
- **The ticket is the record.** A verdict that lives only in chat has not happened.

## 1. Load

From `.claude/monterro.json`: the tracker block (team or project, states, labels); `context_file`; `glossary`. The engineering repo is the repository `glossary` points into; read its `CLAUDE.md` or `AGENTS.md`, `docs/adr/`, and the code. If the config is missing, or the engineering repo is not readable, say so in one sentence and stop: a handover triage that cannot read the code is the grill run twice.

Fetch the ticket through the MCP for `tracker.kind`, never another tracker's, by id or link, with its comments, or take pasted content. One ticket per run. If it is not in `ready-for-handover`, say where it is and stop; triage never runs on a ticket the grill has not passed.

## 2. Read it as engineering

Walk `references/triage.md` top to bottom: the code read, the four bounce tests in order, then the route. Record each test as pass or fail with a one-line reason. The first test that fails decides the reason code; any later failure goes in the note, not in a second code.

Do not design. Where the code shows how a claim could be satisfied, that is engineering's material for the Spec and it stays out of the verdict.

## 3. Verdict, then wait

Print the verdict block and one line: "Reply 'ok' to write this, or say what to change." Nothing in the tracker changes before the answer. Unattended, print and stop.

On **ok**:

- **Route**: status `in-development`; add `handover_route` (`grilling` or `wayfinder`) and `handed_over` (today) to the ticket's fields; post the verdict block as a comment. Create nothing in the engineering team. `/grill-with-docs` then `/to-spec`, or `/wayfinder`, take the Product Ticket as their input and create the Spec issue or the map, which links back per the engineering repo's tracker doc. A stub made here would give one change two roots. `spec_ref` is written when the Spec exists.
- **Bounce**: status `discovery`, label `needs-more-info`, and the verdict block as a comment addressed to the ticket owner, carrying `source: handover-triage` and the reason code. The grill removes the label when the ticket passes again.

If the tracker is read-only or the ticket was pasted, deliver the block and say it was not written.

Status names come from `tracker.states`. In Jira a status moves by transition: take the transition whose target status has that name, whatever the transition itself is called, and if none leads there from where the ticket is, leave it and say which move to make by hand.

## 4. Verdict block

```
Handover triage: <ticket id> <title>, tier <S/M/L>
Source: handover-triage
Verdict: grilling | wayfinder | discovery, reason <code>
Code: <each claim about current behaviour: where it lives, agrees | contradicts>  (or "no claims about current behaviour")
Tests: <failing test: what fails>  (one line each, or "all four pass")
Shape: <the signals that decided the route, one line each>
Asks: <bounce: numbered, each one something the ticket can answer>  (or "none")
Start with: <route: the first questions the grilling session or the map should open>  (or "none")
Parked: <directions raised and not chosen, binding nobody>  (or "none")
```

One screen. The owner reads it cold, a quarter later, next to every other bounce.
