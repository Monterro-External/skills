# Verdicts, precedence, confidence

The yardstick for `ask-product`. All three sets are closed: a label outside them makes the log unanalysable.

## Verdicts

| Verdict | Means | The answer must also give |
|---|---|---|
| `as-designed` | Documented, intended behaviour | The source that documents it |
| `as-designed-but-confusing` | Works as intended, docs or UI mislead | Both what is true and why it reads wrong |
| `config-dependent` | Turns on role, permission, plan or account setting | The exact setting or permission to check |
| `not-supported` | The capability does not exist | Whether a documented workaround exists, and that "not documented" is not "not possible" |
| `likely-bug` | Behaviour contradicts the source of truth | The source it contradicts, plus a reproduction skeleton |
| `unknown-gap` | The sources do not cover it | Who owns the area, and what would need to be written down |

`as-designed-but-confusing` is the label most teams omit and the one that pays for the skill: a usability pain hypothesis arriving pre-labelled, ready for the synthesis reports and discovery.

`not-supported` requires a source that says so or an exhaustive read of the tier that would carry it. Sources merely silent means `unknown-gap`.

## Source precedence

1. **Behaviour evidence the asker supplied.** What is actually happening.
2. **CONTEXT.md, decision records, tickets past discovery.** What was intended.
3. **Release notes.** What changed, and when.
4. **User documentation.** What customers were told.
5. **Known issues on the board.** Whether this is already recognised.
6. **Support queue, request channel, CSM notes, synthesis reports.** Corroborating only, never the sole basis for a verdict.

A tier disagreeing with a higher tier resolves toward the higher one, and sets `gap_flag: conflicting-sources` whatever the verdict. The help centre saying A while the decision records says B is a documentation defect worth reporting even when the asker did not care.

**Two sources in the same tier can disagree, and tier alone does not settle it.** Tier 2 holds the reference file, the decision records, the roadmap page and the tickets, and the reference file is a summary of the other three. Resolve within a tier in this order: the record beats the summary of it, the more specific beats the more general, and the newer beats the older. A reference file that summarises a decision wrongly is the summary being wrong, never the decision. This fires more often than a cross-tier conflict, because a summary drifts from what it summarises every quarter it is not reviewed.

**Name the owner of the losing document.** `conflicting-sources` is a defect in a named file and it needs a human whatever the verdict is: put that document's owner in `escalated_to`, the same as `likely-bug` and `unknown-gap` do. Otherwise a conflict found on an `as-designed` or `not-supported` answer waits for a monthly review that may not happen, which is the silent failure this skill exists to prevent.

## Confidence

Derived, never felt. Start from the best supporting tier, then apply every cap that fires.

| Input | Effect |
|---|---|
| Best source in tier 1 to 3 | May reach `high` |
| Best source in tier 4 | Caps at `medium` |
| Tier 6 alone | Caps at `low` |
| Every supporting source predates the newest release touching its product area | Caps at `medium` |
| A second independent tier agrees, and no cap above fired | Raises `medium` to `high` |
| A tier contradicts another | Caps at `medium`, sets `conflicting-sources` |
| A configured source was missing or unreachable | Caps at `medium`, and the answer names it |

Stale documentation is the most common source of confidently wrong answers, which is why freshness caps rather than merely informs. The cap is on the answer, not on each source: one supporting source that postdates the release clears it, which is the usual case when an open ticket on the board says the same thing as an older reference file. A cap that has fired cannot be lifted by corroboration.
