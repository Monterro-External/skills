---
name: brainstorm-ideas
disable-model-invocation: true
description: "Runs a structured divergence and convergence pass over a customer pain, ending in idea cards with named riskiest assumptions and a discard list."
---

# Brainstorm Ideas

This is a manual skill. It runs when the user calls it, and only then. If it has somehow triggered off ordinary ideation language rather than a deliberate invocation, stop and answer the user's actual question instead.

Generate and then ruthlessly filter solution ideas for a specific pain.

Rule one: brainstorm against a pain, never against a technology or a blank page. If the input is "let's do something with AI", first extract which pain it should serve. If no pain can be named, stop and say so; the fix is more evidence, not more ideas.

## Inputs

This skill runs on whatever pain evidence is available. Good sources, in rough order of strength: user interviews and research notes, support ticket patterns, sales call objections and lost-deal reasons, usage and retention data, NPS or in-app verbatims, and direct observation. Weak sources: a single loud customer, an internal opinion, a competitor's feature list.

If the user has none of these, run anyway but mark the frame as unvalidated and flag that the top ideas should be tested for value before anything gets built.

## Process

### 1. Frame

State, in one sentence each:

- **The pain**, with its evidence and how strong that evidence is
- **The persona** who has it, specifically enough that you could go find one
- **The outcome** that would count as solved, stated as a change in behavior or a metric

Confirm this frame with the user before generating. A wrong frame makes every idea downstream wrong, and the frame is cheap to fix now and expensive to fix later.

### 2. Diverge

Generate widely before judging anything. Run at least three of these lenses, more if the crop is thin:

- **PM lens**: what would move the metric fastest?
- **Designer lens**: what would remove the friction entirely, including by removing steps, or the feature itself?
- **Engineer lens**: what does our architecture or data make nearly free that competitors would find hard?
- **Constraint rounds**: what if it had to ship in two weeks? What if we could not add any UI? What if the user never had to think about it at all (full automation)?
- **Theft round**: how do best-in-class products in *other* categories solve this shape of problem?

Target 15 to 25 raw ideas. No filtering, no "yes but" during this step. Include at least two non-build ideas covering pricing, packaging, documentation, onboarding, or a service change. The best solution to a product pain is sometimes not a feature.

### 3. Converge

Filter in two passes.

**Strategy pass.** Kill ideas that fail the product strategy, the product principles, or fit with the ideal customer profile. Name which filter killed each one. If the team has no written strategy or principles to filter against, say so plainly and filter on stated goals instead, noting that the filter is weaker than it looks.

**Leverage pass.** Score the survivors on impact on the pain, effort class (S/M/L), and confidence. Keep the top 3 to 5.

### 4. Attach the risk

For each surviving idea, write its **riskiest assumption**: the single belief that, if false, kills the idea. Tag it as one of:

- **value**: they will not want it or will not pay
- **usability**: they cannot figure out how to use it
- **feasibility**: we cannot build it with what we have
- **viability**: it works for users but breaks the business, legally, commercially, or operationally

Then name the **cheapest test** that would settle that assumption. Cheapest means fastest and least committing, not most rigorous. A five-customer interview, a fake door, a clickable prototype, or a spike beat a quarter of building.

## Deliver

Two artifacts, both in the response:

**Idea cards**, one per survivor:

```
Idea:                one line, what it is
Pain served:         the framed pain
Riskiest assumption: the belief that kills it if false
Risk type:           value / usability / feasibility / viability
Cheapest test:       what would settle it, and roughly how long
Effort class:        S / M / L
```

**The discard list**: every killed idea in one line, with the reason it died. This matters more than it looks. The discard list is what stops the same ideas resurfacing next quarter and consuming the same meeting again.

Close by naming what each surviving card needs next, based on its risk type: value risk needs customer evidence, usability risk needs something clickable in front of a user, feasibility risk needs an engineering spike, viability risk needs a commercial or legal check. Recommend, do not start. The user decides what runs next.
