---
name: grill-preview
disable-model-invocation: true
description: Dry-run the engineering grill on one ticket without asking anything or touching the ticket. Projects the question tree grill-with-docs would walk, sorts every question into product, engineering or boundary, and writes one self-contained HTML report. Use before the readiness gate or before handover to see whether the grill will take minutes or hours.
allowed-tools: Read, Grep, Glob, Write(grill-preview/**)
---

# grill-preview

A rehearsal of the engineering grill (`grill-with-docs`: grilling plus domain-modeling) on one ticket. It builds the same design tree the grill would build, answers its own questions with the recommended answer the grill would offer, and reports what it found. Nobody is asked anything.

Three rules:

- **Read only.** Never change the ticket, its status, labels or comments, and never write ADRs, glossary entries or CONTEXT.md. The one file this skill writes is the report.
- **Show every question.** Each one appears in the report with its bucket and the reason for the bucket. A count without the questions behind it can't be argued with, and arguing with the bucketing is how the classifying rule gets better.
- **Never estimate time.** Report rounds and decisions. Minutes per question is a calibration the team makes against real sessions, not something this skill invents.

## 1. Load

From `.claude/monterro.json`: the tracker block, or `tickets_dir` when tickets are files; `context_file`; `decisions`; `glossary`. The engineering repo is the repository `glossary` points into: read its `CLAUDE.md` or `AGENTS.md`, `docs/adr/`, the glossary, and the code the ticket touches. If the config is missing, say so in one sentence and stop.

Fetch the ticket through the read tools of `tracker.kind` only, by ID or link, or take pasted content. One ticket per run.

## 2. Build the tree

Run the `grilling` method from the engineer's chair, against the engineering repo, as `grill-with-docs` would:

- **Facts are looked up, never asked.** Anything the code, the ADRs, the glossary or CONTEXT.md answers is resolved by reading it and listed under Facts with its source (`path:line`). A fact that changes the ticket's meaning (an acceptance scenario that cannot occur, a claim the code contradicts) also becomes a question.
- **Round 1 is the frontier**: every decision whose prerequisites are settled by the ticket or by a fact.
- **Later rounds are projected.** Assume the recommended answer to each earlier question and recompute the frontier. Mark these rounds as projected: a real answer can reshape them.
- Stop when the frontier is empty. Also record what domain-modeling would write: an ADR only where the decision is hard to reverse, surprising, and a real trade-off; a glossary entry where the ticket uses a term the glossary lacks or uses differently.

For each question record: round, a short title, the question as the grill would put it, the recommended answer, confidence (`high` the docs or ticket nearly settle it, `medium` a defensible default, `low` a guess), the sources relied on, and, for a later round, the question numbers it depends on.

## 3. Bucket every question

One rule, applied per question:

- **product**: the answer changes what a user sees or can do, or what an acceptance scenario says. The ticket should have settled it.
- **engineering**: the answer changes only how the system does it. This is what the grill is for.
- **boundary**: both are true (visible error states, ordering and collation rules, thresholds, retention, permissions, vocabulary that is a UI label as well as a domain term), or the ticket settles it in a way the code makes questionable.

Give the one-line reason for the bucket. For a product or boundary question, name the ticket section it belongs in and the check from `product-grill-with-docs/references/checks.md` it bears on, if any. Pick the bucket on the rule, not on who would enjoy answering it.

## 4. Verdict

One line, from the counts:

- No product questions: `Ready for the grill: only engineering questions remain.`
- Product questions, all `high` or `medium` confidence: `Ready with caveats: N product questions engineering will put to the PM.`
- Any `low`-confidence product question, or more than five product questions: `Back to the PM first: N product questions open.`

Add one sentence on what drives it.

## 5. Write the report

Read `references/report-template.html`, replace the placeholder `__GRILL_PREVIEW_DATA__` with the JSON below, and write it to `grill-preview/<ticket-id>.html` relative to the working directory. Write nothing else.

```json
{
  "ticket": { "id": "", "title": "", "url": "", "tier": "", "status": "", "area": "" },
  "generated": "YYYY-MM-DD",
  "engineering_repo": "",
  "verdict": { "label": "", "tone": "good | warn | bad", "why": "" },
  "facts": [ { "fact": "", "source": "" } ],
  "would_write": { "adrs": [ { "title": "", "why": "" } ], "glossary": [ { "term": "", "why": "" } ] },
  "notes": [ "" ],
  "questions": [
    {
      "n": 1, "round": 1, "projected": false, "depends_on": [],
      "title": "", "question": "", "recommended": "",
      "confidence": "high | medium | low",
      "bucket": "product | engineering | boundary",
      "bucket_reason": "", "ticket_section": "", "check": "",
      "sources": [ "" ]
    }
  ]
}
```

Then say in the conversation, in three lines at most: where the file is, the verdict line, and the counts per bucket.
