---
name: ask-product
disable-model-invocation: true
description: Answer one question about how the product works, from the product's own sources, with a verdict, a confidence and a citation on every claim, and log the exchange. Use when support, sales, CS or anyone else asks whether a behaviour is expected, whether something is supported, how a setting works, or whether what they are seeing is a bug. Reads only, files nothing, starts no other skill.
---

# ask-product

Anyone in the company asks how the product works; the answer comes back with a verdict, a confidence and a source for every claim. The one failure this skill exists to prevent is an answer composed from the model's priors and delivered with the product team's authority. **No verdict without a citation.** If nothing citable comes back, the verdict is `unknown-gap` and the answer says so. That is the most useful record this skill produces, because it names a documentation gap nobody had written down.

Paths come from `.claude/monterro.json`, read from the repo the skill is invoked in. The engineering repo does not have one, and running there is the likeliest way to reach the config-missing stop. Keys: `context_file` for product areas and non-goals, `glossary` for vocabulary when it resolves (a word list for reading the question and searching, never a precedence tier, and its absence caps nothing), `decisions`, `releases`, `support`, `signals`, `synthesis`, the tracker block for tickets, optional `docs` for user documentation and optional `ask_log` for the log (default `signals/ask-log.jsonl`). If the config is missing, say so in one sentence and stop. The taxonomy, the precedence order and the confidence rules are in `references/verdicts.md`; the log schema is in `references/log-schema.md`.

## 1. Take the question

Restate it in one line as what the asker saw or is trying to do, and name the `product_area` from CONTEXT.md's taxonomy. Note the asker's role if they gave it: support, sales, CS, engineering, product. The role changes the closing advice, never the verdict.

Clarify at most **once**, one question, and only when the verdict genuinely turns on it (plan tier, role, a screenshot) **and the answer cannot simply be enumerated**. Four payment methods by country is an answer, not a question. The cap is a ceiling, not a quota, and non-product people abandon a skill that interrogates them. Unanswered, answer the most likely reading, say which reading you took, and cap confidence at `medium`.

Refuse two kinds outright and route each to where the answer actually lives:

- **A specific member's or tenant's live state.** Not the corpus's to know. Route to the tenant's own records, or, for our own staff, the internal admin surface. Never route to "support" without checking who is asking; support is the most common asker.
- **A ship date or a commitment.** Route to the product team, which owns that conversation. Say what is sayable instead: what has passed the gate and what is being built first.

A refusal is logged like any other invocation, with `verdict: null` and the `refused` field naming which of the two fired. A rising rate of date questions is something the product team should know.

## 2. Read before you think

In precedence order, and all of it before composing anything:

1. Evidence the asker supplied: screenshot, log, recording, ticket id.
2. `context_file`, `decisions`, the roadmap page next to the decision log, and any ticket already past discovery covering the area. Read the roadmap for what is intended and sequenced; its dates are decision and review dates, never ship dates, and never leave in an answer as one.
3. `releases`, newest first. Dates matter here more than wording.
4. `docs`, the user documentation, if the config names one.
5. Known issues: open tickets on the board, through the tracker MCP.
6. Everything else: the support queue through `npm run support -- list --grep`, the request channel, CSM notes, the synthesis reports. Corroborating only, never the sole basis for a verdict.

Search by words the customer would use, in every language the queue holds, never by tag. Stop at the tier that settles it; do not read tier 6 to decorate an answer tier 2 already made.

If a source the config names is missing or unreachable, say which one in the answer and cap confidence per `references/verdicts.md`. A partial search presented as a complete one is how a wrong answer gets authority.

## 3. Rule

Pick the verdict from the closed set in `references/verdicts.md`, then derive confidence mechanically from the best supporting tier, the freshness of that source against the newest release touching the area, and whether a second independent tier agrees. Do not adjust it by feel.

Two sources disagreeing is itself a finding: resolve toward the higher tier, and set `conflicting-sources` whatever the verdict. Inside one tier, where tier settles nothing, the record beats the summary of it, the specific beats the general, and the newer beats the older; `references/verdicts.md` has the order. Name the owner of the losing document in `escalated_to` even when the verdict is not `likely-bug` or `unknown-gap`, because a documentation defect nobody is told about is not reported. `likely-bug` requires naming the source the behaviour contradicts; without one it is `unknown-gap`.

## 4. Answer

Short, plain, and the same shape every time:

```
Verdict: <label>   Confidence: <high|medium|low>

<One paragraph, the asker's own words, no jargon they did not use.>

What to do next: <support: the wording to send the customer. sales: what can and
cannot be claimed. engineering or product: the spec or ticket reference. Every
likely-bug and unknown-gap also names the human who owns the area.>

Sources: <one line each: path or id, tier, last updated>
Not read: <the configured source that was missing or unreachable, or omit>
```

`likely-bug` adds a reproduction skeleton: what was done, what happened, what the cited source says should happen. The skeleton is handed to a human. This skill files nothing and starts no other skill.

## 5. Never

- Invent a setting name, menu path, field label or version number. Not in a source, not in the answer.
- State a roadmap date or a commitment that is not a quoted public one.
- Call something a bug without naming what it contradicts.
- Answer from one tier 6 source alone.
- Say "not supported" where the sources are merely silent. Silence is `unknown-gap`.

## 6. Log, then close

Compose the answer first, then append one record per invocation to `ask_log` in the schema of `references/log-schema.md`. Strip customer names and personal data from `question_raw` at write time; an account goes in as its tenant id.

Two fields are read, never written from memory or copied from the schema's example. `ts` comes from the clock at write time, `date -u +%Y-%m-%dT%H:%M:%SZ`, so two records on one day can be ordered. `skill_version` is the `version` in `${CLAUDE_SKILL_DIR}/../../.claude-plugin/plugin.json`; if that file is not there, as in a source copy outside the plugin, write `unversioned` rather than guess.

If the log cannot be written, still answer, say in one line that it was not logged, and write the record to `.claude/ask-log-fallback.jsonl` with `log_fallback: true` and `log_fallback_reason` carrying the failure in the words it gave. The `Logged:` line then names the fallback path and says the primary was not written, so the asker can see it from the answer. A logging layer that fails quietly kills the feedback loop and nobody notices for a quarter, and a fallback nobody replays is the same failure one file along: the weekly review drains it, per `references/log-schema.md`.

Close in three lines:

```
Verdict: <label>   Confidence: <level>   Area: <product_area>
Gap: <none|missing-doc|stale-doc|conflicting-sources|no-source>
Logged: <path>, record <id>
```
