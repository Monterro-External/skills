---
name: solution-tournament
disable-model-invocation: true
description: Rank alternative solutions for a ticket by blind pairwise judging against the ticket's outcome, ending in a favoured candidate and a validation plan. Refuses when the ticket's evidence is too thin.
---

# solution-tournament

De-anchor the first idea. A ticket usually arrives with one proposed solution that wins by default because nothing ever competed with it. This skill makes it compete: seed genuinely different alternatives, judge them blind against the ticket's own outcome and constraints, evolve the strongest, and hand back a favored candidate with the assumptions that make it win, as a validation plan, never as a decision. The solution commit stays a human door.

One epistemic rule governs everything: **the judges judge against the ticket's evidence, so if the ticket has no evidence, the tournament is theater.** That is why the readiness check comes first and is allowed to say no.

## 1. Readiness check: be frank, or the rest is fiction

Paths come from `.claude/monterro.json`: the ticket home from `ticket_home` (the tracker team in `tracker`, or `tickets_dir` when it is files), product areas, principles and non-goals from `context_file`, the engineering glossary from `glossary`. If the config is missing, say so in one sentence and stop.

Fetch the ticket from the ticket home by ID or link, else ask for pasted content, and read `context_file`, plus the ticket standard named by `standard` in the config when there is one. Then check for the three signals a tournament needs:

1. **A judgeable problem**: the problem statement is specific enough that two ideas could differ in how well they solve it, and at least one piece of evidence is linked (or the original verbatim signal plus a PM-confirmed problem statement exists).
2. **A judgeable outcome**: an outcome metric or measurable success criteria, hypotheses are fine, "make it better" is not.
3. **Judgeable constraints**: the tier and the non-goals, so feasibility and viability can be scored against something real.

**If any of the three is missing, do not run.** Say so plainly, without apology and without a token attempt. Produce a short readiness note instead:

- Which signal is missing, and one sentence on why the tournament cannot substitute for it (judges scoring ideas against a vague outcome are ranking prose style, not likelihood of success).
- Exactly where on the ticket to add it and how: evidence links into Problem and evidence (section 2), a measurable outcome into Outcome and success criteria (section 3), non-goals into Scope (section 4). Name concrete sources where the context suggests them (support clusters, usage data, the team itself for internal tooling).
- The fastest route: running `advance-ticket` will diagnose and prepare exactly these gaps.
- Close with: fix these, then start the tournament again.

Offer to post the readiness note as a comment on the ticket. Then stop. The user re-initiates when the ticket is ready; the skill never quietly lowers the bar instead.

**Partial signals get a frank choice, not a silent decision.** If evidence is named but unlinked, or the outcome has no baseline, tell the PM and offer two routes: fix first (recommended, usually under an hour), or run in **provisional mode**, where the tournament proceeds but the fitness spec, every judgment, and the final report carry a visible flag that results rest on unconfirmed evidence. If the PM is unavailable (unattended run), do not run provisionally on your own initiative; deliver the readiness note.

**Also be frank in the opposite direction:** if the ticket already carries a validated solution direction (validation results linked, experience reference tested), say that a tournament would be reopening a settled question, name what new information would justify reopening, and ask before proceeding.

## 2. Phase 0: freeze the fitness spec

Write the yardstick once, before any idea exists, and never edit it during the loop; a moving target makes rounds incomparable. The fitness spec (see `references/rubric.md` for the template) contains:

- The outcome being optimized for, quoted from the ticket.
- Five dimensions: value, usability, feasibility, viability, strategy fit, each anchored to this ticket's specific evidence and constraints, not to generic definitions.
- The constraints that disqualify: non-goals, tier, product principles, CONTEXT.md capabilities.
- The provisional-mode flag if active.

Save it to a file. Every judge reads this file and nothing else about the process.

## 3. Phase 1: seed a diverse population

Generate the initial field through forced lenses (see `references/lenses.md`), one idea per lens, so diversity is structural rather than hoped for. The PM's parked candidate from the ticket enters as one competitor among equals, clearly included but given no incumbent status. Each idea is written to the same one-paragraph spec: the mechanism, who does what, what changes for whom, in plain language a judge can score. Five to seven ideas is the right field; more dilutes judging, fewer wastes the tournament.

## 4. Phase 2: the loop

Run rounds of judge, select, evolve. Full protocol in `references/rubric.md`; the load-bearing rules:

- **Judging is blind and comparative.** Judges see the fitness spec and the idea texts, never round numbers, provenance, previous rankings, or which idea was the PM's. They judge pairwise (A vs B per dimension, with a one-line reason) and every verdict names the assumption that would flip it. Aggregate pairwise results into a ranking. Never ask for or trust absolute probabilities; models rank far better than they estimate.
- **Selection keeps difference alive.** Keep the top two or three, with a diversity guard: if the leaders are variants of one concept, keep the best of them plus the strongest genuinely different idea, even if it ranked lower. A tournament of five shades of the same idea has already failed.
- **Evolution has three operators**: refine (fix the specific weakness the judges named), hybridize (combine two survivors' complementary strengths), challenge (design a new idea whose sole brief is to beat the current leader on its weakest dimension). Every round includes at least one challenge; a leader that has never been attacked has earned nothing.
- **Termination is rank stability, not rising scores.** Stop when the leader has survived two consecutive rounds including a challenge, or when judges can no longer consistently separate the top ideas (near-ties flipping between rounds), or at the round cap of five. Expect two to four rounds. A "keeps getting better" signal from absolute scores is inflation, ignore it.
- **Isolation mechanics**: run generators and judges as separate subagents when the environment offers them; otherwise run the roles strictly sequentially, writing each role's output to files and giving the judge role only the fitness spec and idea files, so blindness holds even in a single session.

Collect every flip-assumption named by any judge across all rounds; they become the validation plan.

## 5. Phase 3: the honest report, written back to the ticket

The winner is a favored candidate, not a validated solution, and the report never claims otherwise. It contains:

- **Winner and runner-up**, each with the dimensions it wins on and the one it is weakest on.
- **Load-bearing assumptions** per finalist, harvested from the judges' flip-assumptions, each paired with the cheapest test that would confirm or kill it. This section is the actual product of the tournament.
- **Close calls**: where the ranking was nearly a tie, say so; a coin-flip presented as a verdict is a lie with a bow on it.
- **Salvage**: insights from defeated ideas worth keeping, destined for the ticket as non-binding notes.
- **Provisional flag** if active, stating which unconfirmed evidence the whole ranking rests on.

Write back to the ticket: the report as a comment, the winner proposed (not silently installed) as a non-binding leading-candidate line at the end of the ticket, the assumptions as proposed additions to open questions or the validation plan. Statuses and door fields untouched. End by naming the door ahead: the solution commit and the validation that precedes it, owned by the PM, with `validate-value` as the natural next skill.

## Output summary format

```
Ticket: <id> <title>
Readiness: ran | provisional (flagged) | not ready (coached, stopped)
Rounds: <n> (terminated by: <stability | indistinguishable top | cap>)
Winner: <one line>   Runner-up: <one line>
Rests on: <the 2-3 load-bearing assumptions>
Next: validate <assumption> via <cheapest test>; solution commit is <owner>'s
Written back: <comment link / sections proposed / nothing (why)>
```
