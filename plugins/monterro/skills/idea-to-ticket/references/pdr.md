# Product Decision Records

A Product Decision Record (PDR) is a product decision that outlives the ticket it was made in, kept as a file so later tickets and features can be designed against it. Engineering keeps ADRs for how the system is built; PDRs record what the product is: who may do what, what is gated behind which plan, what happens to existing data, what the customer is told when something fails.

## When a decision earns a PDR

All three must be true:

1. **Binds beyond this ticket**: a later feature in the same product area would have to respect it.
2. **Surprising without context**: a PM new to the area would assume the opposite, or someone has already proposed the rejected alternative.
3. **A real trade-off**: there were genuine alternatives and one was picked for a reason.

Everything else stays a decision in its ticket. A PDR for every row makes an index nobody reads.

## Home and numbering

The folder named by `decisions` in `.claude/monterro.json` (default `strategy/decisions`), whatever the ticket home. One file per decision, `PDR-nnnn-slug.md`. Scan the folder for the highest number and add one. Never renumbered, never deleted. Create the folder and its `README.md` index lazily, when the first PDR is written.

`README.md` holds one line per PDR, newest last: `- [PDR-nnnn](PDR-nnnn-slug.md) · <product_area> · <status> · <title>`. Add the line when writing the file.

## Format

```md
---
id: PDR-nnnn
status: proposed
date: YYYY-MM-DD
owner:
product_area:
ticket:
---

# <The decision as a sentence>

<1-3 sentences: what was true when the decision was needed, what was decided, and why. The decision is stated as a rule the product follows, present tense, so a ticket can be checked against it.>

## Rejected alternatives

- <The alternative>: <why it lost.>

## Consequences

<Optional. Only when there is a non-obvious cost: who it disappoints, what it makes harder later.>
```

`ticket` is the id or link of the ticket the decision was made in. `product_area` uses the taxonomy in `context_file`, the same values as the ticket field.

## Status

A skill writes `proposed` and never moves status. A human sets `accepted`, after which the decision text does not change. A change of mind is a new PDR; the old one becomes `superseded` with a line pointing at the new id.

## In the ticket

The ticket's decisions section keeps one line per PDR, `See PDR-nnnn: <title>`, in place of the full decision. Decisions that do not earn a PDR stay written out in the ticket as before.
