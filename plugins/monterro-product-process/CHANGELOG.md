# Changelog

Newest first. A **Re-run setup** line means repos configured on an earlier version need `/monterro-product-process:setup` again.

## 0.15.2, 2026-11-24

- **The readiness gate no longer fails on sources** (product ticket standard v1.17). Check 2 never fails: a claim without a source, or a named source without a link, is one line for engineering in the verdict and never comes up in the fix walk. Engineering can still send a ticket back with `insufficient-evidence`. Nothing to re-run.
- **advance-ticket's evidence stage never blocks.** Unlinked or missing sources become a footnote offering to find them, and evidence gathering is the next action only when the PM asks. The S template in the beefcake-product repo says the same.

## 0.15.1, 2026-11-24

- **The readiness gate no longer asks for a second source.** Check 11 does not grade principles about how much evidence a decision needs, such as "two independent sources or it stays a hypothesis". Check 2 still asks that every claim is linked; a ticket resting on one linked source passes. The synthesis skills keep using the principle. Nothing to re-run.

## 0.15.0, 2026-11-24

- **The readiness gate fixes the ticket with the PM, one check at a time.** After a one-line count, product-grill-with-docs walks each failing check in its own question: why it fails, the exact replacement text, and apply, an alternative fix, or skip. Approved fixes are written to the ticket section by section, and every check runs again on the edited ticket before the status moves. The gate still never comments on the ticket.
- **The verdict is plain markdown at the product altitude.** No code block, no source files or tables in the reasons, passing checks named only by count. Notes split into **Before handover** (at most three, what the PM must act on) and one line each **For engineering / CPO**.
- **idea-to-ticket asks one question per message** (0.14.2). Compound question-bank entries are topics: ask the first part, infer the rest as a guess for the review. Nothing to re-run.

## 0.14.1, 2026-11-17

- **Report ceilings aligned at 1800 words.** feedback-synthesis, sales-synthesis and interview-synthesis go from 1200, account-synthesis from 1400 and synthesis from 1500, matching support-synthesis. The demotion rule over the ceiling is unchanged. Nothing to re-run.

## 0.14.0, 2026-11-13

- **A design reference no longer states its precedence** (product ticket standard v1.16). A reference that says nothing is `illustrative`; only `binding` is written, with what was validated, with whom and when. The grill's check 9 passes a reference with no precedence and still fails an unearned `binding`. The setup templates, and the idea-to-ticket and signal-triage checklists, drop the precedence word. prototype-ticket writes `Design: <canvas link> (Variant <X> · <idea>)` and, when a component is needed, the `Needs component` line, nothing else. Repos that scaffolded templates before this keep their old hint until they copy the new ones; tickets that still say `precedence: illustrative` keep passing.

## 0.13.1, 2026-11-13

- **prototype-ticket keeps rejected variants off the ticket.** The reference line names the chosen variant only. Why the others lost stays on the canvas, in the Rejected row, where an M ticket's PDR can still find the alternative. Nothing to re-run.

## 0.13.0, 2026-11-13

**Re-run setup** to use prototype-ticket. The new question is optional; every other skill works without it.

- **prototype-ticket, new.** Prototypes one ticket on a Claude Design canvas built from the company's design system: a `Current` artboard from the code and two or three variants from the real components, each with its AS-3 note. Nothing is invented: what the ticket and the code do not say shows as `[OPEN: ...]`, and a component the system lacks as `NEEDS COMPONENT · <slug>`. Rerun with the same ID to iterate; `component <slug>` draws a proposal for the design system's owner, with every state from existing tokens and a proposed prop, and never adds it to the system; `pick <variant>` moves the rest to a Rejected row and, after the PM's yes, writes the reference line to the ticket, `illustrative` unless a user validation is named. It never changes state, labels or comments. Keeps its working notes in `prototypes/<ticket-id>.md`. Run end to end on BCPRO-143.
- **Setup asks for the design system.** Question 11, optional: the Claude Design System artifact the prototypes are built from and the engineering checkout whose screens they start from, written to `design_system` in `.claude/monterro.json`. A company without one runs `/design-sync` in Claude Code from its engineering repo first; the system then follows the code, so a token or component changes in the code and `/design-sync` runs again.

## 0.12.0, 2026-11-12

- **brainstorm-ideas, solution-tournament, handover-triage and spec-review removed.** Handover is now an engineer's step: they read the ticket at `ready-for-handover`, pick the grilling or wayfinder route and move it to `in-development`, or send it back to `discovery` with `needs-more-info` and a reason code (product ticket standard v1.15). The grill's closing line now hands the ticket to engineering instead of pointing at handover-triage. The engineering side is Matt Pocock's published skills with nothing of ours wrapped around them. Reinstall or update the plugin; nothing to re-run in setup.

## 0.11.4, 2026-11-04

- **support-synthesis batches run on Haiku.** Each batch subagent is started on the Haiku model instead of inheriting the session model. The batch job reads and tags one ticket at a time; clustering and the report stay on the session model. Not yet evaluated.

## 0.11.3, 2026-10-26

- **grill-preview is also the intake size check.** Run it on a new ticket to see, from the code, whether the tier is right and whether the ticket is too big. The report sizes and buckets questions; nothing technical goes on the ticket. Only the description changed.

## 0.11.2, 2026-10-26

- **The grill reminds about proposed PDRs.** A PDR the ticket points to that is still `proposed` is listed under Notes as "accept before handover", with its owner. Nothing in the flow sets `accepted`, and a proposed PDR binds nothing. It never fails the ticket.

## 0.11.1, 2026-10-26

- **support-synthesis reads a large queue in batches.** One month per batch, halved when a month will not fit, each read in full by its own subagent (or in sequence, appending to the table) and returning rows only. A short batch is rerun before clustering. A timed run on a 454-ticket queue had fallen back to classifying most tickets by pattern match, which the skill forbids.

## 0.11.0, 2026-10-23

**Re-run setup.** Setup drops `ticket_home`, `tickets_dir`, `id_prefix` and `validator` from an existing config and points `templates` wherever they live; a repo moving its templates out of `tickets/` updates the three paths.

- **Tickets live only in Linear or Jira.** The markdown-file ticket home is gone: no skill reads, writes, searches or dedups ticket files, and setup no longer offers files as an answer. With no tracker connected, setup stops. When the tracker is unreachable, the reading skills still take pasted content.
- **Setup scaffolds templates into `templates/`**, without the file-ticket conventions README. The per-ticket validator is gone with the files; idea-to-ticket and signal-triage check the body against their checklist.

## 0.10.0, 2026-10-23

Reinstall: `/plugin uninstall monterro@monterro-product-process`, then `/plugin marketplace remove monterro-product-process`, `/plugin marketplace add Monterro-External/skills` and `/plugin install monterro-product-process@monterro-product`.

- **The plugin is renamed `monterro-product-process`.** Every skill is now invoked as `/monterro-product-process:<skill>`, and the plugin folder is `plugins/monterro-product-process/`. The marketplace is renamed `monterro-product`; its repo is `Monterro-External/skills`. The config file stays `.claude/monterro.json`, so configured repos need no change.

## 0.9.1, 2026-10-14

- **grill-preview splits vertically only.** A cluster that touches no acceptance scenario is engineering-internal: it is folded into the clusters that depend on it, does not count toward a too-big verdict, and is never proposed as a ticket. Every proposed piece carries at least one acceptance scenario of its own. Folded work shared by several pieces counts as a shared item for the split's confidence.

## 0.9.0, 2026-10-13

Repos set up on an earlier version: add the **Proposed terms** section to the end of each `TEMPLATE-*.md` by hand, or copy it from `setup/references/templates/`.

- **Proposed terms**, an optional last section in every template, per ticket standard v1.14: one bullet per domain term engineering's glossary lacks, with what it means in this ticket.
- **The grill fails an undefined new term.** Check 12 used to note a term the glossary lacked and pass; it now passes only when the ticket defines the term under Proposed terms, and still notes it as a glossary gap for engineering. A synonym for a glossary term, or a new meaning for one, fails as before.
- **idea-to-ticket writes the definition** with the PM's meaning, or marks it a guess in the review. **signal-triage lists glossary gaps** in its closing note and leaves the definition to discovery.

## 0.8.1, 2026-10-13

- **grill-preview judges size.** It groups the question tree into clusters, lists the size signals with evidence (clusters, scenarios that split along them, several surfaces, a second verb in the Intent, a would-be ADR, tier mismatch), and gives a size verdict: right-sized, too big or tier mismatch. A too-big ticket gets a drafted split, one ticket per cluster, whose confidence comes from the structure: high when nothing is shared across the cut, medium when one decision must be settled first, low when no clean cut exists, in which case it names the scope to cut instead. Drafts only; nothing is created.

## 0.8.0, 2026-10-13

- **grill-preview**, a new skill. A dry run of the engineering grill on one ticket: it builds the question tree grill-with-docs would walk against the engineering repo, answers each question with the recommended answer, and sorts every question into product, engineering or boundary with the reason. One self-contained HTML report at `grill-preview/<ticket-id>.html` shows the questions, the facts it looked up instead of asking, and what domain-modeling would have written. Nothing is asked and the ticket is not touched. No time estimate, by design.

## 0.7.0, 2026-10-13

**Re-run setup** for a repo whose tickets live in Jira.

- **Jira as a ticket home.** The tracker mapping covers Jira field by field: issue types, `tier-S/M/L` and `area-<value>` labels, statuses from `tracker.states`, JQL for duplicates and the triage queue. setup reads the project's statuses and proposes the mapping; a state the project lacks is null and named as the thing to add.
- **Status moves by transition in Jira**, matched on the target status, never the transition's name. The grill and handover-triage say so.
- **One tracker per session, by config.** The skills use only the connector `tracker.kind` names, so a session with both Linear and Jira connected never finds a same-keyed ticket in the wrong one.
- **Screenshots are attached by hand in Jira**: the MCP cannot upload files, so the closing note names the attachment.

## 0.6.0, 2026-10-06

**Re-run setup.** The config gains `channels`, `support_cli` and `standard`; `decisions` is now a folder; tier moves from the tracker's estimate to a label.

- **Works on any product, not just the sample company.** No skill assumes a repo layout, a support script, a ticket standard file or company facts. Segments, ARR shares and key accounts are read from CONTEXT.md, whose stub now has a section for them.
- **Product Decision Records.** A decision that binds beyond its ticket is written as a file in `strategy/decisions/` (`PDR-nnnn-slug.md`), and the ticket keeps a `See PDR-nnnn` line. idea-to-ticket and advance-ticket write them as `proposed`; only a person sets `accepted`. The folder replaces the single-file decision log for every skill that read it, and setup offers to split an old log.
- **Tier is a label.** `S`, `M` or `L` in a `tier` label group in Linear, `tier-S/M/L` in Jira. The estimate field is left to engineering.
- **Setup asks for ticket templates** and offers to install the bundled ones, so the first ticket no longer fails for want of a template. It asks "Where should product tickets live?".
- **Feedback channels are configured one by one** (NPS, reviews, in-product, sales, CSM, escalations), any of them absent. The support queue is read from its export files; `support_cli` keeps a query script where a repo has one.
- **The grill and advance-ticket answer in the conversation** and never comment on the ticket. The grill still moves the state and the `needs-more-info` label.
- **The product side stays out of the code.** advance-ticket and idea-to-ticket never read source code or write a root-cause hypothesis; that is engineering's after handover.
- **No second source required.** idea-to-ticket no longer asks who else sees a problem; S tickets take at most three questions.
- The grill checks against the bundled rules (product ticket standard v1.13) unless `standard` names the company's own.
