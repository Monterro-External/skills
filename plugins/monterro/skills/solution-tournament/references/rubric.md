# Fitness spec template and judging protocol

## Fitness spec template

```markdown
# Fitness spec: <ticket id> (FROZEN <date>; provisional: yes/no)

Outcome optimized for (quoted from ticket): "<outcome metric / SC>"

Dimensions, each anchored to THIS ticket:
1. Value: how directly does the idea produce the outcome above, for <the persona/segment
   named in section 2>, given <the evidence: quote the strongest linked item>?
2. Usability: can <the actual persona> succeed with it, given how they work today
   (<from evidence/CONTEXT.md>)?
3. Feasibility: buildable or configurable within the tier this ticket earns: "<tier and the non-goals from section 4>"?
   Existing capabilities that matter: <from CONTEXT.md, if present>.
4. Viability: cost to run and maintain, support burden, data/permission exposure,
   consistent with <pricing/packaging reality if relevant>.
5. Strategy fit: consistent with <product principles / non-goals: quote them>.

Disqualifiers (an idea violating these loses regardless of other scores):
- <non-goals from section 4>
- <product principles that act as hard rules>
```

Anchoring means quoting the ticket, not paraphrasing generic product theory. A judge should be able to score every dimension by pointing at ticket text.

## Judging protocol

- Each judge receives: the fitness spec file, the idea files (neutral filenames: idea-a.md, idea-b.md, randomized order per judge). Nothing else. No round number, no history, no authorship.
- Judgments are pairwise: for each pair, per dimension, which idea wins and one line why; then an overall winner for the pair.
- Every pairwise verdict must include: "Flip-assumption: if <assumption> is false, the verdict reverses." Vague flip-assumptions ("if users don't like it") are rejected; a flip-assumption names something testable.
- Use three judges per round when subagents are available (majority per pair); one careful judge pass otherwise, with pairs judged in randomized order.
- Aggregate to a ranking by pairwise win count (Borda-style). Record the margin; margins of one pairwise win are near-ties and must be reported as such.

## Round bookkeeping (kept by the orchestrator, never shown to judges)

Per round: the ranking with margins, survivors and why (including diversity-guard overrides), operators applied, flip-assumptions harvested. This log becomes the transparency appendix of the final report.

## Termination checklist, evaluated after every round

- Leader unchanged for two consecutive rounds AND at least one of those rounds included a challenge idea aimed at it → stop, stable.
- Top two or three flip order between rounds with one-win margins → stop, indistinguishable; report finalists as a near-tie with the assumption that separates them.
- Five rounds reached → stop, cap; say the cap was hit and what was still moving.
```
