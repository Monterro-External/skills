# Diagnosis ladder

Walk top to bottom; the diagnosis is the first stage whose exit condition fails. Exit conditions are written to be checkable against the ticket text alone.

## Stage 1: Structure

**Exit condition:** the ticket has the Product Ticket sections (intent, problem and evidence, outcome, scope, the decisions, acceptance scenarios, open questions, and at L success criteria and migration intent) and the fields (type, tier, product_area, status), even if some are placeholders. Verbatim original signal present.

**Fails →** next action is restructure: map existing text into the standard's sections, mark all content unconfirmed, show the PM before writing back. If the ticket is actually a bug, route it to bug intake instead.

## Stage 2: Evidence

**Exit condition:** every claim in Problem and evidence resolves to a live link or an attached artifact. Named-but-unlinked sources ("retro notes, link needed") fail. An honest "None yet, to be gathered in discovery" also fails this stage, and that is the point: the stage's next action is to gather it.

**Fails →** next action is evidence gathering: draft the request messages, search connected sources for candidate links, and if the claim cannot be evidenced at all, recommend demoting it to an assumption. Baseline questions (volumes, time spent) belong here too.

## Stage 3: Open questions

**Exit condition:** three or fewer open questions, each still genuinely deferrable and none of them blocking an acceptance scenario. More than three means the ticket is still in early discovery. Owners and dates are worth adding while a question is being chased, but the ticket does not require them.

**Fails →** next action: answer, re-own, or demote. Questions a reasonable default resolves become assumptions with the default stated. Expired questions get either an answer today or a new honest date.

## Stage 4: Validation

**Exit condition:** the success criteria are no longer bare hypotheses: each is either validated (with a result linked), being tested (with the test named), or explicitly accepted as a launch hypothesis by the PM (stated in the ticket). The solution question is settled enough for the ticket's ambition: either a validated direction, or a deliberate decision to hand over with open solution space (wayfinder route).

**Build-versus-configure check, part of this stage:** when the parked candidate resembles capability an existing tool, platform feature, or vendor already provides, compare that existing mechanism against the acceptance scenarios before proposing any new validation or ideation. Compare what it does as documented and as a user sees it, never by reading its code. Configuration that satisfies the scenarios ends the ticket's discovery early, and that is a success.

**Fails →** next action: the minimum sufficient test for this tier and audience. Internal audience: a poll or a corridor test. M-tier customer feature: a handful of ICP interviews (validate-value scaled down). L-tier: the full validate-value machinery.

## Stage 5: Gate

**Exit condition:** `gate_decision` has a date, `door` is set, and kill criteria exist.

**Fails →** next action: assemble the gate package and put the decision in front of the gate owner. This skill prepares; a human decides. Never fill these fields from a skill run.

## Stage 6: Grilling

**Exit condition:** the readiness checks pass (the grill's `references/checks.md`, or the configured `standard`): no implementation detail, measurable success criteria, populated non-goals, error and regression scenarios present, no vague adjectives without numbers, three or fewer open questions, experience precedence declared, tier matches size, consistent with CONTEXT.md.

**Fails →** next action: run the grilling skill if present, otherwise apply the checklist inline and list each failing check with its smallest fix. CONTEXT.md silences are flagged as documentation gaps, not ticket defects.

## Stage 7: Handover

**Exit condition:** none; reaching this stage is the exit.

**Next action:** move the ticket to the handover state your flow uses and notify engineering triage. The routing (grilling versus wayfinder) is engineering's call, made from the ticket's shape.
