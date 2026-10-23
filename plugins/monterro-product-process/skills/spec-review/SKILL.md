---
name: spec-review
disable-model-invocation: true
description: Engineering side, after /implement. Review a change against its Spec and check that each requirement's test fails without the change. Posts a verdict, fixes nothing.
---

# spec-review

Step 9's check, after `/implement` and before merge. `/to-spec` grades the Spec against itself and `/implement` reports its own tests green; this skill is the reader who trusts neither. The failure it exists to catch is a suite that passes for the wrong reason: a test that proves a neighbour of the requirement, or one that would have passed before the change too.

Three rules:

- **Every finding names a check** from `references/checks.md`, with the smallest fix. Never "looks good" or "needs work".
- **A requirement without a proof that fails on the base commit is not proven**, whatever the suite says.
- **The reviewer does not fix.** No edit to the code, the tests, the Spec or any ticket body. The implementer writes.

## 1. Load

Tracker conventions from `docs/agents/issue-tracker.md` in this repo, which `/setup-matt-pocock-skills` writes. If it is missing, say so in one sentence and stop. The yardstick is `handover-artifact-standard.md`, sections 3 to 5; `references/checks.md` restates it and is the fallback when the standard is not in the session.

Then, one change per run:

- **The Spec issue**, by id or link, with its comments and sub-issues, or pasted content. Given none, take it from the branch name or the commit messages; more than one candidate, ask once.
- **The diff**: the branch or PR named, against its merge base with the default branch unless a base is given. Record both commit ids.
- **The Product Ticket** the Spec's first line links to, from the tracker or the file it names. Read it for tier, behaviour and acceptance scenarios, which check 1 holds the Spec to. This skill never writes to it.

## 2. Run the checks

Walk `references/checks.md` top to bottom and record pass, fail, skipped or not-applicable for each, with a one-line reason for anything but pass. Run all of them: the whole bill at once. Skipped counts as failing.

Checks 11 and 12 need the suite run twice. Run it at head. Then, in a throwaway worktree of the base commit, run each proof test file as it stands at head: the new test against the old code. Never check out the base in the working tree the engineer is using, and remove the worktree afterwards.

## 3. Verdict

All applicable checks pass: `pass`, and say the change can merge. Any fails or is skipped: `changes-needed`. Move no state either way; merging is a person's call.

A failure that traces past the Spec to the Product Ticket (a requirement faithful to a ticket that is itself wrong or silent) goes under Upstream, addressed to the ticket's owner. That is the signal a grill check needs tightening, so name the check if one fits.

```
Spec review: <spec id> <title>, tier <S/M/L>, base <sha> head <sha>
Verdict: pass | changes-needed
Coverage: <requirement id: proof test, fails on base yes/no, passes at head yes/no>  (one line each)
Failing: <check number and name: what fails, smallest fix>  (one line each, or "none")
Skipped: <check number: why>  (or "none")
Upstream: <what the Product Ticket got wrong, for its owner>  (or "none")
```

Post it as a comment on the Spec issue, addressed to the implementer. If the tracker is read-only, deliver it in the conversation and say it was not written to the issue.
