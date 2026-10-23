# Ask log schema

One flat JSON object per invocation, appended as a line to `ask_log` (default `signals/ask-log.jsonl`). Append-only. Never rewrite an earlier line except to fill the review fields.

| Field | Type | Why it is there |
|---|---|---|
| `id` | string | `ask-YYYYMMDD-nnn`, unique per record |
| `ts` | ISO 8601 | When it was asked, read from the clock at write time, to the second |
| `skill_version` | string | Ties answer quality to a version of this skill. The plugin's `version` from `plugin.json`, or `unversioned` outside the plugin |
| `config_version` | string | The `.claude/monterro.json` revision that produced it |
| `asker` | string | Name or handle, internal only |
| `asker_role` | enum | `support`, `sales`, `cs`, `engineering`, `product`, `other`. Sales and support asking the same thing are two different problems |
| `product_area` | string | From CONTEXT.md's taxonomy. The unit the product team plans against |
| `question_raw` | string | The asker's own words, personal data stripped, accounts as account ids |
| `question_normalized` | string | Canonical phrasing, so questions cluster |
| `verdict` | enum or null | The closed set in `verdicts.md`. `null` on a refusal |
| `refused` | enum or null | `live-state`, `commitment`, or null. Why no verdict was reached |
| `confidence` | enum | `high`, `medium`, `low` |
| `sources_used` | array | `{uri, tier, label, last_updated}`. Shows which sources carry the load and which are dead weight |
| `answer_summary` | string | Two sentences maximum, not the full answer |
| `gap_flag` | enum | `none`, `missing-doc`, `stale-doc`, `conflicting-sources`, `no-source` |
| `clarification_asked` | boolean | A high rate means the sources lack a dimension, usually plan or role |
| `escalated_to` | string or null | The human named for `likely-bug` and `unknown-gap`, and for any `conflicting-sources`: there it is the owner of the document that lost |
| `ticket_ref` | string or null | Filled in when a human turns the record into a ticket. Closes the loop |
| `review_status` | enum | `unreviewed`, `reviewed`. Set by the product team |
| `review_verdict` | enum or null | `correct`, `wrong`, `incomplete`. The only real accuracy number |
| `log_fallback` | boolean | `true` only on a record written to `.claude/ask-log-fallback.jsonl`. Omitted otherwise. Survives replay, so a replayed record is never mistaken for one the log took first time |
| `log_fallback_reason` | string or null | Why the append failed, in the words the failure gave. Present only with `log_fallback` |

A refused record carries `question_raw`, `question_normalized`, `product_area`, `asker_role`, `refused`, and empty `sources_used`. Counting them monthly is how the product team learns that sales is asking for dates weekly, which is a fact about the company rather than about the documentation.

`question_normalized`, `product_area` and `gap_flag` are the three fields that make the monthly review produce work. Raw question text alone does not cluster.

## Example

```json
{"id":"ask-20260922-001","ts":"2026-09-22T09:14:00Z","skill_version":"0.4.1","config_version":"2026-09-08","asker":"a.support","asker_role":"support","product_area":"payments","question_raw":"Customer says an end user was charged twice after a failed payment. Expected?","question_normalized":"duplicate charge after failed payment retry","verdict":"likely-bug","confidence":"medium","sources_used":[{"uri":"strategy/decisions/PDR-0004-retry-policy.md","tier":2,"label":"Retry policy decision","last_updated":"2026-05-11"}],"answer_summary":"PDR-0004 specifies one retry after 48 hours. Two charges inside a day contradicts it.","gap_flag":"none","clarification_asked":false,"escalated_to":"payments area owner","ticket_ref":null,"review_status":"unreviewed","review_verdict":null}
```

## The review loop this feeds

Weekly, 20 minutes: `likely-bug` into tickets, `unknown-gap` and `missing-doc` into the documentation backlog. Both are already written up. Monthly: cluster `question_normalized`; any cluster answered mostly from tier 4 or from stale sources earns a canonical answer written properly. Repeated `as-designed-but-confusing` in one area is a pain hypothesis for discovery.

**Replay is part of the ritual, not a thing someone remembers.** The weekly pass starts by
checking whether `.claude/ask-log-fallback.jsonl` exists; if it does, its lines are appended to
`ask_log` keeping `log_fallback` and `log_fallback_reason`, and the file is removed. A fallback
that is never replayed is the quiet failure the fallback was built to prevent, moved one file
along.

Without the ritual the log is dead weight, and the skill stops improving.
