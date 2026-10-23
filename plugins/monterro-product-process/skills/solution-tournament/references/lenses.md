# Seeding lenses and evolution operators

## Seeding lenses

One idea per lens; skip a lens only when it is genuinely inapplicable and say so. The point is structural diversity: these lenses fail in different ways, so their ideas differ in kind, not in wording.

1. **Configure, don't build.** An existing tool, platform feature, or vendor already does this or close to it. Name the actual mechanism and check it against the acceptance scenarios.
2. **Eliminate the task.** Instead of doing the task better, make it unnecessary. What upstream change removes the need?
3. **Process, not software.** A working-agreement, checklist, or ritual change that reaches the outcome with zero build.
4. **Borrowed pattern.** How does a different domain solve the structurally identical problem? Import the pattern, adapted.
5. **The minimal version.** The smallest mechanism that could move the outcome metric at all; strip until removing more breaks it.
6. **The generous version.** If the ticket were a tier larger, what becomes possible, then cut it back to this tier and see what survives.
7. **(Seeded competitor) The ticket's own leading candidate**, restated fairly in the same one-paragraph spec format as the others.

## Idea spec format (identical for every idea, so judging compares substance)

```markdown
# <neutral id>
Mechanism: <what exists or happens, one or two sentences>
Who does what: <the persona's new behavior, and any admin/setup>
What changes: <for the user, for the team, for the system>
Honest cost: <build/configure effort class, run cost, main dependency>
```

## Evolution operators

- **Refine**: take one survivor and fix the specific weakness the judges named (quote it). No general polishing; one weakness, one fix.
- **Hybridize**: take two survivors and combine the mechanism of one with the strength of the other. A hybrid must be a coherent single idea, not a feature list of both.
- **Challenge**: write a new idea whose sole design brief is: beat the current leader on its weakest dimension while staying within the disqualifiers. The challenger may reuse any lens. This is the anti-local-maximum device; every round includes at least one.

## Diversity guard, applied at selection

Two ideas are variants if they share the same core mechanism and differ in parameters (channel, threshold, audience). If the top ranks are variants: keep the best variant plus the highest-ranked non-variant, and record the override in the round log. Difference is kept alive deliberately, not by luck.
