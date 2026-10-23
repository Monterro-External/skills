---
name: setup
disable-model-invocation: true
description: Configure a product repo for the Monterro skills by writing .claude/monterro.json. Run once per repo, or again to change an answer.
---

# setup

Asks a few questions, writes `.claude/monterro.json`, and creates the files the other skills need if they do not exist. Every other monterro skill reads this file instead of guessing.

## 1. Look before asking

Check the repo for existing answers: `TEMPLATE-*.md` files, a `CONTEXT.md`, and a connected Linear or Jira MCP. Use what you find as defaults. Tickets always live in Linear or Jira; if neither MCP is connected, say so in one sentence and stop, since question 2 reads the tracker.

If `.claude/monterro.json` already exists, the person is re-running setup to change something: **always ask**, with the current values as defaults, and say which values changed at the end. Never finish silently because the config looked complete.

## 2. Ask, in one round

Use the structured question tool if available. Defaults in brackets.

1. **Tracker**: ask exactly "Which tracker do product tickets live in?" with the options Linear or Jira [the connected one]
2. If Linear: the team. If Jira: the site and the project key. Then read the project's issue types and statuses through the MCP (the transitions of any issue in it list every status) and propose a mapping from the standard's states to them, by name, for the person to confirm. Statuses the project lacks are mapped to null and named in the close as the thing to add in Jira.
3. **Ticket templates**: the S, M and L templates every ticket is written in. Default to the `TEMPLATE-*.md` files found in step 1. If none were found, say so and offer: set them up now from the standard templates bundled with this plugin (default), point at existing templates by path, or skip. If the person skips, warn that idea-to-ticket and signal-triage stop until templates exist.
4. **Reference file**: path to the CONTEXT.md every ticket is checked against [strategy/CONTEXT.md]
5. **Engineering glossary**: path to the engineering repo's domain glossary, relative to this repo, or none [../<engineering-repo>/docs/domain-glossary.md]
6. **Research folders**: where interview rounds live, one folder per round with the round's README, a `transcripts/` subfolder for the untouched recordings and a `notes/` subfolder for the write-ups, where synthesis reports go, and where usage-cohort analyses go [discovery/interviews, discovery/synthesis, discovery/usage-cohorts]
7. **Signal channels**: the signals root, then for each channel the folder its export lives in, or none when the company has no such channel: NPS, reviews, in-product feedback, sales calls and lost deals, CSM notes, escalations. Then the support queue export (any format: JSONL, CSV, or one file per ticket) and, if the repo has one, a command that queries it; the request-channel export; where release notes go [signals, signals/<channel> for each channel found in step 1 and none for the rest, signals/support, no command, signals/requests, releases/release-notes]
8. **Decision records**: the folder for Product Decision Records, product decisions that bind beyond one ticket, one file each [strategy/decisions]
9. **Optional, for ask-product**: where the user documentation lives, and where the question log goes [none, signals/ask-log.jsonl]
10. **Optional, own ticket standard**: path to the company's own product ticket standard, if it has one. The skills carry the Monterro standard's checks and use them when this is none [none]

## 3. Write the config

```json
{
  "tracker": { "kind": "linear", "team": "...", "tier_label_group": "tier" },
  "context_file": "strategy/CONTEXT.md",
  "templates": { "S": "templates/TEMPLATE-S.md", "M": "templates/TEMPLATE-M.md", "L": "templates/TEMPLATE-L.md" },
  "glossary": "../<engineering-repo>/docs/domain-glossary.md",
  "standard": null,
  "interviews": "discovery/interviews",
  "synthesis": "discovery/synthesis",
  "cohorts": "discovery/usage-cohorts",
  "signals": "signals",
  "channels": {
    "nps": "signals/nps",
    "reviews": "signals/reviews",
    "in_product": null,
    "sales": "signals/sales",
    "csm": "signals/csm",
    "escalations": null
  },
  "support": "signals/support",
  "support_cli": null,
  "requests": "signals/requests",
  "decisions": "strategy/decisions",
  "releases": "releases/release-notes",
  "docs": null,
  "ask_log": "signals/ask-log.jsonl"
}
```

A `channels` value of null means the company has no such channel; the skills that read it say so in their report and carry on. `support_cli` is a command prefix such as `npm run support --` when the repo has a query script for the queue; null means the skills read the export files directly. `standard` null means the bundled checks are the standard.

`tracker` for Linear is the shape above. Tier is a label inside that group, never the tracker's estimate field, so setup asks nothing about estimates. For Jira:

```json
{
  "kind": "jira",
  "site": "example.atlassian.net",
  "project": "KEY",
  "issue_types": { "bug": "Bug", "feature": "Feature", "initiative": "Epic" },
  "tier_label_prefix": "tier-",
  "area": "label",
  "states": {
    "triage": "Triage",
    "backlog": "Backlog",
    "todo": null,
    "discovery": "In Discovery",
    "ready-for-handover": "In Handover",
    "in-development": "In Development",
    "done": "Done",
    "canceled": "Canceled"
  }
}
```

The values are the project's exact names. `area` is `label` (`area-<value>`) unless the project uses components for product areas; team-managed projects have no components. A state mapped to null is one the project lacks: the skills fall back to the closest status and say so. `triage` null stops signal-triage reading a tracker queue; it then works from the synthesis reports alone and says so.

If an existing config has `tier_field` or `tier_values`, drop them, say tier now lives in labels, and offer to add the tier label to open tickets in the team from their current estimate. If it has `ticket_home`, `tickets_dir`, `id_prefix` or `validator`, drop them and say tickets now live only in the tracker; an existing `tracker: null` means asking question 1 again. Ticket files in the repo are left alone and named in the close, for the person to move into the tracker.

## 4. Scaffold what is missing

Only create, never overwrite:

- `templates/` with `TEMPLATE-S.md`, `-M.md`, `-L.md` copied from `references/templates/` in this plugin when the person chose to set them up. The frontmatter names the tracker fields; the skills write the sections into the issue description.
- `strategy/CONTEXT.md` as a stub with the section headings the grill reads (what we sell and to whom, strategy, principles, non-goals, product areas, customer segments with each one's ARR share, its key accounts and how it tends to raise problems, a vocabulary section that only points at the glossary, what we already ship) and a first line saying it is a stub the CPO owns. The synthesis skills read the segments section instead of assuming anything about the company's customers.
- `strategy/decisions/README.md`, the PDR index, with one line saying the folder holds one `PDR-nnnn-slug.md` per decision and that only a human sets a PDR to `accepted`.

If an old single-file decision log exists (for example `roadmap/decisions.md`), say so and offer to split it: one PDR per entry that meets the bar in `references/pdr.md`, status `accepted` with the entry's original date, since those were decided already. Entries below the bar are listed back, not converted. Never delete the old file; the person does that.

Say what was created and what was found.

## 5. Close

Before closing, check that every path in `templates` resolves. A missing one is the thing still to do by hand.

Three lines: the config path, which skills are now usable, and the one thing still to do by hand if any (for example: get read access to the engineering repo so the glossary path resolves).
