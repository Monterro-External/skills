---
name: setup
disable-model-invocation: true
description: Configure a product repo for the Monterro product-side skills. Run once per repo, or again to change an answer. Use when someone asks to set up, install or configure the Monterro skills, the product workflow, or the ticket home, or when another monterro skill reports that .claude/monterro.json is missing.
---

# setup

Asks seven questions, writes `.claude/monterro.json`, and creates the files the other skills need if they do not exist. Every other monterro skill reads this file instead of guessing.

## 1. Look before asking

Check the repo for existing answers: `tickets/` or similar, `TEMPLATE-*.md` files, a `CONTEXT.md`, a `package.json` validate script, and a connected Linear or Jira MCP. Use what you find as defaults.

If `.claude/monterro.json` already exists, the person is re-running setup to change something: **always ask**, with the current values as defaults, and say which values changed at the end. Never finish silently because the config looked complete.

## 2. Ask, in one round

Use the structured question tool if available. Defaults in brackets.

1. **Ticket home**: markdown files in this repo, Linear, or Jira? [files]
2. If Linear or Jira: team or project, and whether tier templates exist there.
3. **Tickets folder and id prefix**: [tickets/, prefix from the repo name, e.g. BC]
4. **Reference file**: path to the CONTEXT.md every ticket is checked against [strategy/CONTEXT.md]
5. **Engineering glossary**: path to the engineering repo's domain glossary, relative to this repo, or none [../<engineering-repo>/docs/domain-glossary.md]
6. **Validator**: a command that checks one ticket, or none [npm run validate -- {id} --strict if package.json has a validate script]
7. **Research folders**: where interview rounds live, one folder per round with the round's README, a `transcripts/` subfolder for the untouched recordings and a `notes/` subfolder for the write-ups, and where synthesis reports go [discovery/interviews, discovery/synthesis]
8. **Signal folders**: the signals root (with `nps/`, `reviews/`, `member-feedback/`, `sales/`, `csm/`, `escalations/` under it), where the support queue's `tickets.jsonl` lives, where the request-channel export lives, where the decision log is, and where release notes go [signals, signals/support, signals/slack, roadmap/decisions.md, releases/release-notes]

## 3. Write the config

```json
{
  "ticket_home": "files",
  "tracker": null,
  "tickets_dir": "tickets",
  "id_prefix": "BC",
  "context_file": "strategy/CONTEXT.md",
  "templates": { "S": "tickets/TEMPLATE-S.md", "M": "tickets/TEMPLATE-M.md", "L": "tickets/TEMPLATE-L.md" },
  "glossary": "../beefcake-platform/docs/domain-glossary.md",
  "validator": "npm run validate -- {id} --strict",
  "interviews": "discovery/interviews",
  "synthesis": "discovery/synthesis",
  "cohorts": "discovery/usage-cohorts",
  "signals": "signals",
  "support": "signals/support",
  "requests": "signals/slack",
  "decisions": "roadmap/decisions.md",
  "releases": "releases/release-notes"
}
```

`tracker` when not files: `{ "kind": "linear", "team": "...", "templates": { "S": "...", "M": "...", "L": "..." } }`.

## 4. Scaffold what is missing

Only create, never overwrite:

- `tickets/` with `TEMPLATE-S.md`, `-M.md`, `-L.md` copied from `references/templates/` in this plugin, and a `README.md` stating the conventions (one file per ticket, `PREFIX-nnnn-slug.md`, id in frontmatter matches filename, statuses).
- `strategy/CONTEXT.md` as a stub with the section headings the grill reads (what we sell and to whom, strategy, principles, non-goals, product areas, vocabulary, what we already ship) and a first line saying it is a stub the CPO owns.

Say what was created and what was found.

## 5. Close

Three lines: the config path, which skills are now usable, and the one thing still to do by hand if any (for example: get read access to the engineering repo so the glossary path resolves).
